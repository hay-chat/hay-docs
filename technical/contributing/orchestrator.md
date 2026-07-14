---
layout: docs.njk
title: Orchestrator
description: Event-driven conversation processing pipeline (RabbitMQ + three layers)
section: technical
navGroup: Contributing
navOrder: 1
---

# Orchestrator Contributor Guide

The orchestrator turns an incoming customer message into a bot reply. It is **event-driven**: messages are enqueued to RabbitMQ and consumed by a worker that runs a single pipeline — Perception → Retrieval → Execution — with tool calling, human handoff, and confidence guardrails along the way.

This guide covers how the pieces fit together and where to make changes. For code-organization rules (repository vs. entity vs. service responsibilities), read [`server/orchestrator/ARCHITECTURE.md`](../../../server/orchestrator/ARCHITECTURE.md) — that document defines the layering conventions; this one describes the runtime flow.

> **Note:** Older docs described an 8-phase polling architecture with cooldowns, runtime agent routing, and "plan/ender" modules. That design is gone. Agents are assigned at conversation creation, cooldowns no longer gate processing (`Conversation.addMessage()` still writes `cooldown_until`, but only the disabled legacy polling path and the unused `findReadyForProcessing()` repository method read it), and processing is triggered by queue messages, not polling.

## File Map

| Path                                                                        | What lives there                                                                    |
| --------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| `server/orchestrator/run.ts`                                                | `runConversation()` — the main pipeline and the execution loop                      |
| `server/orchestrator/index.ts`                                              | `Orchestrator` class: `processConversation()`, `checkInactivity()`, legacy `loop()` |
| `server/orchestrator/perception.layer.ts`                                   | `PerceptionLayer` — intent / sentiment / language analysis                          |
| `server/orchestrator/retrieval.layer.ts`                                    | `RetrievalLayer` — playbook selection and vector document search                    |
| `server/orchestrator/execution.layer.ts`                                    | `ExecutionLayer` — planner LLM call, guardrails, prompt assembly                    |
| `server/orchestrator/conversation-utils.ts`                                 | Closure validation, title generation, inactivity warning/close helpers              |
| `server/orchestrator/types.ts`                                              | `ConversationContext`, `ProcessingPhase`, guardrail log types                       |
| `server/workers/orchestrator.worker.ts`                                     | `OrchestratorWorker` — RabbitMQ consumer, retry policy                              |
| `server/services/orchestrator-queue.service.ts`                             | Queue names, `enqueue()`/`enqueueRetry()`/`enqueueDead()`, message shape            |
| `server/services/rabbitmq.service.ts`                                       | amqplib wrapper: reconnect, publish, consume                                        |
| `server/services/scheduled-jobs.registry.ts`                                | Sweep, inactivity check, stale-message recovery jobs                                |
| `server/services/core/tool-execution.service.ts`                            | Executes planner tool calls                                                         |
| `server/services/core/confidence-guardrail.service.ts`                      | Stage 2 guardrail: fact grounding                                                   |
| `server/services/core/company-interest-guardrail.service.ts`                | Stage 1 guardrail: company interest                                                 |
| `server/prompts/en/{perception,retrieval,execution,conversation,playbook}/` | All LLM prompt templates (also `pt/`, `es/`)                                        |

## End-to-End Flow

```
customer message ──> Conversation.addMessage()
                       │  sets needs_processing = true
                       ▼
     orchestratorQueueService.enqueue(id, orgId, trigger)
                       │  publishes JSON to RabbitMQ
                       ▼
              orchestrator.process queue
                       │
                       ▼
  OrchestratorWorker.startConsuming()  (prefetch: 2)
                       │
                       ▼
  Orchestrator.processConversation() ──> runConversation()
                       │
        lock ─> perception ─> retrieval ─> execution loop
                       │
                       ▼
     bot message(s) via conversation.addMessage()
     (delivered to channels via message hooks / WebSocket)
```

### Enqueue triggers

`OrchestratorTrigger` (`server/services/orchestrator-queue.service.ts`) enumerates every reason a conversation gets processed:

| Trigger            | Fired from                                                                                            |
| ------------------ | ----------------------------------------------------------------------------------------------------- |
| `customer_message` | `Conversation.addMessage()` for customer messages (`server/database/entities/conversation.entity.ts`) |
| `creation`         | `server/services/conversation.service.ts` when a conversation is created                              |
| `ai_return`        | Conversation entity, when a human hands the conversation back to the AI                               |
| `recovery`         | `server/services/message-recovery.service.ts` (stale-message recovery)                                |
| `inactivity`       | `Orchestrator.checkInactivity()` after sending an inactivity warning                                  |
| `sweep`            | The `orchestrator-sweep` scheduled job (safety net, see below)                                        |

