---
layout: docs.njk
title: Quick Reference
description: Plugin development quick reference card
section: technical
navGroup: Plugin Development
navOrder: 5
---

# Plugin Development Quick Reference

> **Copy-paste cheat sheet for common plugin tasks. Full details in the [API Reference](/docs/technical/plugins/api-reference/).**

## Quick Start Checklist

- [ ] Create plugin directory in `plugins/core/<name>/`
- [ ] Create `package.json` with a `hay-plugin` block (the package `name` IS the plugin ID — there is **no** `manifest.json`)
- [ ] Create `tsconfig.json` (ESM, `strict: true`, exclude `mcp/` and `dist/`)
- [ ] Implement `src/index.ts` — default export `defineHayPlugin(...)`
- [ ] Add a `thumbnail.svg`, `thumbnail.png`, or `thumbnail.jpg` in the plugin root (core serves the first match, priority svg > png > jpg)
- [ ] Add `i18n/en.json` (additional locales auto-discovered)
- [ ] Build: `npm --workspace=plugins/core/<name> run build` from the repo root
- [ ] Enable in dashboard and test

---

## Minimal package.json

```json
{
  "name": "hay-plugin-myservice",
  "version": "1.0.0",
  "type": "module",
  "main": "dist/index.js",
  "hay-plugin": {
    "entry": "./dist/index.js",
    "displayName": "My Service",
    "category": "integration",
    "capabilities": ["mcp", "auth"]
  },
  "scripts": {
    "build": "tsc"
  },
  "dependencies": {
    "@hay/plugin-sdk": "file:../../../packages/plugin-sdk"
  },
  "devDependencies": {
    "typescript": "^5.3.3",
    "@types/node": "^20.10.0"
  }
}
```

- **Categories**: `integration | channel | tool | analytics | products` (document importers are marked separately via `"documentImporter": true` in the `hay-plugin` block, not a category)
- **Capabilities**: `mcp | auth | config | routes | ui | cron | products` (+ `messages | customers | sources` for channels)
- **SDK path** is `file:../../../packages/plugin-sdk` — note `packages/`
- Channel plugins add `"channel": "<slug>"` and (if needed) `"env": ["VAR_A", "VAR_B"]` — the allow-list for host env fallbacks

---

## Minimal Entry Point (`src/index.ts`)

```typescript
import { defineHayPlugin } from "@hay/plugin-sdk";

export default defineHayPlugin((globalCtx) => ({
  name: "My Service",

  onInitialize(ctx) {
    // Descriptor-only: register.* calls, no network, no org data
    ctx.register.config({
      apiKey: {
        type: "string",
        label: "API Key",
        required: true,
        encrypted: true,
      },
    });

    ctx.register.auth.apiKey({
      id: "myservice-apikey",
      label: "My Service API Key",
      configField: "apiKey",
    });
  },

  async onStart(ctx) {
    // Per-org runtime start — gate on credentials, never crash the worker
    const authState = ctx.auth.get();
    if (!authState) {
      ctx.logger.info("Not configured yet — enabled without tools");
      return;
    }
    // ... start MCP servers etc.
  },
}));
```

`name` is the only required field. Real example: `plugins/core/klaviyo/src/index.ts`.

---

## Lifecycle Hooks

| Hook                  | When                            | Notes                                                        |
| --------------------- | ------------------------------- | ------------------------------------------------------------ |
| `onInitialize(ctx)`   | Once per worker boot            | `register.*` only — no I/O                                   |
| `onStart(ctx)`        | Per-org start/restart           | Read `ctx.config`/`ctx.auth`, start MCP                      |
| `onValidateAuth(ctx)` | Credentials change              | Return `true`/`false`; a throw = invalid (logged, not shown) |
| `onConnected(ctx)`    | Right after OAuth tokens stored | Return `{ routingKeys }` for shared webhooks                 |
| `onConfigUpdate(ctx)` | Settings saved                  | Platform restarts registered MCP servers itself              |
| `onDisable(ctx)`      | Disable/uninstall               | Tear down pollers/clients                                    |

`onEnable` exists in the types but **the runner never calls it** — don't rely on it.

---

## Common Patterns

### Local Bundled MCP Server (Archetype A)

```typescript
async onStart(ctx) {
  const apiKey = ctx.config.get<string>("apiKey");
  if (!apiKey) return;

  await ctx.mcp.startLocalStdio({
    id: "myservice-mcp",
    command: "node",
    args: ["index.js"],
    cwd: "./mcp", // relative to plugin dir
    env: { PRIVATE_API_KEY: apiKey },
  });
}
```

