---
layout: docs.njk
title: WhatsApp
description: Connect your WhatsApp Business number directly through Meta's Cloud API so Hay's AI agent can answer your customers' WhatsApp messages automatically.
section: user-guide
---

## What is WhatsApp?

WhatsApp is the world's most widely used messaging app, and for many customers it's the most natural way to contact a business. Connecting WhatsApp to Hay routes messages sent to your WhatsApp Business number into your Hay inbox, where your AI agent answers them automatically, 24/7.

## How to Connect WhatsApp

Hay connects **directly to Meta's WhatsApp Cloud API** — no third-party messaging provider sits in between, and Meta bills you for conversations on your own WhatsApp Business account.

1. In the Hay dashboard, go to **Integrations** → **Marketplace**
2. Find **WhatsApp**, enable it and open its settings
3. Click **Connect WhatsApp** and complete Meta's signup window. Log in with Facebook, pick or create your business portfolio and WhatsApp Business account, then add and verify the phone number to connect. Numbers already used in the WhatsApp Business app can stay on your phone: Meta offers this in the window and asks you to scan a QR code from the app
4. Back in Hay, the settings page shows your connected number and its health:
   - **Can send messages:** Available, Limited or Blocked, with Meta's reason
   - **Quality rating**
   - **Messaging limit:** how many new customers you can message per day
5. If the settings page says no payment method is on file, click **Add payment method in WhatsApp Manager**, add a card there, and come back. Hay re-checks automatically

Then assign the channel to an agent: open your agent's settings, and under **Channels** select WhatsApp.

**Who pays for messages?** Meta bills your own WhatsApp Business account directly; Hay adds no messaging fee. Each phone number gets 1,000 free service messages (replies inside the 24-hour window) per month. Beyond that, and for any message you start with a template, Meta needs a payment method on file.

**Self-hosted, or connecting before Hay's Meta app approval?** Choose **Manual token** as the connection method and paste a permanent system-user token from your own Meta app, your phone number ID, WhatsApp Business Account ID, app secret and a verify token. The settings page shows the webhook URL to enter in your Meta app. The plugin README on GitHub has the step-by-step guide.

## What Hay Can Do with WhatsApp

- **Receive messages** — text, button replies, locations, shared contacts, images, videos, documents and voice notes appear as conversations in your Hay inbox, with the customer's WhatsApp profile name and phone number attached to the customer record
- **Voice notes** — Hay transcribes voice notes, so the agent can answer them; the original audio stays in the conversation
- **Delivery status** — replies show as sent, delivered or read, and a failed reply shows why
- **AI replies** — the agent assigned to the WhatsApp channel answers automatically, and replies (including files) are delivered back to the customer's WhatsApp chat
- **Human takeover** — your team can step in on any WhatsApp conversation from the Conversations view, just like any other channel

**24-hour reply window.** WhatsApp only allows businesses to send free-form replies within 24 hours of the customer's last message. If that window has closed, the conversation shows a notice and the reply waits until the customer writes again — this is a WhatsApp rule, not a Hay setting.

**Disconnecting.** Click **Disconnect** on the plugin's settings page. If you remove Hay from your business in Meta Business Settings, or disconnect it in the WhatsApp Business app, Hay notices and marks the channel disconnected; reconnect from the settings page.
