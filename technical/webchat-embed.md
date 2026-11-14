---
layout: docs.njk
title: Webchat Embed Reference
description: The webchat embed code, its config options, the browser API, and the public config the widget loads.
section: technical
navGroup: Core Systems
navOrder: 3
---

# Webchat Embed Reference

The webchat is added to a website with a small loader script, `widget.js`, configured through `window.HayChat.config`. The loader draws the chat button, fetches the organization's public webchat config, and downloads the rest of the chat when it is needed. Merchants get a ready-made snippet from **Channels** → **Webchat** → **Install** in the dashboard (see the [Webchat user guide](/docs/user-guide/webchat/)).

Source of truth: `webchat/src/types/index.ts` (`HayChatConfig`, `PublicConfig`) and `server/services/webchat/webchat-settings.service.ts` (`getPublicConfig`).

## The snippet

```html
<script>
  window.HayChat = window.HayChat || {};
  window.HayChat.config = {
    organizationId: "YOUR_ORG_ID",
    baseUrl: "https://hay.example.com",
  };
</script>
<script src="https://hay.example.com/v1/webchat/widget.js" async></script>
```

`baseUrl` is the Hay server URL; `widget.js` is served from `{baseUrl}/v1/webchat/widget.js`. Place the snippet before `</body>` on every page.

## Precedence

Most options are also set in the dashboard. A value given in the embed config wins over the dashboard value; leave an option out to use the dashboard's. The dashboard snippet sets only `organizationId` and `baseUrl`, so every dashboard change reaches the site without updating the code.

## Config options

| Option                        | Type                                                      | Default                                       | Description                                                                                                                                                                                                                     |
| ----------------------------- | --------------------------------------------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `organizationId`              | `string`                                                  | required                                      | The organization the chat belongs to.                                                                                                                                                                                           |
| `baseUrl`                     | `string`                                                  | required                                      | Hay server URL.                                                                                                                                                                                                                 |
| `widgetTitle`                 | `string`                                                  | dashboard **Chat title**, else "Chat with us" | Header title when the assistant has no name.                                                                                                                                                                                    |
| `position`                    | `"left" \| "right"`                                       | dashboard **Button position**, else `"right"` | Corner of the chat button.                                                                                                                                                                                                      |
| `brandColor`                  | `string` (`"#rrggbb"`)                                    | dashboard **Brand colour**, else `"#001df5"`  | Any hex colour. The widget derives readable text and accent colours from it; the button keeps the exact colour.                                                                                                                 |
| `themeMode`                   | `"light" \| "dark" \| "system"`                           | dashboard **Theme**, else `"light"`           | `"system"` follows the visitor's `prefers-color-scheme` (shown as **Match the visitor** in the dashboard).                                                                                                                      |
| `logoUrl`                     | `string`                                                  | dashboard logo                                | Logo shown in the header and on the home view.                                                                                                                                                                                  |
| `homeHeading`                 | `string`                                                  | dashboard **Heading**                         | Heading of the home view.                                                                                                                                                                                                       |
| `showGreeting`                | `boolean`                                                 | dashboard, else `true`                        | Start new conversations with the greeting message.                                                                                                                                                                              |
| `greetingMessage`             | `string`                                                  | dashboard **Greeting**                        | The greeting text.                                                                                                                                                                                                              |
| `teaserMessage`               | `string`                                                  | dashboard **Message next to the chat button** | Short message beside the closed button until the visitor opens the chat or dismisses it. `""` shows none on this page. With `loading: "interaction"` only a message given here shows.                                           |
| `csatEnabled`                 | `boolean`                                                 | dashboard, else `true`                        | Ask for a 1–5 rating (five faces) when a conversation ends.                                                                                                                                                                     |
| `csatQuestion`                | `string`                                                  | dashboard, else "How did we do?"              | Question above the rating.                                                                                                                                                                                                      |
| `context`                     | `Record<string, unknown>`                                 | —                                             | Public context for the AI prompt. See the [Context API](/docs/technical/context-api/).                                                                                                                                          |
| `customerExternalId`          | `string`                                                  | —                                             | Your system's ID for a signed-in user. Only used together with a valid signature.                                                                                                                                               |
| `customerExternalIdSignature` | `string`                                                  | —                                             | Hex HMAC-SHA256 of `customerExternalId`, keyed with the organization's identity secret (**Install** → **Signed-in customers**). Compute it on your server.                                                                      |
| `locale`                      | `string`                                                  | browser language                              | Widget UI language, e.g. `"en-us"`, `"pt-br"`, `"fr-fr"`.                                                                                                                                                                       |
| `consent`                     | `"strict" \| "implicit"`                                  | `"implicit"`                                  | Device storage consent. `"implicit"` enables persistent storage once the visitor sends their first message; `"strict"` waits for `HayChat.grantConsent()`. Nothing is stored before the first message in either mode.           |
| `loading`                     | `"eager" \| "idle" \| "interaction"`                      | `"idle"`                                      | When the chat (everything but the button) is downloaded. `"interaction"` fetches nothing, not even the public config, until the visitor reaches for the button, so the button paints in `brandColor` or the default until then. |
| `onConversationStarted`       | `(conversation: { id: string }) => void \| Promise<void>` | —                                             | Called when the first message creates a conversation. If it returns a Promise, that first message is sent once it resolves.                                                                                                     |
| `debug`                       | `boolean`                                                 | `false`                                       | Log widget diagnostics to the browser console.                                                                                                                                                                                  |

