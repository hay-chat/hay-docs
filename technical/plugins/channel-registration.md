---
layout: docs.njk
title: Channel Registration
description: How a plugin registers a communication channel with Hay
section: technical
navGroup: Plugin Development
navOrder: 4
---

# Channel Registration

> **How a plugin declares a communication channel and how Hay routes messages to and from it. For the end-to-end message flow design, see the [Channel Architecture](/docs/technical/plugins/channel-architecture/) guide.**

## Overview

Channel registration in Hay is **declarative**. A plugin does not call a registration API at runtime — it declares its channel in `package.json`, and core discovers it when the plugin is loaded. There are three pieces:

1. **`hay-plugin.channel`** in `package.json` — binds the plugin to a channel slug (e.g. `"instagram"`).
2. **HTTP routes on the plugin worker** — `POST /webhook` (inbound) and `POST /deliver` (outbound), registered with `register.route()` from the SDK.
3. **The plugin API callback `messages.receive`** — how the worker hands an inbound message to core, which creates the customer, conversation, and message.

Once the plugin is enabled for an organization, the channel automatically appears in the agent settings page (`dashboard/pages/agents/[id].vue` lists every enabled plugin whose type includes `channel` and that declares a `channel` slug, alongside the built-in `web` channel). No separate registration step exists or is needed.

