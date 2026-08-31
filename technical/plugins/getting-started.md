---
layout: docs.njk
title: Getting Started
description: Build your first Hay plugin in under 30 minutes
section: technical
navGroup: Plugin Development
navOrder: 1
---

## Building Your First Plugin

Hay's plugin system lets you connect external platforms to the agent. This tutorial walks you through building a complete, working plugin from scratch using the real plugin SDK.

### The 30-Second Model

Before writing any code, understand what a Hay plugin actually is:

- A plugin is a **directory under `plugins/core/<name>/`** whose `package.json` contains a **`hay-plugin` block**. That block is the only metadata the loader reads — **there is no `manifest.json`**.
- The **plugin ID is the npm package name** (e.g. `hay-plugin-klaviyo`).
- The entry file (`src/index.ts`, compiled to `dist/index.js`) **default-exports the result of `defineHayPlugin(...)`** from `@hay/plugin-sdk`.
- Plugins do **not** run inside the core server process. For each `(organization, plugin)` pair, core spawns a **separate HTTP worker process** from the SDK runner, injects the org's config and credentials, and talks to it over HTTP. Idle workers are killed after 5 minutes and respawned on demand.
- You **declare** everything (config fields, auth methods, routes, UI) in `onInitialize`, and **wire up runtime behavior** (MCP servers, API clients) in `onStart`.

### What You'll Build

We'll build `hay-plugin-nasa` — an integration plugin that exposes NASA's public API to the agent as MCP tools. The agent will be able to:

- Fetch the Astronomy Picture of the Day
- List near-Earth asteroids for a date range

This follows **Archetype A** (local bundled MCP server), the most common plugin shape: the platform has a REST API, and you wrap it in a small Node MCP server that the plugin spawns over stdio. It's the same structure used by the `klaviyo` and `zendesk` plugins in `plugins/core/`.

NASA's API works with the shared key `DEMO_KEY`, so you can run the finished plugin end-to-end without signing up for anything.

### Prerequisites

- Node.js 18+ installed
- The `hay-core` repository checked out, with the dev environment set up and running
- Basic TypeScript knowledge

### Step 1: Create the Plugin Structure

From the repository root:

```bash
mkdir -p plugins/core/nasa/src plugins/core/nasa/mcp plugins/core/nasa/i18n
```

The finished layout will be:

```
plugins/core/nasa/
├── package.json          # name = plugin ID; contains the hay-plugin block
├── tsconfig.json
├── thumbnail.svg         # icon shown in the marketplace (svg, png, or jpg)
├── src/
│   └── index.ts          # default export = defineHayPlugin(...)
├── mcp/
│   ├── index.js          # local stdio MCP server (plain JS, not compiled)
│   └── package.json      # the MCP server's own dependencies
├── i18n/
│   └── en.json           # tool and config labels
└── dist/                 # build output (gitignored)
```

### Step 2: Write `package.json`

Create `plugins/core/nasa/package.json`:

```json
{
  "name": "hay-plugin-nasa",
  "version": "1.0.0",
  "description": "Connect NASA's open APIs: astronomy picture of the day and near-Earth objects",
  "author": "Hay",
  "type": "module",
  "main": "dist/index.js",
  "hay-plugin": {
    "entry": "./dist/index.js",
    "displayName": "NASA",
    "category": "integration",
    "capabilities": ["mcp", "auth"],
    "env": []
  },
  "scripts": {
    "build": "tsc",
    "dev": "tsc --watch"
  },
  "dependencies": {
    "@hay/plugin-sdk": "file:../../../packages/plugin-sdk"
  },
  "devDependencies": {
    "@types/node": "^20.10.0",
    "typescript": "^5.3.3"
  }
}
```

Key points:

- **`name`** is the plugin ID used everywhere (API calls, worker routing, UI asset paths). The convention is `hay-plugin-<name>` for integrations and `hay-channel-<name>-<provider>` for channels.
- **`hay-plugin`** is what makes this directory a plugin — the discovery loop in `server/services/plugin-manager.service.ts` skips any directory without it.
- **`category`** must be one of `integration | channel | tool | analytics | products`.
- **`capabilities`** are declarative: `mcp | auth | config | ui | routes | cron | products` (channels additionally use `messages`, `customers`, `sources`). They drive marketplace classification and the scope of the JWT the worker uses to call back into core.
- **`env`** is an allow-list of host environment variable names that config fields may fall back to. Leave it empty unless self-hosters need to set credentials via `process.env`.
- **`"type": "module"`** is required — the loader dynamically `import()`s the entry.
- The SDK dependency path is `file:../../../packages/plugin-sdk` (note the `packages/` segment).

### Step 3: Configure TypeScript

Create `plugins/core/nasa/tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ES2020",
    "moduleResolution": "node",
    "lib": ["ES2020"],
    "outDir": "./dist",
    "rootDir": "./src",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "declaration": true,
    "strict": true,
    "resolveJsonModule": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "mcp"]
}
```

Note that `mcp/` is excluded — the MCP server is plain runtime JavaScript, not compiled TypeScript.

### Step 4: Write the Plugin Entry

Create `plugins/core/nasa/src/index.ts`:

```typescript
/**
 * NASA Plugin
 *
 * Spawns a local Node MCP server (./mcp/index.js) that calls NASA's open API
 * directly using an API key (the shared key "DEMO_KEY" works for testing).
 */

import { defineHayPlugin } from "@hay/plugin-sdk";

const NASA_BASE_URL = "https://api.nasa.gov";

export default defineHayPlugin((globalCtx) => ({
  name: "NASA",

  // ── 1. DECLARE everything (runs once per worker, before any request). ──
  // Only descriptor calls here: config, auth, ui, routes. No network, no MCP.
  onInitialize(ctx) {
    ctx.register.config({
      apiKey: {
        type: "string",
        label: "API Key",
        description: "Your NASA API key from api.nasa.gov. Use DEMO_KEY to try it out.",
        required: true,
        encrypted: true, // secrets MUST be encrypted; never log them
      },
    });

    ctx.register.auth.apiKey({
      id: "nasa-apikey",
      label: "NASA API Key",
      configField: "apiKey",
    });

    globalCtx.logger.info("NASA plugin: config + auth registered");
  },

  // ── 2. VALIDATE with a real round-trip (called when creds change). ──
  // Return true if the credentials work, false otherwise. A thrown error is
  // also treated as invalid (its message goes to the worker log, not the UI).
  async onValidateAuth(ctx) {
    const authState = ctx.auth.get();
    if (!authState) {
      ctx.logger.warn("NASA: no authentication configured");
      return false;
    }

    const apiKey = ctx.config.get<string>("apiKey");
    if (!apiKey) {
      ctx.logger.warn("NASA: API key missing");
      return false;
    }

    const res = await fetch(
      `${NASA_BASE_URL}/planetary/apod?api_key=${encodeURIComponent(apiKey)}`,
    );
    if (!res.ok) {
      ctx.logger.warn("NASA: auth validation failed", { status: res.status });
      return false;
    }
    return true;
  },

  // ── 3. START runtime per org. GATE on credentials, then wire MCP. ──
  // Missing creds = enabled-but-idle (log + return), never crash the worker.
  async onStart(ctx) {
    const authState = ctx.auth.get();
    if (!authState) {
      ctx.logger.info("NASA: credentials not configured — enabled but MCP tools unavailable.");
      return;
    }

    const apiKey = ctx.config.get<string>("apiKey");
    if (!apiKey) {
      ctx.logger.warn("NASA: no API key in config — MCP server not started.");
      return;
    }

    await ctx.mcp.startLocalStdio({
      id: "nasa-mcp",
      command: "node",
      args: ["index.js"],
      cwd: "./mcp", // relative to the plugin directory
      env: { NASA_API_KEY: apiKey }, // creds reach the child via env
    });

    ctx.logger.info("NASA MCP server started", { orgId: ctx.org.id });
  },

  // ── 4. React to config edits. ──
  // The platform restarts registered MCP servers on config change, so for
  // pure-MCP plugins logging is enough. If you hold your OWN client/state in
  // closure variables, re-initialize it here.
  async onConfigUpdate(ctx) {
    ctx.logger.info("NASA plugin: config updated");
  },

  // ── 5. Tear DOWN (called on disable + worker shutdown). ──
  // The platform stops MCP servers for you, but anything YOU opened
  // (pollers, sockets, clients), you must close here.
  async onDisable(ctx) {
    ctx.logger.info("NASA plugin disabled", { orgId: ctx.org.id });
  },
}));
```