The queue message is an `OrchestratorMessage`: `{ messageId, conversationId, organizationId, trigger, timestamp, attempt }`.

## Queue Topology and Retries

Three durable queues, declared in `OrchestratorQueueService.declareQueues()`:

- **`orchestrator.process`** — main queue. Unhandled consumer nacks dead-letter to `orchestrator.process.dead`.
- **`orchestrator.process.retry`** — holds failed messages with a `messageTtl` of 10 s; expired messages dead-letter _back_ to `orchestrator.process`. Retries are explicit: on failure the worker republishes with `attempt + 1`, up to `MAX_RETRY_ATTEMPTS = 3`.
- **`orchestrator.process.dead`** — terminal. Messages land here after max retries (with `failedAt` and `error` attached) or via nack fallback.

Two error messages are treated as **non-retryable** and acked immediately (`isNonRetryableError()` in the worker): `"Conversation does not need processing"` and `"Last customer message not found"`. If you add a new benign early-exit to `runConversation()`, add its message there too — otherwise it will burn three retries per occurrence.

`RabbitMQService` reconnects with exponential backoff (max 10 attempts, capped at 30 s), re-declares queues and re-attaches consumers on reconnect, and publishes with `persistent: true`.

### Safety nets

Registered in `server/services/scheduled-jobs.registry.ts`:

- **`orchestrator-sweep`** (every 30 s) — re-enqueues conversations that have had `needs_processing = true` for more than 30 s, with trigger `"sweep"`. This is why `enqueue()` can safely no-op when RabbitMQ is down: the sweep catches up once it reconnects.
- **`orchestrator-inactivity-check`** (every 5 min) — runs `Orchestrator.checkInactivity()`.
- **`orchestrator-stale-message-check`** (every 60 s) — detects and recovers stale/lost messages via `message-recovery.service.ts`.
- **`orchestrator-worker-tick`** — the **legacy 1-second polling loop**. It still exists (`Orchestrator.loop()` / `orchestratorWorker.tick()`) but is registered with `enabled: false`; the RabbitMQ consumer replaced it. Don't build on it.

## `runConversation()` Step by Step

The pipeline in `server/orchestrator/run.ts` is numbered `00`–`04` in code comments:

1. **Skip check** — conversations with `status === "human-took-over"` or an `assigned_user_id` are skipped entirely. Human takeover works by setting those fields; `"ai_return"` re-enqueues when handed back.
2. **Lock (00)** — `conversation.lock()` returns a `lockerId` or bails (lock contention is not an error, just "someone else is on it"). A heartbeat refreshes the lock every 5 s (`LOCK_HEARTBEAT_INTERVAL_MS`) so long LLM/tool calls don't look abandoned. The `finally` block does an ownership-checked unlock.
3. **Bootstrap** — seeds initial system, agent-instruction, and bot messages if absent. Throws the two non-retryable errors if `needs_processing` is false or there is no customer message.
4. **Perception (01)** — intent/sentiment/language on the last customer message, saved via `message.savePerception()`. If the intent is `close_satisfied`/`close_unsatisfied` (or `greet` plus a gratitude regex), `validateConversationClosure()` re-checks against the full transcript; a confirmed closure resolves the conversation, purges Redis secrets (`conversationSecretService`), sends an LLM-generated closing message, and exits early.
5. **Retrieval (02)** — playbook fetch and vector search run in parallel (`Promise.all`), then playbook selection. Selected playbooks are stored with `conversation.updatePlaybook()`, which populates `enabled_tools`. Matched documents are attached via `conversation.addDocument()`.
6. **Context init (03)** — `orchestration_status` is initialized (`{ version: "v1", lastTurn: 0, toolLog: [] }`) if missing.
7. **Execution loop (04)** — `handleExecutionLoop()`, see below.
8. **On success** — error counters reset, title generated asynchronously once there are ≥ 2 customer messages, `setProcessed(true)`. **On failure** — `processing_error_count` increments; at ≥ 3 the conversation is flagged `is_stuck` with `stuck_reason: "repeated_processing_failures"`.

Throughout the run, the processing phase (`ProcessingPhase` in `types.ts`: `perceiving → retrieving → executing → idle`) is persisted in `orchestration_status.processingState` and broadcast as a `conversation_status_changed` WebSocket event — via Redis pub/sub channel `websocket:events` when Redis is up, direct `websocketService` otherwise (`publishStatusChange()` in `run.ts`). The dashboard's "typing" indicators are driven by these events.

