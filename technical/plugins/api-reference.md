---
layout: docs.njk
title: API Reference
description: Complete reference for the Hay plugin SDK and plugin API endpoints
section: technical
navGroup: Plugin Development
navOrder: 2
---

# Hay Plugin API Reference

> **The complete reference for the `@hay/plugin-sdk` surface, the plugin worker contract, and the plugin-related API endpoints**

Hay plugins are **code-first**: a plugin is an npm package whose `package.json` carries a `hay-plugin` block and whose entry module default-exports the result of `defineHayPlugin(...)`. There is **no `manifest.json`** — if you find docs or examples referencing one, they describe the old system and are stale.

## Table of Contents

- [Plugin Model](#plugin-model)
- [The `hay-plugin` Package Block](#the-hay-plugin-package-block)
- [defineHayPlugin and Lifecycle Hooks](#definehayplugin-and-lifecycle-hooks)
- [Hook Contexts](#hook-contexts)
- [The register API](#the-register-api)
  - [register.config](#registerconfig)
  - [register.auth](#registerauth)
  - [register.route](#registerroute)
  - [register.ui.page](#registeruipage)
  - [register.cron](#registercron)
  - [register.webhookRouting](#registerwebhookrouting)
- [Runtime APIs](#runtime-apis)
  - [ctx.config](#ctxconfig)
  - [ctx.auth](#ctxauth)
  - [ctx.mcp](#ctxmcp)
- [Worker HTTP Contract](#worker-http-contract)
- [Plugin → Core Callback API](#plugin--core-callback-api)
- [Channel Delivery Contract](#channel-delivery-contract)
- [Dashboard tRPC Endpoints](#dashboard-trpc-endpoints)
- [Internationalization (i18n)](#internationalization-i18n)
- [Migrating from the Old Manifest System](#migrating-from-the-old-manifest-system)

---

## Plugin Model

### Discovery

A directory under `plugins/core/<name>/` (or `plugins/custom/{orgId}/<name>/`) is recognized as a plugin **if and only if its `package.json` contains a `hay-plugin` key**. Discovery lives in `server/services/plugin-manager.service.ts` (`scanPluginDirectory` → `registerPlugin`); directories without the block are skipped.

- **Plugin ID** = the npm package `name` (e.g. `hay-plugin-klaviyo`, `hay-channel-instagram-meta`).
- **Entry point** = `hay-plugin.entry` (e.g. `./dist/index.js`), which must default-export `defineHayPlugin(factory)`.
- Packages must be **ESM** (`"type": "module"`) — the loader dynamically `import()`s them.
- The SDK is always referenced as `"@hay/plugin-sdk": "file:../../../packages/plugin-sdk"` (note the `packages/` segment).

### Execution: one worker per org per plugin

Plugins do **not** run inside the core process. For each `(organizationId, pluginId)` pair, core lazily spawns a separate HTTP worker process:

```
node packages/plugin-sdk/dist/runner/index.js --plugin-path=… --org-id=… --port=…
```

(`server/services/plugin-runner.service.ts`). Key facts about workers:

- **Config and auth are injected as env JSON** (`HAY_ORG_CONFIG`, `HAY_ORG_AUTH`) — never as raw provider env vars. Read them through `ctx.config` / `ctx.auth`.
- Each worker receives `HAY_API_URL` and a scoped JWT in `HAY_API_TOKEN` (generated with the plugin's declared capabilities) for calling back into core.
- Core waits for the worker's `GET /metadata` endpoint before considering it started.
- Workers are **idle-killed after 5 minutes** of inactivity (checked every minute, `server/services/plugin-instance-manager.service.ts`). Never rely on the process staying alive — this is why crons are scheduled by core, not the worker.

### Canonical directory layout

```
plugins/core/<name>/
├── package.json          # name = plugin ID; "type": "module"; hay-plugin block
├── tsconfig.json         # ESM, strict, exclude ["node_modules", "dist", "mcp"]
├── thumbnail.svg         # plugin icon, served at /plugins/thumbnails/:pluginId (svg > png > jpg)
├── src/index.ts          # default export = defineHayPlugin(...)   [REQUIRED]
├── mcp/                  # local stdio MCP server (optional)
│   ├── index.js
│   └── package.json
├── components/           # Vue UI (capability "ui")
├── vite.config.ui.ts     # builds components → dist/ui.js (UMD, vue externalized)
├── i18n/                 # en.json (+ other locales)
└── dist/                 # build output (gitignored)
```

Build from the repo root with `npm --workspace=plugins/core/<name> run build`; `scripts/build-plugins.sh` batch-builds all plugins.

---

## The `hay-plugin` Package Block

The fields core actually reads from `package.json → "hay-plugin"` (interface `HayPluginBlock` in `server/services/plugin-manager.service.ts`):

| Field              | Type       | Description                                                                                                                                                   |
| ------------------ | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `entry`            | `string`   | Path to the compiled entry module (e.g. `"./dist/index.js"`)                                                                                                  |
| `displayName`      | `string`   | Human-readable name shown in the marketplace                                                                                                                  |
| `category`         | `string`   | One of `integration \| channel \| tool \| analytics \| products` (SDK `PluginCategory`); document importers are marked via `documentImporter`, not a category |
| `capabilities`     | `string[]` | Declared capabilities: `routes`, `mcp`, `auth`, `config`, `ui`, `messages`, `customers`, `sources`, `products`, `cron`                                        |
| `env`              | `string[]` | Allow-list of host env var names that config fields may fall back to via their `env:` property                                                                |
| `channel`          | `string`   | Channel plugins only: the channel slug (e.g. `"instagram"`); matched by `pluginManagerService.findPluginIdByChannel()` for outbound delivery                  |
| `autoActivate`     | `boolean`  | Legacy (document importers)                                                                                                                                   |
| `trpcRouter`       | `string`   | Legacy (document importers): path to a tRPC router loaded in-process                                                                                          |
| `documentImporter` | `boolean`  | Legacy: marks a document importer plugin                                                                                                                      |
| `config`           | `object`   | Legacy: manifest-style config schema, copied into the registry manifest's `configSchema` (new plugins use `register.config` instead)                          |
| `auth`             | `object`   | Legacy: manifest-style auth config, copied into the registry manifest (new plugins use `register.auth.*` instead)                                             |

Capabilities are **declarative**: they drive marketplace classification, plugin type inference, and the scope of the worker's callback JWT — they are not hard-enforced against your implementation.

### Example (channel plugin)

```json
{
  "name": "hay-channel-instagram-meta",
  "type": "module",
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

### Example (integration plugin)

```json
{
  "name": "hay-plugin-klaviyo",
  "type": "module",
  "hay-plugin": {
    "entry": "./dist/index.js",
    "displayName": "Klaviyo",
    "category": "integration",
    "capabilities": ["mcp", "auth"]
  }
}
```

---

## defineHayPlugin and Lifecycle Hooks

The only factory is `defineHayPlugin` from `@hay/plugin-sdk` (not `createPlugin` or `definePlugin`). It takes a `HayPluginFactory`:

```typescript
import { defineHayPlugin } from "@hay/plugin-sdk";

export default defineHayPlugin((globalCtx) => ({
  name: "My Plugin", // the ONLY required field

  onInitialize() {
    globalCtx.register.config({
      /* ... */
    });
    globalCtx.register.auth.apiKey({
      /* ... */
    });
  },

  async onStart(ctx) {
    const apiKey = ctx.config.getOptional<string>("apiKey");
    if (!apiKey) {
      ctx.logger.warn("Not configured yet — skipping startup");
      return;
    }
    await ctx.mcp.startLocalStdio({
      /* ... */
    });
  },
}));
```

`HayPluginDefinition` (`packages/plugin-sdk/types/plugin.ts`) — `name` is required, every hook is optional:

| Hook             | Signature                                                                                         | When it runs                                                                                                                                                                                                                      |
| ---------------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `onInitialize`   | `(ctx: HayGlobalContext) => void \| Promise<void>`                                                | Once per worker process, before the HTTP server starts. **Descriptor-only**: make `register.*` calls; no network requests, no org data.                                                                                           |
| `onStart`        | `(ctx: HayStartContext) => void \| Promise<void>`                                                 | Every org runtime start/restart (first config save, config/auth updates). Read config/auth, gate on missing credentials, start MCP servers. Must never crash the worker — log errors instead.                                     |
| `onConnected`    | `(ctx: HayConnectedContext) => { routingKeys?: string[] } \| Promise<{ routingKeys?: string[] }>` | Called by core (`POST /on-connected`) right after OAuth tokens are stored. Return opaque **routing keys** that core persists for shared-webhook fan-out. Throwing never fails the OAuth flow — core logs a warning and continues. |
| `onValidateAuth` | `(ctx: HayAuthValidationContext) => boolean \| Promise<boolean>`                                  | When auth credentials are saved/updated (`POST /validate-auth`). Return `true` for valid, or throw an `Error` with a user-facing message. If not implemented, auth is assumed valid.                                              |
| `onConfigUpdate` | `(ctx: HayConfigUpdateContext) => void \| Promise<void>`                                          | After settings are saved (`POST /config-update`). Most plugins can omit this — the platform restarts registered MCP servers itself.                                                                                               |
| `onDisable`      | `(ctx: HayDisableContext) => void \| Promise<void>`                                               | On disable/uninstall (and SIGTERM). Tear down pollers, sockets, and external subscriptions.                                                                                                                                       |
| `onEnable`       | —                                                                                                 | **Typed but never called by the runner.** Reserved for future use by core. Do not rely on it.                                                                                                                                     |

---

## Hook Contexts

All context interfaces live in `packages/plugin-sdk/types/contexts.ts`.

| Context                                         | Fields                                                                                                                                                                                         |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `HayGlobalContext` (onInitialize + factory arg) | `register: HayRegisterAPI`, `config: HayConfigDescriptorAPI` (field references only — `config.field(name)`), `logger: HayLogger`                                                               |
| `HayStartContext` (onStart)                     | `org: HayOrg`, `config: HayConfigRuntimeAPI`, `auth: HayAuthRuntimeAPI`, `mcp: HayMcpRuntimeAPI`, `productSource?: HayProductSourceRuntimeAPI` (only with the `products` capability), `logger` |
| `HayConnectedContext` (onConnected)             | `org`, `config`, `auth` (fresh tokens), `logger`                                                                                                                                               |
| `HayAuthValidationContext` (onValidateAuth)     | `org`, `config`, `auth`, `logger`                                                                                                                                                              |
| `HayConfigUpdateContext` (onConfigUpdate)       | `org`, `config`, `logger`                                                                                                                                                                      |
| `HayDisableContext` (onDisable)                 | `org`, `logger`                                                                                                                                                                                |
| `HayCronContext` (cron handlers)                | `org`, `config`, `auth: HayCronAuthAPI` (adds `update()`), `logger` — no `mcp`                                                                                                                 |

The key rule: **`onInitialize` is declarative** (register schemas and descriptors), **org runtime hooks are operational** (read resolved values, start things).

---

## The register API

Available as `ctx.register` inside `onInitialize` (types in `packages/plugin-sdk/types/register.ts`). Registered metadata is exposed via the worker's `/metadata` endpoint.

### register.config

```typescript
register.config(schema: Record<string, ConfigFieldDescriptor>): void
```

Defines configuration fields; the dashboard generates the settings form from this schema. `ConfigFieldDescriptor` (`packages/plugin-sdk/types/config.ts`):

| Property      | Type                                                | Description                                                                                 |
| ------------- | --------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `type`        | `"string" \| "number" \| "boolean" \| "json"`       | Field type (required)                                                                       |
| `label`       | `string`                                            | UI label                                                                                    |
| `description` | `string`                                            | UI help text                                                                                |
| `placeholder` | `string`                                            | Input placeholder                                                                           |
| `required`    | `boolean`                                           | Show a configuration error if missing (default `false`)                                     |
| `encrypted`   | `boolean`                                           | Encrypt at rest, mask in UI and logs. **Mandatory for secrets.**                            |
| `default`     | `T`                                                 | Default value                                                                               |
| `env`         | `string`                                            | Host env var fallback. **Must be listed in `hay-plugin.env`** — validated at load time.     |
| `options`     | `Array<{ label: string; value: string \| number }>` | Renders a dropdown instead of free text                                                     |
| `showWhen`    | `ShowWhen`                                          | Declarative visibility: `{ field, equals? \| in? \| notEquals? }` referencing another field |

```typescript
register.config({
  apiKey: {
    type: "string",
    label: "API Key",
    description: "Your Klaviyo private API key",
    required: true,
    encrypted: true,
  },
  maxRetries: { type: "number", label: "Max Retries", default: 3 },
});
```

Resolution order at runtime for `ctx.config.get(key)`: org-stored value → `process.env[env]` fallback (if allow-listed) → `default`.

### register.auth

The **platform runs the entire OAuth dance** — authorization, token exchange, storage, and automatic refresh. Plugins never touch OAuth endpoints themselves.

#### register.auth.apiKey

```typescript
register.auth.apiKey(options: ApiKeyAuthOptions): void

interface ApiKeyAuthOptions {
  id: string;          // auth method id (appears as AuthState.methodId)
  label: string;       // UI label
  configField: string; // name of the register.config field holding the key
}
```

#### register.auth.oauth2

```typescript
register.auth.oauth2(options: OAuth2AuthOptions): void
```

`OAuth2AuthOptions` (`packages/plugin-sdk/types/auth.ts`):

| Property              | Type                     | Description                                                                                                                                     |
| --------------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                  | `string`                 | Auth method id                                                                                                                                  |
| `label`               | `string`                 | UI label                                                                                                                                        |
| `authorizationUrl`    | `string`                 | Provider authorize URL                                                                                                                          |
| `tokenUrl`            | `string`                 | Provider token URL                                                                                                                              |
| `scopes`              | `string[]`               | Requested scopes (optional)                                                                                                                     |
| `clientId`            | `ConfigFieldReference`   | Created with `ctx.config.field("clientId")`                                                                                                     |
| `clientSecret`        | `ConfigFieldReference`   | Created with `ctx.config.field("clientSecret")`                                                                                                 |
| `authorizationParams` | `Record<string, string>` | Extra static authorize-URL params. Reserved params (`client_id`, `redirect_uri`, `response_type`, `state`, `scope`, etc.) cannot be overridden. |
| `scopeSeparator`      | `string`                 | Scope join delimiter; defaults to a space. Instagram needs `","`.                                                                               |
| `tokenExchange`       | `OAuthTokenOp`           | Declarative one-time token transform run by core right after the code exchange (e.g. Instagram `ig_exchange_token` short→long-lived swap)       |
| `tokenRefresh`        | `OAuthTokenOp`           | Declarative custom refresh strategy for providers without a standard `refresh_token` grant (e.g. Instagram `ig_refresh_token`)                  |

```typescript
interface OAuthTokenOp {
  url: string; // endpoint to GET
  grantType: string; // value for the grant_type query param
  tokenParam: string; // query-param name carrying the current access token
  includeClientSecret?: boolean; // append client_secret (needed by token exchange)
}
```

Both token ops are executed server-side by core (`server/services/oauth.service.ts`) as a single GET request whose JSON response must include `access_token`.

```typescript
register.auth.oauth2({
  id: "oauth",
  label: "Connect with Instagram",
  authorizationUrl: "https://www.instagram.com/oauth/authorize",
  tokenUrl: "https://api.instagram.com/oauth/access_token",
  scopes: ["instagram_business_basic", "instagram_business_manage_messages"],
  scopeSeparator: ",",
  clientId: globalCtx.config.field("clientId"),
  clientSecret: globalCtx.config.field("clientSecret"),
  tokenExchange: {
    url: "https://graph.instagram.com/access_token",
    grantType: "ig_exchange_token",
    tokenParam: "access_token",
    includeClientSecret: true,
  },
  tokenRefresh: {
    url: "https://graph.instagram.com/refresh_access_token",
    grantType: "ig_refresh_token",
    tokenParam: "access_token",
  },
});
```

### register.route

```typescript
register.route(method: HttpMethod, path: string, handler: RouteHandler): void

type HttpMethod = "GET" | "POST" | "PUT" | "PATCH" | "DELETE";
type RouteHandler = (req, res) => void | Promise<void>; // Express-compatible
```

Routes are mounted on the plugin's worker HTTP server. Common uses: provider webhooks (`POST /webhook`), outbound delivery (`POST /deliver`), escalation (`POST /escalate`), callbacks. External traffic reaches them through the core proxy (`ALL /v1/plugins/:pluginId/*`), which strips credential-bearing headers before forwarding.

```typescript
register.route("POST", "/webhook", async (req, res) => {
  // verify signature, filter events, then call back into core
  res.status(200).json({ received: true });
});
```

### register.ui.page

```typescript
register.ui.page(page: PluginPage): void
```

`PluginPage` (`packages/plugin-sdk/types/ui.ts`):

| Property        | Type                                                    | Description                                                                                                                   |
| --------------- | ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `id`            | `string`                                                | Unique within the plugin                                                                                                      |
| `title`         | `string`                                                | Display title                                                                                                                 |
| `component`     | `string`                                                | Path relative to plugin root (e.g. `"./components/settings/AfterSettings.vue"`); the export name is derived from the filename |
| `slot`          | `"standalone" \| "after-settings" \| "before-settings"` | Where to render (default `"standalone"` — standalone routes are a future enhancement)                                         |
| `icon`          | `string`                                                | Optional icon identifier                                                                                                      |
| `requiresSetup` | `boolean`                                               | Only show after configuration is complete                                                                                     |

UI components are built by Vite (`vite.config.ui.ts`) into a UMD bundle at `dist/ui.js` with Vue externalized; the dashboard resolves components by export name and serves assets from `/plugins/ui/:pluginName/:assetPath`.

### register.cron

```typescript
register.cron(options: CronJobOptions): void
```

`CronJobOptions` (`packages/plugin-sdk/types/cron.ts`):

| Property      | Type                                                          | Description                                        |
| ------------- | ------------------------------------------------------------- | -------------------------------------------------- |
| `name`        | `string`                                                      | Unique within the plugin; must match `[a-z0-9_-]+` |
| `schedule`    | `string`                                                      | 5-field cron expression                            |
| `handler`     | `(ctx: HayCronContext) => void \| Promise<void>`              | Invoked when the job fires                         |
| `retryPolicy` | `{ maxRetries?: number; backoff?: "fixed" \| "exponential" }` | Optional; `backoff` is advisory                    |

**Crons are scheduled by core, not the worker** (workers are idle-killed). `server/services/plugin-cron.service.ts` registers one platform-scheduler job per enabled org per declared cron (job id `plugin-cron:<pluginId>:<orgId>:<name>`), wakes the worker when it fires, and invokes `POST /cron/:name`. Never use `setInterval` or `node-cron` inside a plugin.

Cron handlers get an extended auth API to persist refreshed credentials:

```typescript
register.cron({
  name: "refresh_token",
  schedule: "0 */20 * * *",
  handler: async (ctx) => {
    const fresh = await refreshSomehow(ctx.auth.get());
    // Buffered, returned to core, persisted encrypted; worker restarted.
    ctx.auth.update({ accessToken: fresh.accessToken, expiresAt: fresh.expiresAt });
  },
  retryPolicy: { maxRetries: 3, backoff: "exponential" },
});
```

### register.webhookRouting

```typescript
register.webhookRouting(descriptor: WebhookRoutingDescriptor): void
```

For providers that deliver **every org's events to a single shared webhook URL with no org identifier** (e.g. Meta apps). The plugin declares — as plain data, no code execution — how core should verify, answer the handshake, and route. Core executes the strategy blindly (`server/services/webhook-router.service.ts`) and fans events out to the right per-org workers' `POST /webhook`.

`WebhookRoutingDescriptor` (`packages/plugin-sdk/types/webhook-routing.ts`):

| Field                   | Description                                                                                                                                                                                                                                                                              |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `signature`             | `{ header, format: "sha256-hmac", secretEnv }` — core verifies HMAC-SHA256 over the exact raw request bytes with the secret from `process.env[secretEnv]` (must be in the `hay-plugin.env` allow-list); timing-safe compare, `sha256=` prefix stripped. Only `sha256-hmac` is supported. |
| `verificationChallenge` | Optional GET handshake: `{ modeParam, verifyTokenParam, challengeParam, verifyTokenEnv? \| verifyTokenConfigField? }` — core echoes the challenge when mode is `subscribe` and the token matches.                                                                                        |
| `routeKeyPath`          | `{ itemsPath, keyPath }` — dot-paths (no eval, no DSL) to the events array and the per-event routing key.                                                                                                                                                                                |

Routing keys are matched against the keys persisted from `onConnected` to resolve the target org.

```typescript
register.webhookRouting({
  signature: {
    header: "x-hub-signature-256",
    format: "sha256-hmac",
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

---

## Runtime APIs

Available on org runtime hook contexts (never in `onInitialize`).

### ctx.config

`HayConfigRuntimeAPI` (`packages/plugin-sdk/types/config.ts`):

```typescript
config.get<T = any>(key: string): T                       // throws/errors if required + missing
config.getOptional<T = any>(key: string): T | undefined   // undefined if not configured
config.keys(): string[]                                   // all registered field names
config.toEnv(mapping: Record<string, string>): Record<string, string>
// toEnv maps config values to env-var names for child processes;
// undefined/null values are omitted.
```

### ctx.auth

`HayAuthRuntimeAPI` (`packages/plugin-sdk/types/auth.ts`):

```typescript
auth.get(): AuthState | null

interface AuthState {
  methodId: string;                      // the id used in register.auth.*
  credentials: Record<string, unknown>;
  // API key:  { apiKey: string }
  // OAuth2:   { accessToken: string, refreshToken?: string, expiresAt?: number }
}
```

Cron handlers additionally get `auth.update(credentials, methodId?)` (see [register.cron](#registercron)).

### ctx.mcp

`HayMcpRuntimeAPI` (`packages/plugin-sdk/types/mcp.ts`) — three ways to expose MCP tools. The platform restarts registered MCP servers on config change, stops them on disable, and caches tool lists via the worker's `GET /mcp/list-tools`.

```typescript
// 1. Local stdio child process — the most common pattern.
//    cwd is relative to the plugin directory; pass credentials via env.
await ctx.mcp.startLocalStdio({
  id: "klaviyo-mcp",
  command: "node",
  args: ["index.js"],
  cwd: "./mcp",
  env: { KLAVIYO_API_KEY: ctx.config.get("apiKey") },
});

// 2. Provider-hosted (remote) MCP server.
await ctx.mcp.startExternal({
  id: "stripe-mcp",
  url: "https://mcp.stripe.com",
  authHeaders: { Authorization: `Bearer ${apiKey}` },
});

// 3. In-process MCP server (advanced).
await ctx.mcp.startLocal("my-mcp", (mcpCtx) => new MyMcpServer(mcpCtx));
```

> **Known framework gap:** nothing runs `npm install` inside `mcp/` at build time (and `node_modules` is gitignored repo-wide). Plugins with a local stdio MCP server must either ensure `mcp/node_modules` exists on the host (run `npm install` inside `mcp/` manually, as `plugins/core/klaviyo` requires) or use only Node built-ins.

---

## Worker HTTP Contract

Endpoints the SDK runner mounts on every worker (`packages/plugin-sdk/runner/http-server.ts`) — core calls these; plugins do not implement them directly:

| Endpoint              | Purpose                                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------------------------- |
| `GET /health`         | Liveness check                                                                                            |
| `GET /metadata`       | Registered config schema, auth methods, routes, UI pages, cron descriptors; core waits on this at startup |
| `POST /validate-auth` | Invokes `onValidateAuth`                                                                                  |
| `POST /on-connected`  | Invokes `onConnected` after OAuth tokens are stored                                                       |
| `POST /config-update` | Invokes `onConfigUpdate`                                                                                  |
| `POST /disable`       | Invokes `onDisable`                                                                                       |
| `POST /cron/:name`    | Invokes the named cron handler                                                                            |
| `POST /mcp/call-tool` | Executes an MCP tool                                                                                      |
| `GET /mcp/list-tools` | Lists MCP tools (cached by core)                                                                          |
| _(plugin routes)_     | Anything registered via `register.route` (e.g. `/webhook`, `/deliver`, `/escalate`)                       |

External traffic reaches worker routes only through the core proxy: `ALL /v1/plugins/:pluginId/*` (`server/routes/v1/plugins/proxy.ts`). The org is resolved from auth, subdomain, or query param — or, for a `POST /webhook` with no org identifier and a declared `webhookRouting` descriptor, the request is diverted to the shared webhook router.

---

## Plugin → Core Callback API

Workers call back into core using the injected `HAY_API_URL` + `HAY_API_TOKEN` (tRPC over HTTP). Procedures live in `server/routes/v1/plugin-api/trpc.ts` and are **capability-gated** by the worker's JWT (a plugin without the `messages` capability cannot call `messages.receive`).

| Procedure                                  | Capability  | Input (key fields)                                                                                                                                                                                                                                                                      |
| ------------------------------------------ | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `messages.receive`                         | `messages`  | `{ from, content, channel, metadata?, senderType?: "customer" \| "human_agent", externalConversationId? }` — finds/creates the customer (keyed on `external_id` per channel) and conversation, adds the message. `senderType: "human_agent"` flips the conversation to human-took-over. |
| `messages.send`                            | `messages`  | `{ to, content, channel, conversationId?, metadata? }`                                                                                                                                                                                                                                  |
| `messages.getByConversation`               | `messages`  | `{ conversationId: uuid }`                                                                                                                                                                                                                                                              |
| `conversations.updateStatusByExternalId`   | `messages`  | `{ channel, externalConversationId, status: "open" \| "pending-human" \| "human-took-over" \| "resolved" \| "closed" }`                                                                                                                                                                 |
| `customers.get`                            | `customers` | `{ customerId: uuid }`                                                                                                                                                                                                                                                                  |
| `customers.findByExternalId`               | `customers` | `{ externalId }`                                                                                                                                                                                                                                                                        |
| `customers.upsert`                         | `customers` | `{ externalId, channel, email?, phone?, name?, metadata? }`                                                                                                                                                                                                                             |
| `sources.register`                         | `sources`   | `{ id, name, category: "messaging" \| "social" \| "email" \| "helpdesk", icon?, metadata? }` — **currently a stub** (accepts input, logs, returns `{ success: true }`; persistence is a TODO in the handler)                                                                            |
| `mcp.registerLocal` / `mcp.registerRemote` | `mcp`       | Register MCP server + tool descriptors with core                                                                                                                                                                                                                                        |
| `products.upsertMany` / `products.delete`  | `products`  | Product catalog sync (e.g. Shopify); core stamps the product `source` from the authenticated plugin id                                                                                                                                                                                  |

**Inbound message dedupe is core-side**: if `metadata.mid` is set on `messages.receive`, core claims it atomically via Redis SETNX and silently drops duplicates. Channels that don't pass `mid` get no dedupe.

> The router self-describes as a simplified initial implementation ("full conversation management coming in Phase 4"). Also note: the `PluginApiClient` helper is currently duplicated per channel plugin (e.g. `plugins/core/instagram/src/plugin-api.ts`) rather than exported from the SDK — a known gap.

---

## Channel Delivery Contract

For `category: "channel"` plugins (see [Channel Registration](/docs/technical/plugins/channel-registration/) and [Channel Architecture](/docs/technical/plugins/channel-architecture/) for the full guide).

**Inbound:** provider webhook → core proxy `POST /v1/plugins/:pluginId/webhook` → worker's registered `POST /webhook` route → plugin verifies/filters → plugin calls `messages.receive` on the callback API.

**Outbound:** `server/services/channel-delivery.service.ts` subscribes to the Redis `websocket:events` channel. On `message_received` events for bot/human-agent messages with `deliveryState === "sent"` (web channel skipped), it resolves the plugin via `hay-plugin.channel`, wakes the org's worker, and posts:

```
POST /deliver
{ to, content, messageId, conversationId, conversationMetadata, messageMetadata }
```

The plugin's `/deliver` route responds with `{ success, providerMessageId?, error? }`. Convention: **non-retryable provider errors return HTTP 200 with `success: false`** (e.g. a 24-hour-window expiry) to avoid retry storms; reserve non-200 responses for retryable failures.

**Escalation:** when a conversation transitions to pending-human, core posts to the worker's `/escalate`. This is **best-effort** — plugins that don't implement it return 404, which core silently tolerates.

---

## Dashboard tRPC Endpoints

The dashboard-facing router (`server/routes/v1/plugins/index.ts`, handlers in `plugins.handler.ts`). All procedures use `authenticatedProcedure` (JWT + `x-organization-id`). Call them from the dashboard via the `Hay` client:

```typescript
import { Hay } from "@/utils/api";
```

### Queries

| Procedure                                             | Input                  | Description                                                                              |
| ----------------------------------------------------- | ---------------------- | ---------------------------------------------------------------------------------------- |
| `Hay.plugins.getAll.query()`                          | —                      | All discovered plugins with org-scoped enabled/config state                              |
| `Hay.plugins.get.query({ pluginId })`                 | `{ pluginId: string }` | One plugin's details, including current configuration                                    |
| `Hay.plugins.getInstances.query()`                    | —                      | Plugin instances for the org                                                             |
| `Hay.plugins.getUITemplate.query({ pluginId })`       | `{ pluginId }`         | Configuration UI template                                                                |
| `Hay.plugins.getMCPTools.query()`                     | —                      | Cached MCP tools across enabled plugins                                                  |
| `Hay.plugins.getMenuItems.query()`                    | —                      | Plugin-contributed menu items                                                            |
| `Hay.plugins.testConnection.query({ pluginId })`      | `{ pluginId }`         | Health check (`PluginHealthCheckResult`)                                                 |
| `Hay.plugins.getPluginTranslations.query({ locale })` | `{ locale: string }`   | i18n bundles for the org's **enabled** plugins, falling back to `en`                     |
| `Hay.plugins.oauth.isAvailable.query({ pluginId })`   | `{ pluginId }`         | `{ available, ... }` — whether OAuth is registered and client credentials are resolvable |
| `Hay.plugins.oauth.status.query({ pluginId })`        | `{ pluginId }`         | Current OAuth connection status                                                          |

### Mutations

| Procedure                                                   | Input                                                | Description                                                         |
| ----------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------- |
| `Hay.plugins.enable.mutate({ pluginId, configuration? })`   | `{ pluginId, configuration?: Record<string, any> }`  | Enable for the org (custom plugins are org-access-checked)          |
| `Hay.plugins.disable.mutate({ pluginId })`                  | `{ pluginId }`                                       | Disable; calls the worker's `/disable` (5s timeout) before teardown |
| `Hay.plugins.restart.mutate({ pluginId })`                  | `{ pluginId }`                                       | Restart the org's worker                                            |
| `Hay.plugins.configure.mutate({ pluginId, configuration })` | `{ pluginId, configuration: Record<string, any> }`   | Save configuration (encrypted fields encrypted at rest)             |
| `Hay.plugins.refreshMCPTools.mutate({ pluginId })`          | `{ pluginId }`                                       | Re-fetch tool list from the worker                                  |
| `Hay.plugins.validateAuth.mutate({ pluginId, authState })`  | `{ pluginId, authState: { methodId, credentials } }` | Proxies to the worker's `/validate-auth`                            |
| `Hay.plugins.oauth.initiate.mutate({ pluginId })`           | `{ pluginId }`                                       | Returns `{ authorizationUrl, state }` to start the flow             |
| `Hay.plugins.oauth.revoke.mutate({ pluginId })`             | `{ pluginId }`                                       | Revoke the stored connection                                        |

### Non-tRPC HTTP endpoints

- `ALL /v1/plugins/:pluginId/*` — catch-all proxy to the org's worker (webhooks, plugin routes)
- `GET /plugins/ui/:pluginName/:assetPath` — serves built plugin UI assets

---

## Internationalization (i18n)

Plugins may ship an `i18n/` directory with per-locale JSON files (`en.json` required as the fallback; e.g. `pt.json` for Portuguese). Each file supports:

```json
{
  "name": "My Plugin",
  "description": "One-sentence summary",
  "tools": { "tool_name": { "label": "…", "description": "…" } },
  "config": { "fieldName": { "label": "…", "description": "…" } }
}
```

Keys under `tools` must match MCP tool names; keys under `config` must match `register.config` field names. The dashboard fetches bundles via `Hay.plugins.getPluginTranslations.query({ locale })` — only for plugins the org has enabled — and falls back to `en` for missing locales.

---

## Migrating from the Old Manifest System

If you are updating an old plugin or reading stale docs, these are the changes:

| Old system                                                            | Current system                                                                                                                                        |
| --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `manifest.json` with id/type/capabilities/configSchema                | `package.json → "hay-plugin"` block; nothing reads a manifest file                                                                                    |
| Plugin ID declared in the manifest                                    | Plugin ID = npm package `name`                                                                                                                        |
| Types `mcp-connector`, `retriever`, `playbook`, `workflow`, `utility` | Categories: `integration \| channel \| tool \| analytics \| products` (+ legacy `documentImporter: true` flag)                                        |
| Manifest `configSchema`                                               | `register.config(...)` in `onInitialize`                                                                                                              |
| Manifest `auth` / OAuth env-var config                                | `register.auth.apiKey(...)` / `register.auth.oauth2(...)`; platform owns the whole OAuth flow, including declarative `tokenExchange` / `tokenRefresh` |
| Manifest `capabilities.api` route declarations                        | `register.route(...)`                                                                                                                                 |
| `settingsExtensions` in the manifest                                  | `register.ui.page(...)`                                                                                                                               |
| MCP `startCommand` / `installCommand` in the manifest                 | `ctx.mcp.startLocalStdio(...)` / `startExternal(...)` in `onStart`                                                                                    |
| Plugins loaded in-process                                             | Per-org HTTP workers spawned from the SDK runner, idle-killed after 5 minutes                                                                         |
| Reading credentials from `process.env` in the entry                   | `ctx.config` / `ctx.auth`; the `env:` field is only an allow-listed host fallback                                                                     |
| Self-scheduled background jobs (`setInterval`, `node-cron`)           | `register.cron(...)` — core owns the schedule and wakes the worker                                                                                    |
| SDK path `file:../../plugin-sdk`                                      | `file:../../../packages/plugin-sdk`                                                                                                                   |

Legacy paths that still exist: **document importers** (`category: "document_importer"`, `autoActivate: true`, `trpcRouter`) ride a special in-process path outside the worker model (see `plugins/core/atlassian`).

---

## Additional Resources

- **Getting Started**: [Getting Started guide](/docs/technical/plugins/getting-started/)
- **Channel plugins**: [Channel Registration](/docs/technical/plugins/channel-registration/) and [Channel Architecture](/docs/technical/plugins/channel-architecture/)
- **SDK source**: `packages/plugin-sdk/` (types under `types/`, runner under `runner/`)
- **Reference implementations**: `plugins/core/klaviyo` (local MCP), `plugins/core/stripe` / `plugins/core/hubspot` (remote MCP), `plugins/core/chatwoot` / `plugins/core/instagram` (channels)
