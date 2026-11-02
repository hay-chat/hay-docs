---
layout: docs.njk
title: Messenger
description: Connect your Facebook Pages so Hay.chat's AI agent can answer Messenger conversations, with replies going back to the customer's Messenger thread.
section: user-guide
---

## What is the Messenger channel?

Customers who find you on Facebook often message your Page. Connecting Messenger routes those messages into your Hay.chat inbox, where your AI agent answers them and replies land back in the customer's Messenger thread.

## How to Connect Messenger

You need to manage the Facebook Pages you connect.

1. In the Hay.chat dashboard, go to **Integrations** → **Marketplace**
2. Find **Messenger** and install it
3. Click **Connect** and sign in with Facebook
4. Choose the Pages you want Hay.chat to answer for and approve the permissions it asks for

There are no tokens to copy and paste. To add another Page later, connect again and select it.

Then assign an agent: go to **Settings** → **Channels** and select an agent for the Messenger channel.

**Self-hosted?** Messenger uses a Hay.chat-managed Meta app. A self-hosted installation needs its own Meta app credentials configured on the server (`META_MESSENGER_APP_ID`, `META_MESSENGER_APP_SECRET`, `META_VERIFY_TOKEN`).

## What Hay.chat Can Do with Messenger

- **Receive messages**: new Messenger conversations appear in your inbox. Photos, videos, voice messages and files are kept, and voice messages are transcribed so the AI can answer them. Stickers, locations and shared links show as a short note
- **AI replies**: the agent assigned to the channel answers automatically. The AI and your team can send photos, videos, audio and PDFs back
- **Human takeover**: replies you send from your Page inbox or Meta Business Suite also appear in Hay.chat, and the AI stops answering that conversation

## The 24-hour window

Messenger allows a free-form reply within 24 hours of the customer's last message. The AI only replies inside that window. A reply written by a person on your team is still sent for up to 7 days. After that, wait for the customer to write again; this is a Meta rule, not a Hay.chat setting.