## The Three Layers

All LLM calls go through `LLMService` (`server/services/core/llm.service.ts`). Instead of hardcoding models, callers pass a **task-complexity tier** (`"easy" | "medium" | "hard"`, default `"hard"`) which resolves to a concrete model through the organization's provider config (`server/services/llm/model-catalog.ts`, `tier-maps.ts`). Env overrides: `LLM_TIER_HARD`, `LLM_TIER_MEDIUM`, `LLM_TIER_EASY`. Never document or assume a single model name.

Prompts are file-based markdown templates resolved by `PromptService.getPrompt()` from `server/prompts/{lang}/` — to change what a layer says to the LLM, edit the prompt file, not the layer code.

### Perception (`perception.layer.ts`)

- `perceive(message, conversationId, organizationId)` — one structured-output LLM call (prompt `perception/intent-analysis`) returning `{ intent: {label, score}, sentiment: {label, score}, language }`. Intent/sentiment labels are schema-enum-constrained to `MessageIntent` / `MessageSentiment`; language is an ISO 639-1 code enforced by the JSON-schema pattern `^[a-z]{2}$`.
- `getAgentCandidate()` exists (prompt `perception/agent-selection`, score threshold > 0.7) but is **not called from `runConversation()`** — agents are assigned at conversation creation with a fallback to the organization default. Don't wire new features through it without reconsidering that decision.

### Retrieval (`retrieval.layer.ts`)

- `getPlaybookCandidate(messages, playbooks, orgId)` — an LLM scores active playbooks against the conversation (prompt `retrieval/playbook-selection`); accepted only if score > 0.7, otherwise `null`.
- `getRelevantDocuments(messages, orgId)` — builds the query from the **last 3 customer messages**, runs `vectorStoreService.search(orgId, query, 5)` (top-5 chunks, pgvector), and keeps results with similarity > 0.4. Errors return `[]` — retrieval failure never fails the run.

### Execution (`execution.layer.ts`)

`execute(conversation, customerLanguage)` assembles a single planner prompt from:

- the base planner prompt (`execution/planner`), including the combined tool name list (playbook `enabled_tools` + always-available core tools from `coreToolRegistry` in `server/services/core/core-tools`);
- a language-enforcement block (only while the conversation has ≤ 3 customer messages);
- a user-context block (`buildContextPrompt()`) — merged customer `external_metadata` + conversation `context`, plus Redis secret **key names** to be referenced as `<<secret.key>>` in tool args (values are never exposed to the model);
- a knowledge block (`buildKnowledgePrompt()`) — the last 5 attached documents, each truncated to 8,000 chars.

One structured LLM call against `buildPlannerSchema(allToolNames)` returns `{ step, userMessage, toolName, toolArgs, handoffReason, closeReason, rationale }` with `step ∈ ASK | RESPOND | CALL_TOOL | HANDOFF | CLOSE`. When tools exist, `toolName` is **enum-constrained in the schema** to the allowed set, so the model can't emit a malformed name. The layer then auto-corrects common LLM mistakes: missing step with a tool → `CALL_TOOL`; `RESPOND` with a tool but no message → `CALL_TOOL`; invalid tool name or `ASK`/`RESPOND` without a `userMessage` → return `null`, which makes the loop retry the planner.

## The Execution Loop (`run.ts::handleExecutionLoop`)

Caps: `MAX_ITERATIONS = 15`, `MAX_EMPTY_RETRIES = 3` (after which a canned fallback message is sent). Each iteration calls `executionLayer.execute()` and dispatches on `step`:

- **`CALL_TOOL`** — a `TOOL`-type message is written with `toolStatus: "RUNNING"`, then `ToolExecutionService.handleToolExecution()` runs the tool and updates the message in place; the loop continues so the planner can read the result. A guard in `run.ts` blocks non-core tools not present in `enabled_tools` — the blocked call is fed back to the model as a `TOOL` error message and does **not** count against the iteration cap. After a successful `recommend_products` call, `maybeEmitProductRecommendation()` emits a separate `PRODUCT_RECOMMENDATION` message carrying the product payload for the webchat/dashboard cards.
- **`HANDOFF`** — status becomes `pending-human` (broadcast via WebSocket). If online humans exist (`userRepository.findOnlineByOrganization`), the agent's `human_handoff_available_instructions` are injected and the loop continues, or a default/LLM-generated transfer message is sent; with nobody online, `human_handoff_unavailable_instructions` or a default apology. A `handoffProcessed` flag deduplicates repeated HANDOFF steps.
- **`CLOSE`** — optional final message, Redis secrets deleted, loop ends. (The conversation status itself is resolved by the closure path in perception or by inactivity — CLOSE only ends the turn.)
- **`RESPOND` / `ASK`** — the bot message is sent with guardrail metadata attached. If the result also carries an unexecuted tool, the message is flagged `potentialHallucination` in metadata and an error is logged.