What's happening here:

- `defineHayPlugin(factory)` is the **only** plugin factory. The factory receives a global context (logger, plugin metadata) and returns the plugin definition. The only required field is `name`; every hook is optional.
- **`onInitialize`** runs once per worker, before the worker starts serving HTTP. It must be descriptor-only: `ctx.register.*` calls, no network requests, no org data.
- **`onValidateAuth`** is invoked by core when the user saves credentials. Do a real API round-trip and return `true`/`false`. If the hook throws, the runner catches it and treats the result as invalid — the message lands in the worker log, and the dashboard shows a generic "Auth validation failed" error. If you don't implement the hook at all, credentials are assumed valid.
- **`onStart`** runs per org whenever the worker starts (including respawns after idle-kill). Credentials may legitimately be absent — a user can enable a plugin before configuring it — so gate and return instead of throwing.
- Config and credentials arrive through `ctx.config` / `ctx.auth`, **not** through `process.env` in the entry. Core injects them into the worker; per-org secrets never sit in raw host environment variables.
- Do **not** implement `onEnable` — it exists in the SDK types but the runner never calls it.

### Step 5: Write the MCP Server

Create `plugins/core/nasa/mcp/package.json`:

```json
{
  "name": "nasa-mcp-server",
  "version": "1.0.0",
  "private": true,
  "description": "Local Node MCP server for NASA's open API.",
  "main": "index.js",
  "type": "module",
  "dependencies": {
    "@modelcontextprotocol/sdk": "^1.6.1",
    "zod": "^3.24.2"
  }
}
```

Create `plugins/core/nasa/mcp/index.js`:

```javascript
/**
 * Local MCP server for NASA (plugins/core/nasa/mcp/index.js)
 *
 * Plain runtime JS. Spawned over stdio by the entry's ctx.mcp.startLocalStdio.
 * stdout is reserved for JSON-RPC — log ONLY to console.error.
 */

import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

const API_KEY = process.env.NASA_API_KEY;
if (!API_KEY) {
  console.error("[nasa-mcp] NASA_API_KEY missing — cannot start.");
  process.exit(1);
}

const BASE_URL = "https://api.nasa.gov";

/** Single request helper: centralizes auth + error normalization. */
async function api(path, query = {}) {
  const url = new URL(BASE_URL + path);
  url.searchParams.set("api_key", API_KEY);
  for (const [k, v] of Object.entries(query)) {
    if (v === undefined || v === null || v === "") continue;
    url.searchParams.set(k, String(v));
  }
  const res = await fetch(url, { headers: { Accept: "application/json" } });
  const text = await res.text();
  if (!res.ok) {
    throw new Error(`NASA GET ${path} failed: ${res.status} ${text}`);
  }
  return text ? JSON.parse(text) : null;
}

/** Standard tool responses. */
const ok = (payload) => ({
  content: [{ type: "text", text: JSON.stringify(payload, null, 2) }],
});
const fail = (err) => ({
  content: [{ type: "text", text: `Error: ${err.message || String(err)}` }],
  isError: true,
});

const server = new McpServer({ name: "nasa-mcp-server", version: "1.0.0" });

server.tool(
  "get_astronomy_picture",
  "Fetch NASA's Astronomy Picture of the Day (APOD): title, explanation, and image URL. " +
    "Omit the date to get today's picture.",
  {
    date: z.string().optional().describe("Date in YYYY-MM-DD format. Defaults to today."),
  },
  async ({ date }) => {
    try {
      return ok(await api("/planetary/apod", { date }));
    } catch (err) {
      return fail(err);
    }
  },
);

server.tool(
  "list_near_earth_objects",
  "List asteroids approaching Earth between two dates (max 7 days apart), with size " +
    "and closest-approach data. Use get_astronomy_picture for imagery instead.",
  {
    start_date: z.string().describe("Start date in YYYY-MM-DD format."),
    end_date: z
      .string()
      .describe("End date in YYYY-MM-DD format, at most 7 days after start_date."),
  },
  async ({ start_date, end_date }) => {
    try {
      return ok(await api("/neo/rest/v1/feed", { start_date, end_date }));
    } catch (err) {
      return fail(err);
    }
  },
);

await server.connect(new StdioServerTransport());
console.error("[nasa-mcp] server connected over stdio.");
```

