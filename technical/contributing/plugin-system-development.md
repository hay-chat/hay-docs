---
layout: docs.njk
title: Plugin System
description: Internals of the plugin loader, worker runtime, and database entities
section: technical
navGroup: Contributing
navOrder: 3
---

# Plugin System Internals

> **How Hay Core discovers, loads, and runs plugins**

This document is for developers working on the plugin system infrastructure itself — the loader, the worker runtime, the database entities, and the hook dispatch. If you're building a plugin, see [Getting Started with Plugins](/docs/technical/plugins/getting-started/) instead.

## Table of Contents

- [Architecture Overview](#architecture-overview)
- [Plugin Discovery and Registration](#plugin-discovery-and-registration)
- [The Worker Runtime](#the-worker-runtime)
- [Instance Lifecycle Management](#instance-lifecycle-management)
- [Database Entities](#database-entities)
- [Lifecycle Hooks](#lifecycle-hooks)
- [Cron Scheduling](#cron-scheduling)
- [Webhook Routing and Channel Delivery](#webhook-routing-and-channel-delivery)
- [The Plugin API (Worker → Core)](#the-plugin-api-worker--core)
- [Dashboard-Facing tRPC Surface](#dashboard-facing-trpc-surface)
- [Known Gaps and Legacy Paths](#known-gaps-and-legacy-paths)

---

## Architecture Overview

The core model in one paragraph: a plugin is a directory under `plugins/core/<name>/` (or `plugins/custom/{organizationId}/<name>/`) whose `package.json` contains a `hay-plugin` block. The plugin ID **is** the npm package `name` (e.g. `hay-plugin-klaviyo`, `hay-channel-instagram-meta`). Plugins do **not** run inside the core process — for each `(organizationId, pluginId)` pair, core lazily spawns a separate Node.js HTTP worker via the SDK runner, talks to it over localhost HTTP, and kills it after 5 minutes of inactivity.

> **There is no `manifest.json`.** The plugin system was migrated to a TypeScript-first model where all metadata lives in `package.json` → `hay-plugin` and in the code itself (via `defineHayPlugin` from `@hay/plugin-sdk`). Nothing in core reads a `manifest.json` file. Older docs and comments that mention one are stale.

### Key services

| Service                 | File                                                 | Responsibility                                                                   |
| ----------------------- | ---------------------------------------------------- | -------------------------------------------------------------------------------- |
| Plugin Manager          | `server/services/plugin-manager.service.ts`          | Filesystem discovery, registry (DB + in-memory), build/install orchestration     |
| Plugin Runner           | `server/services/plugin-runner.service.ts`           | Spawning/stopping workers, env injection, worker JWT, metadata readiness         |
| Plugin Instance Manager | `server/services/plugin-instance-manager.service.ts` | On-demand startup, activity tracking, idle cleanup, per-plugin instance pool     |
| Plugin Cron             | `server/services/plugin-cron.service.ts`             | Core-side scheduling of plugin-declared cron jobs                                |
| Webhook Router          | `server/services/webhook-router.service.ts`          | Shared-webhook fan-out for URLs that carry no org identifier                     |
| Channel Delivery        | `server/services/channel-delivery.service.ts`        | Outbound message delivery to channel plugins via `/deliver`                      |
| OAuth                   | `server/services/oauth.service.ts`                   | The entire OAuth dance (authorize, token exchange, refresh) on behalf of plugins |

The SDK itself lives at `packages/plugin-sdk/` — types under `packages/plugin-sdk/types/`, the worker runner under `packages/plugin-sdk/runner/`.

---

## Plugin Discovery and Registration

**File**: `server/services/plugin-manager.service.ts` (`PluginManagerService`)

### Initialization flow (`initialize()`)

1. `discoverPlugins()` — scan `plugins/core/` and every `plugins/custom/{organizationId}/` directory.
2. For each subdirectory, `registerPlugin(pluginPath, sourceType, organizationId)`:
   - Read `package.json`. **If there is no `hay-plugin` key, the directory is silently skipped** — this is the sole discovery criterion.
   - Plugin ID = `package.json.name`. Display name = `hay-plugin.displayName` or a Title-Cased version of the package name (`parseDisplayName`).
   - Build a manifest from the `hay-plugin` block and persist it as the `manifest` jsonb column. The primary `type` comes from `hay-plugin.category`, plus extra types inferred from capabilities (`inferTypeFromCapabilities`: `mcp` → `mcp-connector`, `routes`/`messages` → `channel`, `sources` → `retriever`, `products` → `products`) and `documentImporter: true` → `document_importer`.
   - Load i18n JSON files from the plugin's `i18n/` directory into `manifest.i18n`.
   - Calculate a SHA-256 checksum over the plugin's `.ts`/`.js` files (excluding `node_modules` and `dist`) for change detection.
   - Upsert into `plugin_registry` via `pluginRegistryRepository.upsertPlugin()` and the in-memory `registry` map.
3. `loadRegistryFromDatabase()` — merge DB rows into the in-memory map.
4. `validateExistingPlugins()` — plugins present in the DB but not found on disk are marked `status = "not_found"`.
5. `restorePluginsFromZip()` — custom/git plugins with a stored ZIP upload get re-extracted if their directory is missing.
6. `initializeAutoActivatedPlugins()` — legacy path for `autoActivate: true` plugins (document importers) that register a tRPC router at boot.

### The `hay-plugin` block

The fields core actually reads (interface `HayPluginBlock` in `plugin-manager.service.ts`):

```typescript
interface HayPluginBlock {
  displayName?: string;
  category?: PluginType;        // integration | channel | tool | analytics
  entry?: string;               // e.g. "./dist/index.js"
  capabilities?: string[];      // routes | mcp | auth | config | ui | messages | customers | sources
  config?: ...;                 // config schema (usually declared in code instead)
  env?: string[];               // allow-list of host env vars the worker may inherit
  auth?: ...;
  channel?: string;             // channel plugins: the channel slug, e.g. "instagram"
  autoActivate?: boolean;       // legacy (document importers)
  trpcRouter?: string;          // legacy (document importers)
  documentImporter?: boolean;   // legacy (document importers)
}
```

Capabilities are declarative: they drive marketplace classification, type inference, and the scope of the worker's JWT. They are not hard-enforced against what the plugin actually implements.

The manifest stored in the registry is `RegistryManifest` — identical to `HayPluginManifest` (`server/types/plugin.types.ts`) except `capabilities` is the flat string array from `package.json`. Consumers branch on `Array.isArray(manifest.capabilities)`.

---

## The Worker Runtime

**File**: `server/services/plugin-runner.service.ts` (`PluginRunnerService`, singleton via `getPluginRunnerService()`)

There is no in-process plugin loading and no generic "process manager" service — the runner service is the single source of truth for worker processes, keyed by `"orgId:pluginId"` in an in-memory `Map<string, WorkerInfo>`.

### `startWorker(orgId, pluginId)`

1. Load the `PluginRegistry` row and the org's `PluginInstance` row; refuse to start if the instance doesn't exist or isn't `enabled`.
2. If `authMethod === "oauth"` and the stored token expires within 5 minutes, refresh it via `oauthService` **before** spawning.
3. Allocate a port (`server/services/port-allocator.service.ts`) and resolve the org's config (`resolveConfigForWorker` in `server/lib/config-resolver.ts`), which applies `env:` fallbacks for config fields.
4. Spawn the SDK runner as a child process:

   ```
   node packages/plugin-sdk/dist/runner/index.js \
     --plugin-path=<abs path> --org-id=<orgId> --port=<port> --mode=production
   ```

   (In development, if the compiled runner doesn't exist, it falls back to `npx tsx packages/plugin-sdk/runner/index.ts`.)

5. Environment injection (`buildSDKEnv`):
   - `HAY_ORG_ID`, `HAY_PLUGIN_ID`, `HAY_WORKER_PORT`
   - `HAY_ORG_CONFIG` — JSON: `{ org: { id }, config: <resolved config> }`
   - `HAY_ORG_AUTH` — JSON-serialized `AuthState` (decrypted credentials)
   - `HAY_API_URL` + `HAY_API_TOKEN` — a JWT (`generatePluginJWT`) scoped to `{ organizationId, pluginId, scope: "plugin-api", capabilities }`, expiring in 24h, so the worker can call back into core
   - Host env vars from the plugin's `hay-plugin.env` allow-list — filtered through a deny-pattern list (`SECRET`, `PASSWORD`, `TOKEN`, `^DB_`, `^HAY_`, `^JWT`, `^OPENAI`, …) so secret-looking names are blocked by default
6. Wait for the worker's **`GET /metadata`** endpoint to respond (up to 20 attempts × 500ms) — not `/health`.
7. Update the `PluginInstance` row: `runtimeState = "ready"`, `running = true`, `processId`, health `"healthy"`. On failure: `runtimeState = "error"` + `lastError`.
8. Kick off MCP tool discovery in the background (`fetchAndStoreTools` in `server/services/plugin-tools.service.ts`, which caches the worker's `GET /mcp/list-tools` result).

### `stopWorker(orgId, pluginId)`

`POST /disable` on the worker (5s timeout, best-effort), then `SIGTERM`, then `SIGKILL` after 5 seconds if it hasn't exited. The port is released and the instance row updated on the process `exit` event.

### Worker HTTP surface

The SDK runner's HTTP server (`packages/plugin-sdk/runner/http-server.ts`) exposes exactly:

| Route                                        | Purpose                                                                                                                                                              |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `GET /health`                                | Liveness                                                                                                                                                             |
| `GET /metadata`                              | Descriptors registered in `onInitialize` (config schema, auth methods, routes, crons, UI pages, webhook routing) — core's readiness signal and metadata cache source |
| `POST /validate-auth`                        | Runs the plugin's `onValidateAuth` hook                                                                                                                              |
| `POST /on-connected`                         | Runs `onConnected` after OAuth tokens are stored                                                                                                                     |
| `POST /config-update`                        | Runs `onConfigUpdate`                                                                                                                                                |
| `POST /disable`                              | Runs `onDisable`                                                                                                                                                     |
| `POST /cron/:name`                           | Runs a registered cron handler                                                                                                                                       |
| `POST /mcp/call-tool`, `GET /mcp/list-tools` | MCP tool invocation / discovery                                                                                                                                      |
| _(dynamic)_                                  | Any route the plugin declared via `register.route(method, path, handler)` — mounted verbatim on the Express app                                                      |

All dashboard/webhook traffic to a worker goes through the core proxy: `ALL /v1/plugins/:pluginId/*` (`server/routes/v1/plugins/proxy.ts`). The proxy resolves the organization from auth, subdomain, or query param, starts the worker on demand, bumps its activity timestamp, strips credential-bearing headers (`authorization`, `cookie`, etc.), and forwards the request.

---

## Instance Lifecycle Management

**File**: `server/services/plugin-instance-manager.service.ts` (`PluginInstanceManagerService`)

- `ensureInstanceRunning(organizationId, pluginId)` — the on-demand entry point. Deduplicates concurrent startups via a `startupQueue` map, checks pool limits, and delegates the actual spawn to the runner service.
- **Idle cleanup**: `INACTIVITY_TIMEOUT_MS = 5 minutes`. `cleanupInactiveInstances()` is **not** run by an internal timer — it's invoked by the platform scheduler job `plugin-instance-cleanup` (see `server/services/scheduled-jobs.registry.ts`), which runs every minute. Activity is tracked both in memory (`instanceActivity` map) and in the `last_activity_at` column.
- **Pool limits**: each plugin's `plugin_registry.max_concurrent_instances` (default 10) caps concurrent workers across orgs. `ensureInstanceRunning` waits up to 30 seconds for a slot before throwing.
- `updateActivityTimestamp()` is called from the proxy on every forwarded request so active workers aren't reaped mid-conversation.
- `stopAllForOrganization(organizationId)` tears down every worker for an org.

Implication for plugin behavior: **workers are ephemeral**. Anything a plugin needs long-lived (schedules, webhook subscriptions) must be delegated to core (`register.cron`, `register.webhookRouting`) rather than held in worker memory.

---

## Database Entities

All three entities live in `server/entities/` and follow the project's snake_case-in-DB / camelCase-in-TS convention (see `server/database/DATABASE_CONVENTIONS.md`). The field lists below reflect the entity classes — do not trust older docs showing raw SQL with `SERIAL` primary keys; IDs are UUIDs.

### `plugin_registry` (`plugin-registry.entity.ts`)

One row per discovered plugin (global, not per-org). Unique index on `pluginId`.

| Field                                                                                                  | Type                          | Notes                                                          |
| ------------------------------------------------------------------------------------------------------ | ----------------------------- | -------------------------------------------------------------- |
| `id`                                                                                                   | uuid PK                       |                                                                |
| `pluginId`                                                                                             | varchar(255)                  | npm package name; unique                                       |
| `name`, `version`                                                                                      | varchar                       | display name / package version                                 |
| `pluginPath`                                                                                           | varchar(255)                  | relative to the `plugins/` root, e.g. `core/stripe`            |
| `manifest`                                                                                             | jsonb                         | the `RegistryManifest` built from `package.json`               |
| `installed`, `built`                                                                                   | boolean                       | build/install state                                            |
| `lastInstallError`, `lastBuildError`                                                                   | text                          |                                                                |
| `installedAt`, `builtAt`                                                                               | timestamptz                   |                                                                |
| `checksum`                                                                                             | varchar(64)                   | SHA-256 for change detection                                   |
| `maxConcurrentInstances`                                                                               | integer, default 10           | worker pool cap                                                |
| `metadata`, `metadataFetchedAt`, `metadataState`                                                       | jsonb / timestamptz / varchar | plugin-global cache of the worker's `/metadata` response       |
| `status`                                                                                               | varchar                       | `available` \| `not_found` \| `disabled` (`PluginStatus` enum) |
| `sourceType`                                                                                           | varchar                       | `core` \| `custom` \| `git`                                    |
| `organizationId`                                                                                       | uuid, nullable                | set for custom plugins                                         |
| `zipFilePath`, `zipUploadId`, `uploadedById`, `uploadedAt`                                             |                               | custom-plugin upload provenance                                |
| `gitConnectionId`, `gitRepoFullName`, `gitBranch`, `gitLastCommitSha`, `gitLastSyncAt`, `gitSyncError` |                               | git-sourced plugin fields                                      |

### `plugin_instances` (`plugin-instance.entity.ts`)

One row per `(organizationId, pluginId)` — the org's enablement, config, auth, and runtime state. Unique index on `(organizationId, pluginId)`. Note `pluginId` here is a **uuid FK to `plugin_registry.id`**, not the string plugin ID.

| Field                                                                                 | Type                        | Notes                                                                                                                                 |
| ------------------------------------------------------------------------------------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `pluginId`                                                                            | uuid FK → `plugin_registry` |                                                                                                                                       |
| `enabled`                                                                             | boolean                     |                                                                                                                                       |
| `config`                                                                              | jsonb                       | org config values                                                                                                                     |
| `authState`                                                                           | jsonb                       | `AuthState` — **all credential fields encrypted at rest** via `AuthStateEncryptedTransformer` (`server/lib/auth/utils/encryption.ts`) |
| `authValidatedAt`                                                                     | timestamptz                 |                                                                                                                                       |
| `authMethod`                                                                          | varchar                     | `api_key` \| `oauth`                                                                                                                  |
| `running`, `processId`, `lastStartedAt`, `lastStoppedAt`, `lastError`, `restartCount` |                             | worker process bookkeeping                                                                                                            |
| `lastHealthCheck`, `healthStatus`                                                     |                             | `healthy` \| `unhealthy` \| `unknown`                                                                                                 |
| `status`                                                                              | varchar                     | legacy field (`stopped`/`starting`/`running`/`stopping`/`error`), kept for backwards compatibility                                    |
| `runtimeState`                                                                        | varchar                     | the current state machine field (`PluginInstanceRuntimeState`)                                                                        |
| `lastActivityAt`                                                                      | timestamptz                 | idle-kill input                                                                                                                       |
| `priority`                                                                            | integer                     |                                                                                                                                       |

### `plugin_webhook_routes` (`plugin-webhook-route.entity.ts`)

Plugin-agnostic index mapping `(pluginId, routingKey)` → `(organizationId, pluginInstanceId)`, with a unique index on the pair. Populated from the routing keys a plugin returns from its `onConnected` hook; consumed by the webhook router to resolve which org a shared-webhook payload belongs to. Cascade-deleted with the plugin instance.

---

## Lifecycle Hooks

A plugin's entry module default-exports `defineHayPlugin(factory)`; the factory returns a `HayPluginDefinition` (`packages/plugin-sdk/types/plugin.ts` — the only required field is `name`). Core drives the hooks over the worker HTTP surface:

| Hook             | Triggered by                                                                       | Contract                                                                                                                                                                                          |
| ---------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `onInitialize`   | Runner boot, before the HTTP server starts                                         | Descriptor-only: `register.*` calls. No network, no org data.                                                                                                                                     |
| `onStart`        | Every worker start/restart for an org                                              | Read `ctx.config` / `ctx.auth`, gate on credentials, start MCP servers. Must not crash the worker.                                                                                                |
| `onConnected`    | Core `POST /on-connected` right after OAuth tokens are stored (`oauth.service.ts`) | May return `{ routingKeys }`, which core persists in `plugin_webhook_routes`. Throwing never fails the OAuth flow.                                                                                |
| `onValidateAuth` | Core `POST /validate-auth` when credentials change                                 | Return `true`/`false`; a thrown error is caught by the runner and treated as invalid. If absent, auth is assumed valid.                                                                           |
| `onConfigUpdate` | **Never (currently).**                                                             | The runner exposes `POST /config-update`, but core never calls it — `plugins.configure` saves the config and restarts the running worker instead, so config changes arrive via a fresh `onStart`. |
| `onDisable`      | Core `POST /disable` on disable/uninstall and before SIGTERM                       | Tear down resources.                                                                                                                                                                              |
| `onEnable`       | **Never.**                                                                         | Typed in the SDK (`types/hooks.ts`) but the runner does not call it — reserved for a future core-side path. Do not build on it.                                                                   |

The `register.*` API available in `onInitialize` (`packages/plugin-sdk/types/register.ts`): `register.config(schema)`, `register.auth.apiKey(...)` / `register.auth.oauth2(...)`, `register.route(method, path, handler)`, `register.ui.page(...)`, `register.cron(options)`, `register.webhookRouting(descriptor)`. Everything registered surfaces as data in `GET /metadata`, which core caches on `plugin_registry.metadata`.

**OAuth is entirely core-side.** `server/services/oauth.service.ts` runs authorization, token exchange, and refresh — including non-standard flows a plugin declares via `tokenExchange`/`tokenRefresh` descriptors (`packages/plugin-sdk/types/auth.ts`), executed declaratively by core (used by the Instagram plugin's `ig_exchange_token`/`ig_refresh_token`). Plugins never call OAuth endpoints themselves; the runner also refreshes expiring tokens before worker start (see [The Worker Runtime](#the-worker-runtime)).

---

## Cron Scheduling

**File**: `server/services/plugin-cron.service.ts`

Because workers are idle-killed, plugins must not self-schedule with `setInterval`/`node-cron`. Instead, `register.cron` declares the schedule as data, and core:

1. Reads cron descriptors from the plugin's cached `/metadata`.
2. Registers one platform-scheduler job per enabled org per declared cron, named `plugin-cron:<pluginId>:<orgId>:<cronName>` (via `schedulerService.registerJob`).
3. When a job fires: ensure the worker is running (waking it if needed), then `POST /cron/:name` so the handler executes in the worker.
4. If the handler called `ctx.auth.update(...)`, the updated credentials come back in the response and core persists them (encrypted) and restarts the worker.

Jobs are unregistered when the plugin is disabled for the org.

---

## Webhook Routing and Channel Delivery

### Inbound: shared-webhook fan-out

Some providers (e.g. Meta) deliver all events for all customers to **one** webhook URL that carries no org identifier. A plugin can declare a `WebhookRoutingDescriptor` via `register.webhookRouting` — pure data, no code: an HMAC-SHA256 signature spec (`header`, `secretEnv`), an optional GET verification-challenge spec, and dot-path extraction rules (`routeKeyPath: { itemsPath, keyPath }`).

The dispatch point is in the proxy (`server/routes/v1/plugins/proxy.ts`): a `POST .../webhook` request with **no** org identifier, for a plugin that declared a routing descriptor, is diverted to `webhookRouterService.handle()` (`server/services/webhook-router.service.ts`), which:

1. Verifies the HMAC over the raw body bytes (`verifyHmacSha256` from `server/services/plugin-route.service.ts`).
2. Answers the provider's GET verification challenge if declared.
3. Extracts a routing key per payload entry, resolves key → org via `plugin_webhook_routes`, and fans each entry out to the right org's worker `POST /webhook` (with credential-bearing headers stripped).

Core never learns which provider it's routing for — all provider knowledge is in the plugin's declaration.

### Inbound: normal per-org webhooks and message ingestion

Webhooks that do identify the org go straight through the proxy to the worker's registered `/webhook` route. The plugin verifies/filters the event and then calls **back into core** with `messages.receive` (see next section). Inbound dedupe is core-side: if the plugin passes `metadata.mid`, core claims it atomically via Redis SETNX and drops duplicates (`server/routes/v1/plugin-api/trpc.ts`); channels that don't pass a `mid` get no dedupe.

### Outbound: channel delivery

**File**: `server/services/channel-delivery.service.ts`

Subscribes to the Redis `websocket:events` channel:

- `message_received` events for bot/human-agent messages (web channel skipped) → resolve the plugin by channel slug via `pluginManagerService.findPluginIdByChannel(channel)` (matching `hay-plugin.channel`), ensure the org's worker is up, and `POST /deliver` with `{ to, content, messageId, conversationId, ... }`. The plugin returns `{ success, providerMessageId?, error? }`; on success core stores the provider message ID. Convention: non-retryable provider errors return **HTTP 200 with `success: false`** to avoid retry storms.
- `conversation_status_changed` → `pending-human` → `POST /escalate` on the worker. This is best-effort: a 404 from plugins that don't implement `/escalate` is silently tolerated.

See [Channel Architecture](/docs/technical/plugins/channel-architecture/) for the plugin-author view.

---

## The Plugin API (Worker → Core)

**File**: `server/routes/v1/plugin-api/trpc.ts`

Workers call back into core over tRPC-HTTP using the `HAY_API_URL`/`HAY_API_TOKEN` env vars. Every procedure is a `pluginProcedure` gated by the worker's JWT (org + plugin + capabilities). Current surface:

- `messages.receive` — input `{ from, content, channel, metadata?, senderType?, externalConversationId? }`; finds/creates the Customer (keyed on `external_id` per channel) and conversation, adds the message
- `messages.send`, `messages.getByConversation`
- `conversations.updateStatusByExternalId`
- `customers.get`, `customers.findByExternalId`, `customers.upsert`
- `sources.register`
- `mcp.registerLocal`, `mcp.registerRemote`
- `products.upsertMany`, `products.delete`

The router self-describes as a "simplified initial implementation" — expect this surface to grow.

---

## Dashboard-Facing tRPC Surface

**Files**: `server/routes/v1/plugins/index.ts` (router), `server/routes/v1/plugins/plugins.handler.ts` (implementations). All procedures are `authenticatedProcedure`.

- **Queries**: `plugins.getAll`, `plugins.get`, `plugins.getInstances`, `plugins.getUITemplate`, `plugins.getMCPTools`, `plugins.getMenuItems`, `plugins.testConnection`, `plugins.getPluginTranslations`, `plugins.oauth.isAvailable`, `plugins.oauth.status`
- **Mutations**: `plugins.enable`, `plugins.disable`, `plugins.restart`, `plugins.configure`, `plugins.refreshMCPTools`, `plugins.validateAuth`, `plugins.oauth.initiate`, `plugins.oauth.revoke`

There is **no** generic `plugins.invokeTool` mutation — MCP tool invocation happens inside the orchestrator via the worker's `POST /mcp/call-tool`.

Non-tRPC HTTP: the catch-all worker proxy `ALL /v1/plugins/:pluginId/*` (`proxy.ts`), and plugin UI assets served at `/plugins/ui/:pluginName/:assetPath`.

---

## Known Gaps and Legacy Paths

Things a contributor should know exist (or don't) before building on them:

- **`onEnable` is dead code** in the runner — typed but never invoked.
- **Document importers bypass the worker model.** Plugins with `autoActivate: true` + `trpcRouter` (e.g. `plugins/core/atlassian`) load a tRPC router in-process at boot via `server/services/plugin-router-registry.service.ts`, coupled to `@server/*` internals. This is the legacy special case, not the pattern to extend.
- **`onConfigUpdate` is also uncalled** — the runner exposes `POST /config-update`, but core applies config changes by restarting the worker (see the hooks table).
- **`PluginApiClient` is duplicated per channel plugin** (e.g. `plugins/core/instagram/src/plugin-api.ts`) instead of being an SDK export.
- **Webhook signature format** is limited to `sha256-hmac` in `WebhookRoutingDescriptor`.
- **The instance pool's `queuedRequests` stat is tracked but never incremented** — pool limiting works via polling (`waitForAvailableSlot`), not a real queue.
- The `plugin_instances.status` column is legacy; `runtimeState` is the field the runner actually maintains.

When contributing to any of the above: follow existing patterns in the neighboring services, keep the core plugin-agnostic (never hardcode plugin IDs — resolve behavior from the manifest/metadata), and remove legacy paths outright rather than layering compatibility shims (this codebase is in alpha; breaking changes are preferred over dead code).
