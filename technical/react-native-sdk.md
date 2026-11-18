---
layout: docs.njk
title: React Native SDK
description: Add the Hay chat to a React Native or Expo app with @hay-chat/react-native.
section: technical
navGroup: SDKs
navOrder: 2
---

# React Native SDK

`@hay-chat/react-native` puts the Hay chat in a React Native or Expo app. It is the same chat your website shows: same assistant, settings, languages, forms, attachments and handoff to your team.

Source: `packages/react-native` in the Hay repository.

## Install

```bash
npm install @hay-chat/react-native react-native-webview
```

With Expo, install the second package with `npx expo install react-native-webview`. The SDK has no native code of its own, so it works in Expo Go and in development builds.

## Show the chat

```tsx
import { HayChat } from "@hay-chat/react-native";

export function SupportScreen({ navigation }) {
  return (
    <HayChat
      organizationId="YOUR_ORG_ID"
      baseUrl="https://hay.example.com"
      origin="https://shop.example.com"
      onClose={() => navigation.goBack()}
    />
  );
}
```

- `organizationId` and `baseUrl` are the two values in the snippet under **Channels** → **Webchat** → **Install**.
- `origin` is one of the websites listed under **Allowed websites** on that page, with its scheme. If the list is empty, use your organization's own website.

The chat fills the view you put it in: a screen, a modal or a bottom sheet. Give that view the safe area and keyboard handling you give your other screens.

## How it works

The component renders a WebView (`react-native-webview`) with a one-line page that loads `widget.js`, the same loader websites use, in **embedded** mode: no chat button, always full screen, open from the start.

- **Allowed websites.** The page is loaded with `origin` as its address, so requests to Hay carry that site's `Origin` and the existing **Allowed websites** setting covers the app. Nothing extra has to be switched on.
- **Settings.** Colour, greeting, assistant name, forms and the rest come from the dashboard, as on the website. A change there reaches the app without an app release, and so does every improvement to the chat itself.
- **Talking to your app.** The page reports to the component when the chat is ready, when the visitor closes it, how many replies are unread and when a conversation starts. The component turns those into the props below.
- **Links.** A link the visitor taps opens in the device's browser, not inside the chat.
- **Storage.** Conversations are kept in the WebView's own storage on the device, under the same consent rules as the website (`consent`).

Because an app is not a browser, **Allowed websites** does not stop a different app from using your organization ID. Rate limits and the bot check cover that, as they do for any non-browser client.

## Signed-in customers

```tsx
<HayChat
  organizationId="YOUR_ORG_ID"
  baseUrl="https://hay.example.com"
  origin="https://shop.example.com"
  customer={{ externalId: user.id, signature: user.haySignature }}
/>
```

`signature` is the hex HMAC-SHA256 of `externalId`, keyed with the identity secret from **Install** → **Signed-in customers**. Compute it on your server and send it to the app with the rest of the user's profile:

```js
import { createHmac } from "node:crypto";

const haySignature = createHmac("sha256", process.env.HAY_IDENTITY_SECRET)
  .update(user.id)
  .digest("hex");
```

Never put the identity secret in the app. Without a valid signature the visitor is treated as anonymous.

## Unread badge

Keep the chat mounted and hide it instead of unmounting it. Replies that arrive while it is hidden are counted:

```tsx
const [open, setOpen] = useState(false);
const [unread, setUnread] = useState(0);

<HayChat
  organizationId="YOUR_ORG_ID"
  baseUrl="https://hay.example.com"
  origin="https://shop.example.com"
  visible={open}
  onClose={() => setOpen(false)}
  onUnreadChange={setUnread}
/>;
```

This works while your app is running. There are no push notifications yet: a reply that arrives after the app is closed shows the next time the chat is opened.

## Context and secrets

`context` passes non-sensitive facts to the assistant, such as the plan or the current screen. For anything sensitive, use `onConversationStarted` to tell your backend, and attach secrets from there with the server SDK (see the [Context API](/docs/technical/context-api/)):

```tsx
<HayChat
  organizationId="YOUR_ORG_ID"
  baseUrl="https://hay.example.com"
  origin="https://shop.example.com"
  context={{ plan: "pro", screen: "Orders" }}
  onConversationStarted={({ id }) => api.attachHaySecrets(id)}
/>
```

If the callback returns a promise, the visitor's first message is held until it resolves, for at most 15 seconds.

## Props

| Prop                    | Type                                                      | Description                                                                                             |
| ----------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `organizationId`        | `string`                                                  | Required. The organization the chat belongs to.                                                         |
| `baseUrl`               | `string`                                                  | Required. Hay server URL.                                                                               |
| `origin`                | `string`                                                  | Required. One of your allowed websites, e.g. `"https://shop.example.com"`.                              |
| `customer`              | `{ externalId: string; signature?: string }`              | The signed-in customer.                                                                                 |
| `context`               | `Record<string, unknown>`                                 | Public context for the AI prompt.                                                                       |
| `locale`                | `string`                                                  | Chat language, e.g. `"pt-br"`. Defaults to the device's.                                                |
| `brandColor`            | `string` (`"#rrggbb"`)                                    | Overrides the dashboard's brand colour.                                                                 |
| `themeMode`             | `"light" \| "dark" \| "system"`                           | Overrides the dashboard's theme.                                                                        |
| `consent`               | `"strict" \| "implicit"`                                  | Device storage consent, as in the [embed reference](/docs/technical/webchat-embed/).                    |
| `visible`               | `boolean`                                                 | `false` hides the chat but keeps it connected. Defaults to `true`.                                      |
| `onReady`               | `() => void`                                              | The chat is on screen and ready for a question.                                                         |
| `onClose`               | `() => void`                                              | The visitor pressed the chat's close button. Without this prop the chat stays open.                     |
| `onUnreadChange`        | `(count: number) => void`                                 | Replies that arrived while the chat was hidden.                                                         |
| `onConversationStarted` | `(conversation: { id: string }) => void \| Promise<void>` | Called when the first message creates a conversation.                                                   |
| `debug`                 | `boolean`                                                 | Log the chat's diagnostics to the WebView console.                                                      |
| `style`                 | `StyleProp<ViewStyle>`                                    | Style for the WebView.                                                                                  |

Changing `customer`, `context`, `locale`, `brandColor`, `themeMode` or `consent` reloads the chat.

## Not supported yet

- Push notifications for replies while the app is closed.
- Page tools (`registerTool`), which let the assistant act on a web page.
- Native Swift and Kotlin packages.