The rules baked into this file matter:

- Use `@modelcontextprotocol/sdk` — never hand-roll JSON-RPC over readline.
- Credentials come in via `process.env` here (they were passed by `startLocalStdio`'s `env` option) — this is the one place `process.env` is correct.
- `stdout` carries JSON-RPC; anything you `console.log` corrupts the protocol. Log to `console.error`.
- Rich tool descriptions and zod schemas with `.describe()` on every parameter are what make the agent call your tools correctly. Cross-tool hints ("use X for Y instead") help even more.

**How `mcp/` dependencies get installed:** the `mcp/` directory is **not** part of the npm workspace, so its dependencies are installed separately from the plugin root's. Two paths handle it for you:

- Enabling the plugin from the marketplace runs the install step (`installPlugin` in `server/services/plugin-manager.service.ts`), which detects `mcp/package.json` and runs `npm install --ignore-scripts` inside `mcp/`.
- `scripts/build-plugins.sh` also installs `mcp/` dependencies when `mcp/node_modules` is missing.

If you're iterating locally before enabling, you can install them yourself:

```bash
cd plugins/core/nasa/mcp
npm install
```

`mcp/node_modules` is gitignored (commit `mcp/package-lock.json`, not the modules). Keep the dependency list minimal — native `fetch` avoids needing an HTTP client library at all.

### Step 6: Add Translations

Create `plugins/core/nasa/i18n/en.json`. The keys under `tools` **must exactly match your MCP tool names** — the dashboard resolves tool chips, the tool picker, and config labels from this file. Without it, the UI falls back to mechanically humanized names.

```json
{
  "name": "NASA",
  "description": "Astronomy picture of the day and near-Earth asteroid data from NASA's open APIs.",
  "tools": {
    "get_astronomy_picture": {
      "label": "Get Astronomy Picture",
      "description": "Fetch NASA's Astronomy Picture of the Day."
    },
    "list_near_earth_objects": {
      "label": "List Near-Earth Objects",
      "description": "List asteroids approaching Earth in a date range."
    }
  },
  "config": {
    "apiKey": {
      "label": "API Key",
      "description": "Your NASA API key from api.nasa.gov. Use DEMO_KEY to try it out."
    }
  }
}
```

Every `*.json` file in `i18n/` is loaded automatically — add `pt.json` (or other locales) alongside `en.json`; missing locales fall back to English.

### Step 7: Add a Thumbnail

Drop an icon at the plugin root. Core looks for `thumbnail.svg`, `thumbnail.png`, or `thumbnail.jpg` (in that priority order) and serves it in the marketplace.

### Step 8: Build

From the **repository root** (so the `file:` link to the SDK resolves):

```bash
npm install --workspace=plugins/core/nasa
npm run build --workspace=plugins/core/nasa
```

Confirm `plugins/core/nasa/dist/index.js` exists.

(Enabling from the marketplace also runs install and build automatically when they're missing — building yourself just surfaces TypeScript errors earlier.)

### Step 9: Enable and Test

1. Start (or restart) the Hay server so plugin discovery picks up the new directory.
2. In the dashboard, open **Integrations → Marketplace**. Your plugin appears with its display name and thumbnail.
3. Enable it, then open its settings page (`/integrations/plugins/hay-plugin-nasa`) and enter `DEMO_KEY` as the API key. Saving triggers `onValidateAuth` — an invalid key fails validation and the dashboard shows an auth error.
4. On first use, core spawns the worker, waits for its `/metadata` endpoint, and calls your lifecycle hooks. Watch the server logs for your `onStart` log lines.
5. The agent can now see and call `get_astronomy_picture` and `list_near_earth_objects`. Ask it in a test conversation: _"What's today's astronomy picture?"_

Core caches the tool list from the worker's `GET /mcp/list-tools`; the dashboard's `plugins.refreshMCPTools` mutation re-fetches it if you add tools later.

### How It Runs (Recap)

```
core server
  └─ spawns per (org, plugin):  node packages/plugin-sdk/dist/runner/index.js
       ├─ env: HAY_ORG_CONFIG, HAY_ORG_AUTH   (your config + credentials)
       ├─ env: HAY_API_URL, HAY_API_TOKEN     (scoped JWT to call back into core)
       ├─ HTTP: /metadata /validate-auth /config-update /disable /mcp/*  ← core calls these
       └─ your onStart → spawns mcp/index.js over stdio
```

Workers are killed after 5 minutes of inactivity and respawned on demand — never rely on in-memory state surviving between requests, and never schedule work with `setInterval` inside a plugin (the platform provides `register.cron` for that; see the [API Reference](/docs/technical/plugins/api-reference/)).

## Next Steps

- **[Plugin API Reference](/docs/technical/plugins/api-reference/)** — full `register.*` API, lifecycle hooks, OAuth2, crons, UI pages
- **[Quick Reference](/docs/technical/plugins/quick-reference/)** — handy cheat sheet
- **[Channel Architecture](/docs/technical/plugins/channel-architecture/)** — how messaging-channel plugins work
- **[Channel Registration](/docs/technical/plugins/channel-registration/)** — building a channel plugin (Archetype C)

### Beyond Archetype A

The plugin you just built is one of four archetypes:

| Your platform…                | Archetype                | Good reference in `plugins/core/`                                                             |
| ----------------------------- | ------------------------ | --------------------------------------------------------------------------------------------- |
| has a REST API, no hosted MCP | **A. Local bundled MCP** | `klaviyo` (clean), `zendesk` (many tools)                                                     |
| hosts its own MCP server      | **B. Remote connector**  | `hubspot` (OAuth), `stripe` (API key) — use `ctx.mcp.startExternal` instead of a local server |
| is two-way messaging          | **C. Channel**           | `chatwoot` (signature verification), `instagram` (shared-app OAuth)                           |
| is a document source          | **D. Document importer** | `atlassian` (advanced; rides a legacy path outside the worker model)                          |

For OAuth2 platforms, note that **the platform runs the entire OAuth flow** — authorization, token exchange, and refresh. You declare the endpoints with `ctx.register.auth.oauth2(...)` and read tokens from `ctx.auth.get()`; the plugin never touches OAuth endpoints itself.

## Troubleshooting

### Plugin not appearing in the marketplace

1. `package.json` must contain the `hay-plugin` block — directories without it are skipped by discovery.
2. `dist/index.js` must exist (run the build) and match `hay-plugin.entry`.
3. Restart the server after adding a new plugin directory.

### Enabled, but no tools

1. Credentials not configured yet — `onStart` gates on `ctx.auth.get()` and idles until they exist. Check the settings page.
2. The MCP server failed to spawn — a common cause is missing `mcp/node_modules` (installed automatically on enable; see Step 5 — run `npm install` in `mcp/` if you bypassed that flow). Check server logs for the worker's stderr.
3. Tool cache is stale — use the "refresh tools" action (`plugins.refreshMCPTools`).

### `onValidateAuth` never fires

It's only called when auth fields change on save (`plugins.configure` checks for auth changes and calls the worker's `/validate-auth` — and only if the worker is running), or via the explicit `plugins.validateAuth` mutation. If you don't implement it, auth is assumed valid — implement it with a real API round-trip.

### Worker seems to "forget" state

Workers are idle-killed after 5 minutes. Persist anything important via config/auth or core callbacks; re-establish runtime state in `onStart`.