`mcp/index.js` is a stdio server using `@modelcontextprotocol/sdk` that reads credentials from `process.env`. Gold-standard reference: `plugins/core/klaviyo/mcp/index.js`.

> **`mcp/` deps:** `mcp/` is not part of the npm workspace, but its dependencies are installed for you — enabling the plugin (`installPlugin`) and `scripts/build-plugins.sh` both run `npm install` inside `mcp/` when `mcp/node_modules` is missing. Commit `mcp/package-lock.json` (not the modules) and keep the dependency list minimal.

### Remote Hosted MCP Server (Archetype B)

```typescript
async onStart(ctx) {
  const authState = ctx.auth.get();
  if (!authState) return;

  await ctx.mcp.startExternal({
    id: "stripe-mcp",
    url: "https://mcp.stripe.com",
    authHeaders: { Authorization: `Bearer ${authState.credentials.apiKey}` },
  });
}
```

References: `plugins/core/stripe` (API key), `plugins/core/hubspot` (OAuth access token).

### OAuth2 Authentication

```typescript
ctx.register.auth.oauth2({
  id: "myservice-oauth",
  label: "Connect My Service",
  authorizationUrl: "https://service.com/oauth/authorize",
  tokenUrl: "https://service.com/oauth/token",
  scopes: ["read", "write"],
  clientId: ctx.config.field("clientId"), // ConfigFieldReference
  clientSecret: ctx.config.field("clientSecret"),
});
```