> **Note:** an older version of this guide documented a `sources.register` API as the registration mechanism. That API is a **stub** — see [The `sources` API (not the registration mechanism)](#the-sources-api-not-the-registration-mechanism) below.

---

## Declaring the channel

The `hay-plugin` block in the plugin's `package.json` is the single source of truth. Real example from `plugins/core/instagram/package.json`:

```json
{
  "name": "hay-channel-instagram-meta",
  "type": "module",
  "main": "dist/index.js",
  "hay-plugin": {
    "entry": "./dist/index.js",
    "displayName": "Instagram",
    "category": "channel",
    "channel": "instagram",
    "capabilities": ["messages", "customers"],
    "env": ["META_APP_ID", "META_APP_SECRET", "META_VERIFY_TOKEN"]
  }
}
```

Field-by-field (interface `HayPluginBlock` in `server/services/plugin-manager.service.ts`):

| Field          | Value for a channel plugin                                                                                                                                                                        |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `category`     | `"channel"`                                                                                                                                                                                       |
| `channel`      | The channel slug (e.g. `"instagram"`, `"chatwoot"`). This is the value stored on `conversation.channel` and on `agent.channels`, and the key core uses to find your plugin for outbound delivery. |
| `capabilities` | `["messages", "customers"]` — grants the worker's JWT access to the `messages.*` and `customers.*` plugin API procedures.                                                                         |
| `env`          | Allow-list of host env var names that config fields may fall back to (optional).                                                                                                                  |

**Channel slug rules:**

- One plugin per slug. Core resolves the channel with `pluginManagerService.findPluginIdByChannel(channel)`, which returns the first loaded plugin whose `hay-plugin.channel` matches — there is no conflict handling, so pick a slug no other plugin uses.
- `"web"` is reserved for the built-in webchat/dashboard channel; the delivery service explicitly skips it.
- The plugin API validates `channel` as a 1–64 character string. Use lowercase.

---

## The plugin entry

A channel plugin registers its routes in `onInitialize` using the standard SDK entry (`defineHayPlugin` from `@hay/plugin-sdk`). Skeleton, condensed from `plugins/core/instagram/src/index.ts`:

```typescript
import { defineHayPlugin } from "@hay/plugin-sdk";

export default defineHayPlugin((globalCtx) => {
  const { logger } = globalCtx;
  let accessToken: string | null = null;

  return {
    name: "My Channel",

    onInitialize(ctx) {
      const { register } = ctx;

      // Config / auth registration as usual (register.config, register.auth.*)

      // Inbound: core proxies provider webhooks here
      register.route("POST", "/webhook", async (req, res) => {
        // verify, parse, then call messages.receive (see below)
      });

      // Outbound: core's ChannelDeliveryService posts bot/agent replies here
      register.route("POST", "/deliver", async (req, res) => {
        // send to the provider, respond { success, providerMessageId }
      });
    },

    async onStart(ctx) {
      // Read ctx.auth / ctx.config, build the provider client.
      // Gate gracefully: if not connected yet, log and return — never throw.
      accessToken = ctx.auth.get()?.credentials.accessToken ?? null;
    },
  };
});
```

See [Getting Started](/docs/technical/plugins/getting-started/) for the general plugin scaffold and the [API Reference](/docs/technical/plugins/api-reference/) for all `register.*` and lifecycle hook signatures.

---

## Inbound: webhook → `messages.receive`

### Webhook URL

Point the provider's webhook at the core proxy:

```
POST https://<hay-host>/v1/plugins/<pluginId>/webhook
```

The catch-all proxy (`server/routes/v1/plugins/proxy.ts`) resolves the organization from the request — a `?organizationId=<uuid>` query param or an `x-organization-id` header, both validated (UUID format plus a DB existence check) since the route has no auth middleware — then starts (or reuses) that org's plugin worker and forwards the request to the worker's registered `POST /webhook` route. Credential-bearing headers (`authorization`, `cookie`, etc.) are stripped before forwarding.

For providers that deliver **all tenants' events to one shared URL with no org identifier** (e.g. Meta), declare a `register.webhookRouting(...)` descriptor instead — core verifies the shared HMAC, answers the GET verification challenge, extracts a routing key per entry, resolves key → org (using the routing keys your `onConnected` hook returned), and fans out per-org payloads to each worker's `/webhook`. See the [API Reference](/docs/technical/plugins/api-reference/) and `plugins/core/instagram/src/index.ts` for the full pattern.

### Handing the message to core

Inside your `/webhook` handler, after verifying and parsing the provider payload, call back into core with the **`messages.receive`** plugin API procedure. The worker authenticates with the scoped JWT core injected as `HAY_API_TOKEN` (endpoint base in `HAY_API_URL`):

```
POST {HAY_API_URL}/v1/pluginApi.messages.receive
Authorization: Bearer {HAY_API_TOKEN}
Content-Type: application/json
```

Input schema (`server/routes/v1/plugin-api/trpc.ts`, requires the `messages` capability):

```typescript
{
  from: string;                     // channel-scoped customer identity, e.g. "instagram:<psid>"
  content: string;
  channel: string;                  // your channel slug
  metadata?: Record<string, any>;   // see below
  senderType?: "customer" | "human_agent";  // default "customer"
  externalConversationId?: string;  // provider-side conversation id, if any
}
```

What core does with it:

1. **Customer** — finds or creates a `Customer` keyed on `external_id` = `from` for the org. `metadata.profileName` backfills the name, `metadata.phone` the phone; all metadata is stored under `customer.external_metadata[channel]`.
2. **Agent** — resolves the responding agent for the channel: agents explicitly assigned to the channel first (preferring the org default agent, then the earliest-created), then the org default agent, then the first available agent. If the org has no agents at all, the call fails with `NOT_FOUND`.
3. **Conversation** — reuses the customer's active conversation on that channel or creates one. `externalConversationId` is stored in `conversation.metadata[channel].conversationId` so outbound delivery can reuse it.
4. **Dedupe** — if `metadata.mid` is a non-empty string, core claims it atomically in Redis (24h TTL). Redelivered webhooks with the same `mid` return `{ processed: false, deduped: true }` and create no message. **Channels that don't pass `mid` get no dedupe** — always set it if the provider gives you a message id.
5. **Human-agent forwarding** — `senderType: "human_agent"` (a human replied on the external system, e.g. a Chatwoot agent) records the message as a human-agent message, tags it `externalOrigin: true` so it is never echoed back to the channel, and flips the conversation to `human-took-over` so the AI orchestrator stops responding.

Return value: `{ messageId, conversationId, processed, deduped }`.

There is currently no SDK helper for these calls — each channel plugin ships a small `PluginApiClient` (raw tRPC-over-HTTP `fetch`; copy `plugins/core/instagram/src/plugin-api.ts`). This duplication is a known gap.

Other plugin API procedures useful to channel plugins (same auth, capability-gated): `messages.send`, `messages.getByConversation`, `conversations.updateStatusByExternalId` (reflect provider-side resolve/reopen into Hay), `customers.get`, `customers.findByExternalId`, `customers.upsert`.

---

## Outbound: `POST /deliver`

When the orchestrator (or a human agent in the dashboard) produces a reply on a non-web channel, `server/services/channel-delivery.service.ts` picks it up from the Redis `websocket:events` stream and delivers it via your plugin:

1. Only `BotAgent` / `HumanAgent` messages with `deliveryState === "sent"` are delivered. Messages queued for approval by test mode (`deliveryState === "queued"`) are skipped until approved, and messages tagged `externalOrigin: true` are never echoed back.
2. Core looks up your plugin with `findPluginIdByChannel(conversation.channel)`, starts the org's worker if needed, and POSTs to the worker's `/deliver` route:

```typescript
// Request body
{
  to: string;                                        // customer.external_id
  content: string;
  messageId: string;                                 // Hay message id
  conversationId: string;
  conversationMetadata: Record<string, unknown> | null;  // includes metadata[channel].conversationId if set
  messageMetadata: Record<string, unknown> | null;
}

// Expected response body
{
  success: boolean;
  providerMessageId?: string;  // stored on the Hay message when success is true
  error?: string;
}
```

**Error convention** (see `plugins/core/instagram/src/deliver.ts`): for **permanent, non-retryable** provider errors, respond `200` with `{ success: false, error: "<code>" }` — e.g. Instagram's `"24h_window_expired"` — so core logs it without retrying. Reserve `5xx` responses for unknown/transient failures.

### Escalation: `POST /escalate` (optional)

When a conversation flips to `pending-human` on a non-web channel, core POSTs `{ conversationId, conversationMetadata, reason: "orchestrator_handoff" }` to the worker's `/escalate` route so the plugin can perform a provider-side handoff (e.g. Chatwoot reopens the ticket and assigns a team). This is **best-effort**: if your plugin doesn't register `/escalate`, the resulting 404 is silently tolerated.

---

## Test mode and delivery states

Channel plugins do not implement test mode — core enforces it before delivery ever reaches the plugin:

- `deliveryState === "sent"` → delivered to your `/deliver` route.
- `deliveryState === "queued"` → held for approval (test mode); delivered only after approval flips it to `sent`.
- Other states (e.g. blocked) → never delivered.

---

## The `sources` API (not the registration mechanism)

Earlier docs described channel registration via a `sources.register` call. Current state, for the avoidance of confusion:

- **Plugin-facing `sources.register` is a stub.** The procedure exists in the plugin API router (`server/routes/v1/plugin-api/trpc.ts`, gated on the `sources` capability) but its body is a `TODO` — it logs the input and returns `{ success: true }` without persisting anything. Calling it from a plugin has no effect. **Do not use it to register a channel.**
- **A dashboard-facing `sources` tRPC router exists** (`server/routes/v1/sources/`, backed by a real `sources` table via `SourceService`): `sources.list`, `sources.get`, `sources.getByCategory` (all filter to active sources), plus `sources.register`, `sources.activate`, `sources.deactivate` (core sources `playground` and `webchat` are protected). It works, but it plays **no role in channel routing or message delivery** — nothing in the inbound/outbound paths reads it. Its only current consumer is display metadata in the insights feedback page (`dashboard/composables/useSources.ts`).

In short: the `sources` table is a vestigial catalog, and the only registration that matters for a channel plugin is the declarative `hay-plugin.channel` field described above.

---

## Reference implementations

- **`plugins/core/instagram`** — the shared-app model: empty per-org config with env-fallback OAuth client credentials, `register.webhookRouting` for shared-webhook fan-out, `onConnected` returning the IG account id as a routing key, declarative token exchange/refresh, and the `200 + success:false` non-retryable error convention in `/deliver`.
- **`plugins/core/chatwoot`** — the per-org webhook model, with the strongest signature verification (HMAC-SHA256 over raw bytes, replay window, `timingSafeEqual`), `senderType: "human_agent"` forwarding, and `/escalate` handoff.

## Related Documentation

- **[Channel Architecture](/docs/technical/plugins/channel-architecture/)** — end-to-end design of the channel system
- **[Getting Started](/docs/technical/plugins/getting-started/)** — plugin scaffold, build, and enablement
- **[Plugin API Reference](/docs/technical/plugins/api-reference/)** — full `register.*`, lifecycle hook, and plugin API documentation
- **[Quick Reference](/docs/technical/plugins/quick-reference/)** — copy-paste cheat sheet