## Guardrails (Two-Stage)

Applied by `ExecutionLayer.applyConfidenceGuardrails()` **only to `RESPOND` steps**, and skipped for `GREET` / `CLOSE_SATISFIED` / `CLOSE_UNSATISFIED` intents:

1. **Company interest** (`company-interest-guardrail.service.ts`) — an LLM assesses the drafted response and returns `{ violationType, severity, requiresFactCheck }`. Critical severity blocks the response: it becomes a `HANDOFF` with a translated fallback message, the original text preserved in metadata.
2. **Fact grounding** (`confidence-guardrail.service.ts`) — runs only when Stage 1 sets `requiresFactCheck`. Weighted score = grounding 0.6 + retrieval 0.3 + certainty 0.1; default tiers `high ≥ 0.8`, `medium ≥ 0.5`, `low < 0.5` (org-overridable via organization settings, merged by `mergeConfig`). Recent tool results (last 3 `TOOL` messages) count as authoritative synthetic documents (similarity 0.95). Medium tier triggers one `performRecheck()` with relaxed retrieval (threshold 0.3, up to 10 docs), kept only if the score improves. Low tier converts to `HANDOFF` (if `enableEscalation`) or a fallback message.

All outcomes are appended to `orchestration_status.guardrailLog` (plus a legacy `confidenceLog`) by `saveConfidenceLog()` in `run.ts`. The full guardrail design is documented in [`docs/technical/guardrails.md`](../guardrails.md).

## Inactivity Lifecycle

`Orchestrator.checkInactivity()` (`server/orchestrator/index.ts`), helpers in `conversation-utils.ts`. Thresholds derive from `config.conversation.inactivityInterval`:

- **½×** — LLM-generated warning message (marked `metadata.isInactivityWarning`), conversation re-enqueued with trigger `"inactivity"`.
- **1×** — close with a message.
- **2×** — close silently; empty conversations older than 2× are deleted outright.

Closures set `status: "resolved"`, fire the `conversation.resolved` hook, and trigger title generation (prompt `conversation/title-generation`, first 10 public messages, 255-char cap).

## Extending the Orchestrator

- **Change LLM behavior** — edit the prompt file under `server/prompts/en/…` (and translations under `pt/`, `es/`); keep the JSON schema in the layer in sync if the output shape changes.
- **New planner step type** — extend the `step` enum in `buildPlannerSchema()` and `ExecutionResult` (`execution.layer.ts`), then add a branch in `handleExecutionLoop()` (`run.ts`).
- **New processing trigger** — add it to `OrchestratorTrigger` and call `orchestratorQueueService.enqueue()` from wherever the event originates. Never call `runConversation()` directly from request handlers; go through the queue so locking, retries, and the sweep apply.
- **New always-available tool** — register it in the core tool registry (`server/services/core/core-tools`); it will be merged into the planner's tool list and exempted from the `enabled_tools` gate automatically.
- **New early-exit condition** — if it should not be retried, add its error message to `isNonRetryableError()` in `orchestrator.worker.ts`.

Follow the layering rules in [`server/orchestrator/ARCHITECTURE.md`](../../../server/orchestrator/ARCHITECTURE.md): DB access through repositories, entity state changes through entity methods, business logic in the layers.

## Testing and Debugging

- Orchestrator tests live in `server/tests/orchestrator/` (Jest): `cd server && npm test`.
- Set `LOG_LEVEL=debug` and filter with `DEBUG_MODULES="perception,retrieval,execution"` — the layers log under those module names; the pipeline logs under `orchestrator-run`, the consumer under `orchestrator-worker`, the queue under `orchestrator-queue`.
- Every log line is a child logger carrying `organizationId` and `conversationId`, so you can trace a single conversation across layers.
- Stuck conversations are visible in the DB: `is_stuck`, `stuck_reason`, `processing_error_count`, `last_processing_error`. Terminally failed queue messages sit in `orchestrator.process.dead` with the error attached.