The platform runs the entire OAuth dance and auto-refreshes tokens — plugins never touch OAuth endpoints. For non-standard flows, add declarative `tokenExchange`/`tokenRefresh` ops and `scopeSeparator` (see `plugins/core/instagram/src/index.ts` for Instagram's `ig_exchange_token`/`ig_refresh_token`).

### Reading Config & Auth at Runtime

```typescript
const apiKey = ctx.config.get<string>("apiKey"); // throws if missing
const region = ctx.config.getOptional<string>("region");

const authState = ctx.auth.get(); // { methodId, credentials } | null
// apiKey method → credentials.apiKey
// oauth2 method → credentials.accessToken / refreshToken? / expiresAt?
```

Config/auth arrive via `HAY_ORG_CONFIG`/`HAY_ORG_AUTH` env injected by core — never read secrets from raw `process.env` in the entry. A config field's `env:` property is only a host-env _fallback_, gated by the `hay-plugin.env` allow-list.

### HTTP Routes (Webhooks, Delivery)

```typescript
ctx.register.route("POST", "/webhook", async (req, res) => {
  // verify, then forward to core via plugin-api
  res.status(200).json({ received: true });
});
```

### Cron Jobs

```typescript
ctx.register.cron({
  name: "refresh_token", // [a-z0-9_-]+
  schedule: "0 */20 * * *", // 5-field cron
  handler: async (ctx) => {
    const token = await refresh(/* ... */);
    ctx.auth.update({ accessToken: token.accessToken, expiresAt: token.expiresAt });
  },
  retryPolicy: { maxRetries: 3, backoff: "exponential" },
});
```

Crons are scheduled by **core**, not the worker (workers are idle-killed after ~5 min). Never use `setInterval`/`node-cron` inside a plugin. `ctx.auth.update()` persists refreshed credentials encrypted and restarts the worker. Reference: `plugins/core/shopify`.

### Shared Webhook Routing (No Org ID in the URL)

```typescript
ctx.register.webhookRouting({
  signature: {
    header: "x-hub-signature-256",
    format: "sha256-hmac", // only supported format
    secretEnv: "META_APP_SECRET",
  },
  verificationChallenge: {
    modeParam: "hub.mode",
    verifyTokenParam: "hub.verify_token",
    challengeParam: "hub.challenge",
    verifyTokenEnv: "META_VERIFY_TOKEN",
  },
  routeKeyPath: { itemsPath: "entry", keyPath: "id" },
});
```

Core verifies the HMAC, answers the GET challenge, extracts routing keys, resolves key → org using the keys returned by `onConnected`, and fans out to each org's worker `POST /webhook`. Reference: `plugins/core/instagram`.

### UI Page (Settings Extension)

```typescript
ctx.register.ui.page({
  id: "setup-guide",
  title: "Setup Guide",
  component: "./components/settings/AfterSettings.vue",
  slot: "after-settings", // or "before-settings" | "standalone"
  icon: "book",
});
```

Requires the `ui` capability and a `vite.config.ui.ts` that builds `components/` into `dist/ui.js` (UMD, Vue externalized).

---

## Channel Plugins

`package.json`: `"category": "channel"`, `"channel": "<slug>"`, `"capabilities": ["messages", "customers"]`.

### Inbound: Forward a Message to Core

The worker gets `HAY_API_URL` + `HAY_API_TOKEN` env vars; call the plugin-api tRPC endpoints over HTTP (copy the small `PluginApiClient` from `plugins/core/instagram/src/plugin-api.ts` — it is not yet an SDK export):

```typescript
const result = await apiClient.mutation<{
  messageId: string;
  conversationId: string;
  processed: boolean;
}>("messages.receive", {
  from: `instagram:${senderPsid}`, // external customer id
  content: text,
  channel: "instagram",
  senderType: "customer", // or "human_agent"
  metadata: { mid: message.mid }, // mid enables core-side dedupe (Redis SETNX)
});
```

Core finds/creates the Customer (keyed on `external_id` per channel) and the conversation. Without `metadata.mid` you get **no** inbound dedupe.

### Outbound: Implement `/deliver`

Core POSTs `{ to, content, messageId, conversationId, conversationMetadata, messageMetadata }` to your registered `POST /deliver` route:

```typescript
ctx.register.route("POST", "/deliver", async (req, res) => {
  const { to, content } = req.body;
  const providerMessageId = await sendToProvider(to, content);
  res.status(200).json({ success: true, providerMessageId });
});
```

Convention: non-retryable provider errors return **HTTP 200 with `success: false`** (e.g. `{ success: false, error: "24h_window_expired" }`) to avoid retry storms. See `plugins/core/instagram/src/deliver.ts`.

Optional: implement `POST /escalate` for human-handoff notifications — core tolerates a 404 if you don't.

### Plugin → Core Callback Surface

All under `pluginApi.*`, capability-gated via the worker JWT (`server/routes/v1/plugin-api/trpc.ts`):

`messages.receive` · `messages.send` · `messages.getByConversation` · `conversations.updateStatusByExternalId` · `customers.get` · `customers.findByExternalId` · `customers.upsert` · `sources.register` · `mcp.registerLocal` · `mcp.registerRemote` · `products.upsertMany` · `products.delete`

---

## Dashboard API Usage (tRPC)

```typescript
import { Hay } from "@/utils/api";

// Queries
const plugins = await Hay.plugins.getAll.query();
const plugin = await Hay.plugins.get.query({ pluginId: "hay-plugin-myservice" });
const tools = await Hay.plugins.getMCPTools.query(); // all enabled plugins for the org — no input
const status = await Hay.plugins.testConnection.query({ pluginId: "hay-plugin-myservice" });

// Mutations
await Hay.plugins.enable.mutate({ pluginId: "hay-plugin-myservice" });
await Hay.plugins.configure.mutate({
  pluginId: "hay-plugin-myservice",
  configuration: { apiKey: "new_key" },
});
await Hay.plugins.refreshMCPTools.mutate({ pluginId: "hay-plugin-myservice" });
await Hay.plugins.restart.mutate({ pluginId: "hay-plugin-myservice" });
await Hay.plugins.disable.mutate({ pluginId: "hay-plugin-myservice" });

// OAuth
await Hay.plugins.oauth.initiate.mutate({ pluginId: "hay-plugin-myservice" });
const oauthStatus = await Hay.plugins.oauth.status.query({ pluginId: "hay-plugin-myservice" });
```

Router: `server/routes/v1/plugins/index.ts`; handlers in `server/routes/v1/plugins/plugins.handler.ts`.

---

## File Structure

```
plugins/core/myservice/
├── package.json          # name = plugin ID; "type":"module"; hay-plugin block
├── tsconfig.json         # ESM, strict, exclude ["node_modules","dist","mcp"]
├── thumbnail.jpg         # plugin icon (svg > png > jpg priority)
├── src/
│   └── index.ts          # default export = defineHayPlugin(...)
├── mcp/                  # local stdio MCP server (archetype A only)
│   ├── index.js
│   └── package.json
├── components/           # Vue UI (capability "ui")
│   ├── index.ts          # barrel export
│   └── settings/*.vue
├── vite.config.ui.ts     # builds components → dist/ui.js
├── i18n/
│   ├── en.json           # required fallback
│   └── pt-BR.json        # extra locales auto-discovered
└── dist/                 # gitignored build output
```

---

## Internationalization (i18n)

`i18n/en.json` — keys must match MCP tool names and `register.config()` field names:

```json
{
  "name": "My Plugin",
  "description": "What this plugin does",
  "tools": {
    "tool_name": { "label": "Tool Display Name", "description": "What this tool does" }
  },
  "config": {
    "apiKey": { "label": "API Key", "description": "Your API key for authentication" }
  }
}
```

---

## Config Field Descriptor

```typescript
ctx.register.config({
  apiKey: {
    type: "string", // "string" | "number" | "boolean" | "json"
    label: "API Key",
    description: "Your API key",
    required: true,
    encrypted: true, // mandatory for secrets — encrypted at rest, masked in UI
    default: undefined,
    env: "SERVICE_API_KEY", // host env fallback; name must be in hay-plugin.env
  },
});
```

---

## Build & Test

```bash
# Build one plugin (from repo root — plugins are npm workspaces)
npm --workspace=plugins/core/myservice run build

# Batch-build all plugins
./scripts/build-plugins.sh

# Test a local MCP server directly
cd plugins/core/myservice/mcp
PRIVATE_API_KEY=test_key node index.js

# Run a worker manually (what core does per org)
node packages/plugin-sdk/dist/runner/index.js \
  --plugin-path=plugins/core/myservice --org-id=<orgId> --port=4001
```

Workers expose: `GET /health`, `GET /metadata`, `POST /validate-auth`, `POST /on-connected`, `POST /config-update`, `POST /disable`, `POST /cron/:name`, `POST /mcp/call-tool`, `GET /mcp/list-tools`, plus your registered routes.

---

## Common Issues

| Issue                       | Solution                                                                                                                    |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Plugin not appearing        | `package.json` must contain a `hay-plugin` key — dirs without it are skipped                                                |
| Worker won't start          | Check `hay-plugin.entry` points at built output; package must be ESM (`"type": "module"`)                                   |
| MCP tools missing           | `onStart` gates on credentials — configure the plugin first, then check worker logs                                         |
| MCP server crashes on spawn | Missing `mcp/node_modules` — installed on enable / by `build-plugins.sh`; run `npm install` in `mcp/` if you bypassed those |
| Config changes not applied  | Platform restarts MCP servers on config update; re-init closure state in `onConfigUpdate`                                   |
| Duplicate inbound messages  | Pass a provider message id as `metadata.mid` in `messages.receive`                                                          |
| Cron never fires            | Use `register.cron` — core owns the schedule; in-worker timers die with the idle-kill (~5 min)                              |

---

## Plugin ID Naming

- The plugin ID is the npm package `name`
- Integrations: `hay-plugin-<service>` (e.g. `hay-plugin-klaviyo`)
- Channels: `hay-channel-<name>` also seen (e.g. `hay-channel-instagram-meta`)
- Lowercase and hyphens only

---

## Reference Plugins

| Want to build…                     | Copy from                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------- |
| Local MCP over a REST API          | `plugins/core/klaviyo` (single-file), `plugins/core/zendesk` (many tools) |
| Remote hosted MCP                  | `plugins/core/stripe` (API key), `plugins/core/hubspot` (OAuth)           |
| Channel with per-instance webhooks | `plugins/core/chatwoot` (signature verification done right)               |
| Channel with a shared webhook URL  | `plugins/core/instagram` (webhook routing + declarative token ops)        |
| Cron-based token refresh           | `plugins/core/shopify`                                                    |

Avoid copying: `email`, `email-imap`, `magento`, `woocommerce`, `whatsapp` — known anti-patterns (mock paths, missing sources, no idempotency; see `.claude/skills/build-plugin/reference/anti-patterns.md`).

---

## Security Checklist

- [ ] Mark sensitive config fields `encrypted: true`
- [ ] Verify webhook signatures over **raw bytes** with `timingSafeEqual`
- [ ] Request minimal OAuth scopes
- [ ] Never log secrets or tokens
- [ ] Let the platform handle OAuth — don't hand-roll token exchange
- [ ] Treat webhook payloads as untrusted input

---

## Resources

- **Getting Started**: [getting-started](/docs/technical/plugins/getting-started/)
- **Full API Reference**: [api-reference](/docs/technical/plugins/api-reference/)
- **Channel Guide**: [channel-registration](/docs/technical/plugins/channel-registration/)
- **Channel Architecture**: [channel-architecture](/docs/technical/plugins/channel-architecture/)
- **Canonical build guide**: `.claude/skills/build-plugin/` in the hay-core repo
- **SDK source**: `packages/plugin-sdk/types/` (hook and register signatures)