Removed: `theme` (the four colour presets) and `widgetSubtitle`. Use `brandColor` and `themeMode`; the header's second line now comes from the organization's availability ("Typically replies in …").

## Browser API

Available on `window.HayChat` once `widget.js` has run (calls made before the chat chunk loads are queued and replayed in order):

| Method                                        | Description                                                                                                         |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `addContext(key, value)`                      | Add or update public context (see the [Context API](/docs/technical/context-api/)).                                 |
| `grantConsent()` / `revokeConsent()`          | Give or withdraw consent for device storage, e.g. from a cookie banner. Revoking wipes persistent `hay-*` storage.  |
| `setTeaserMessage(message)`                   | Show or replace the message next to the button; `""` removes it.                                                    |
| `registerTool(tool)` / `unregisterTool(name)` | Register a page tool (WebMCP shape) the AI may call in this browser. Needs **Allow page actions** in the dashboard. |
| `highlight(target, label?)`                   | Draw the AI highlight around an element while a page tool acts on it.                                               |

Device storage consent also gates the visitor's conversation history: the home view's **Your conversations** list is only fetched after consent.

## Public config

On load the widget fetches `webchat.getPublicConfig` for the organization. It carries no secrets:

| Field                                              | Description                                                                                              |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `enabled`                                          | `false` when the chat is switched off or the organization is suspended; the loader removes the button.   |
| `widgetTitle`, `position`                          | From the dashboard.                                                                                      |
| `brandColor`                                       | Hex `#rrggbb`. Imported from the organization website's icon unless the merchant picked one.             |
| `themeMode`                                        | `"light"`, `"dark"` or `"system"`.                                                                       |
| `logoUrl`                                          | The webchat logo, else the organization logo, else `null` (the widget shows the organization's initial). |
| `homeHeading`                                      | Home view heading; `null` means the localized "How can we help?".                                        |
| `team`                                             | Up to two `{ firstName, avatarUrl }` of people who can take over; `[]` when handoff is off.              |
| `organizationName`                                 | The organization display name, shown as "Support team · {name}" while a person has taken over.           |
| `showGreeting`, `greetingMessage`, `teaserMessage` | From the **Content** tab.                                                                                |
| `csatEnabled`, `csatQuestion`                      | End-of-chat rating.                                                                                      |
| `agentName`, `agentAvatarUrl`                      | The assistant identity.                                                                                  |
| `aiLabelShown`, `aiLabelText`                      | The AI label next to the assistant's name (set in **Customer Privacy**).                                 |
| `preChatForm`                                      | The pre-chat form schema, or `null`.                                                                     |
| `contributions`                                    | Declarative plugin contributions the widget supports (for example a bot challenge). Never code.          |

`organizationLogoUrl`, `theme` and `widgetSubtitle` are no longer returned.

The public config request also records install detection: while the chat is switched on, the request's `Origin` (or `Referer`) and time are stored as the webchat's last-seen site and time, at most once a minute per organization and only for sites in the **Allowed websites** list. The dashboard shows them as **Working · Seen on {host} {time}**. Requests with `Origin: null` (such as the dashboard's sandboxed preview) are ignored.

## Live updates in the widget

Besides messages, the conversation socket sends the widget:

| Frame                                                                                     | Meaning                                                                                                                                                                                                              |
| ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `{ type: "message_ack", messageId, clientMessageId?, status: "received" \| "delivered" }` | A visitor message was stored (one tick), then picked up by the AI or queued for the team (two ticks).                                                                                                                |
| `{ type: "message_read", messageId, readAt }`                                             | A person opened the conversation in the inbox (shown as **Seen**). Not sent for the AI and not stored.                                                                                                               |
| `{ type: "agent_activity", state: "start" \| "end", toolName, label, doneLabel }`         | The AI started or finished a tool. `label` / `doneLabel` come from the tool definition ([plugin tool progress labels](/docs/technical/plugins/api-reference/#tool-progress-labels)), else "Working on it…" / "Done". |

AI messages carry `metadata.sources` (`{ title, url }[]`, only documents with a public URL; absent when none). Human agent messages and join/leave notices carry `metadata.agentAvatarUrl`.

Visitors rate an AI answer with `publicConversations.rateMessage({ conversationId, messageId, rating: "up" | "down", proof })`. Only sent AI answers of the visitor's own conversation can be rated, and rating again replaces the earlier rating. Ratings are stored as message feedback with source `visitor` (`good` / `bad`) and show in Feedback Insights. `publicConversations.listMine` returns the visitor's recent conversations (max 20, newest first) for the home view, only after consent.
