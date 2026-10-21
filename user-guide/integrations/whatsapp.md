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
2. Find **WhatsApp** and click **Connect**
3. Complete Meta's signup window: log in with Facebook, pick or create your WhatsApp Business account, and choose the phone number to connect. Numbers already used in the WhatsApp Business app can stay on your phone
4. Back in Hay, confirm the connected number and check that a payment method is set on your WhatsApp Business account (Meta needs one to deliver messages beyond its free tier)

Then assign the channel to an agent: open your agent's settings, and under **Channels** select WhatsApp.

**Self-hosted?** You can also connect with a permanent system-user token from your own Meta app. See the plugin README on GitHub for details.

## What Hay Can Do with WhatsApp

- **Receive messages** — text, images, documents, and voice notes appear as conversations in your Hay inbox, with the customer's WhatsApp profile name and phone number attached to the customer record
- **AI replies** — the agent assigned to the WhatsApp channel answers automatically, and replies are delivered back to the customer's WhatsApp chat
- **Human takeover** — your team can step in on any WhatsApp conversation from the Conversations view, just like any other channel

**24-hour reply window.** WhatsApp only allows businesses to send free-form replies within 24 hours of the customer's last message. If that window has closed, the conversation shows a notice and the reply waits until the customer writes again — this is a WhatsApp rule, not a Hay setting.
