---
layout: docs.njk
title: WhatsApp
description: Connect WhatsApp through Twilio so Hay's AI agent can answer your customers' WhatsApp messages automatically.
section: user-guide
---

## What is WhatsApp?

WhatsApp is the world's most widely used messaging app, and for many customers it's the most natural way to contact a business. Connecting WhatsApp to Hay routes messages sent to your WhatsApp Business number into your Hay inbox, where your AI agent answers them automatically, 24/7.

## How to Connect WhatsApp

Hay connects to WhatsApp through **Twilio**, so you need a Twilio account with a WhatsApp-enabled phone number before you start.

1. In the Hay dashboard, go to **Integrations** → **Marketplace**
2. Find **WhatsApp** and click **Connect**
3. Enter your **Twilio Account SID** — it starts with "AC" and is shown on your Twilio Console dashboard at console.twilio.com
4. Enter your **Twilio Auth Token** — found right next to the Account SID in the Twilio Console
5. Enter your **WhatsApp Number** — your Twilio WhatsApp-enabled phone number in international E.164 format (for example, +14155238886). Just the number: Hay adds the "whatsapp:" prefix for you
6. Save. Hay verifies the credentials against Twilio immediately, so you'll know right away if something was mistyped

Then assign the channel to an agent: open your agent's settings, and under **Channels** select WhatsApp. Incoming messages are then routed to that agent for automatic replies.

**Self-hosted?** Credentials can also be supplied through the platform environment instead of the dashboard. See the [plugin README on GitHub](https://github.com/hay-chat/hay-core/tree/master/plugins/core/whatsapp) for details.

## What Hay Can Do with WhatsApp

- **Receive messages** — messages sent to your WhatsApp number appear as conversations in your Hay inbox, with the customer's WhatsApp profile name and phone number attached to the customer record. Every incoming message is verified as genuinely coming from Twilio before it's processed
- **AI replies** — the agent assigned to the WhatsApp channel answers automatically, and replies are delivered back to the customer's WhatsApp chat
- **Human takeover** — your team can step in on any WhatsApp conversation from the Conversations view, just like any other channel

**Current limitations (worth knowing):**

- **Text only.** Hay sends and receives plain-text messages. Rich media (images, videos, documents), quick-reply buttons, and message templates are planned but not yet supported
- **24-hour reply window.** WhatsApp only allows businesses to send free-form replies within 24 hours of the customer's last message. If that window has closed, Hay can't deliver a reply until the customer messages you again — this is a WhatsApp rule, not a Hay setting

If a reply can't be delivered because the window expired, the conversation stays in your inbox so you can follow up as soon as the customer writes back.
