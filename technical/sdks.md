---
layout: docs.njk
title: SDKs
description: The ways to put Hay in your website, app or backend, and which one to pick.
section: technical
navGroup: SDKs
navOrder: 1
---

# SDKs

Hay has one chat, and each SDK is a way to reach it from a different place. Pick by where your customers are.

| SDK                                                  | Use it for                                    | How you get it                                                         |
| ---------------------------------------------------- | --------------------------------------------- | ---------------------------------------------------------------------- |
| [JavaScript (web)](/docs/technical/webchat-embed/)   | Websites and web apps, in any framework       | A script tag, copied from **Channels** → **Webchat** → **Install**     |
| [React Native](/docs/technical/react-native-sdk/)    | iOS and Android apps built with React Native or Expo | `npm install @hay-chat/react-native react-native-webview`       |
| [Server (Node.js)](/docs/technical/context-api/)     | Your backend: attach context and secrets to a conversation or customer | `@hay/server-sdk`, in `packages/server-sdk` of the Hay repository |

All three talk to the same organization, so the assistant, playbooks, documents, languages and inbox are shared. A setting changed in the dashboard reaches the website and the app without a release.

## JavaScript (web)

A small loader script, `widget.js`, configured through `window.HayChat.config`. It works the same in plain HTML, React, Vue, Next.js or any other web stack: there is no framework-specific package to install. In a single-page app, add the snippet once in the page shell.

- [Webchat Embed Reference](/docs/technical/webchat-embed/): the snippet, every config option and the `window.HayChat` browser API.
- [Webchat user guide](/docs/user-guide/webchat/): the dashboard settings.

## React Native

A `HayChat` component for React Native and Expo apps. It shows the web chat inside your app, full size in whatever screen or modal you put it in.

- [React Native SDK](/docs/technical/react-native-sdk/): install, usage, props and how it works.

There are no native Swift or Kotlin packages yet.

## Server (Node.js)

A small client for your backend. It attaches data to a conversation or a customer with your API key: public context the assistant can read, and secrets that are never shown to the model and are only passed to tool calls.

- [Context API](/docs/technical/context-api/): when to use context and secrets, with examples.

It pairs with both chat SDKs: the chat tells your code when a conversation starts (`onConversationStarted`), your code tells your backend, and your backend attaches what the assistant needs.

## Signed-in customers

On the website and in the app, a visitor is anonymous unless you identify them. Pass your own ID for the user together with a signature computed on your server: the hex HMAC-SHA256 of the ID, keyed with the identity secret from **Install** → **Signed-in customers**. The secret stays on your server. It must never be in a web page or inside an app, because anything shipped to a device can be read out of it.
