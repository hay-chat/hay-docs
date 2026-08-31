---
layout: docs.njk
title: Chatwoot
description: Use Chatwoot as a communication channel — Hay's AI answers your Chatwoot conversations and hands off to your human agents when needed.
section: user-guide
---

## What is Chatwoot?

Chatwoot is an open-source customer support platform where teams handle conversations from live chat, email, and social channels in shared inboxes. Connecting it to Hay turns Chatwoot into a communication channel: messages arriving in your Chatwoot inboxes are answered automatically by Hay's AI, and whenever the AI decides a human should take over, the conversation is released back to your Chatwoot agents.

## How to Connect Chatwoot

The connection works through a Chatwoot **Agent Bot** — a bot user you create in Chatwoot and point at Hay.

1. In your Chatwoot instance, create an Agent Bot and set its outgoing URL to Hay's webhook endpoint for this plugin (the Setup Guide inside the plugin settings shows the exact URL for your workspace). Note the bot's access token and webhook secret — you'll need both.
2. In the Hay dashboard, go to **Integrations** → **Marketplace**.
3. Find **Chatwoot** and click **Connect**.
4. Fill in the configuration fields:
   - **Chatwoot Base URL** — your Chatwoot instance URL, e.g. https://app.chatwoot.com for cloud, or your self-hosted URL.
   - **Account ID** — your Chatwoot account ID (the number in your Chatwoot dashboard URL).
   - **Agent Bot Access Token** — the access token returned when you created the Agent Bot. Stored encrypted.
   - **Webhook Secret** — the Webhook Secret shown on the Agent Bot's Edit page in Chatwoot. Hay uses it to verify that every incoming webhook really comes from your Chatwoot.
   - **Escalation Team ID (optional)** — when the AI hands off to a human, Hay can assign the Chatwoot conversation to this team.
5. Save — Hay validates the credentials against your Chatwoot account.
6. Back in Chatwoot, assign the Agent Bot to the inboxes you want Hay to handle. In Hay, make sure an agent is set up to respond on the channel, just like any other channel.

Self-hosted Chatwoot works the same way — just use your own instance URL as the Base URL. For advanced details, see the plugin README on GitHub: https://github.com/hay-chat/hay-core/tree/master/plugins/core/chatwoot

## What Hay Can Do with Chatwoot

Chatwoot is a **channel plugin** — it doesn't add tools to your agent; it carries conversations:

- **Receive messages** — new messages in your bot-assigned Chatwoot inboxes flow into Hay as conversations, with the customer recognized and tracked in Hay.
- **Reply automatically** — Hay's AI generates responses and sends them back into the Chatwoot conversation through the bot.
- **Escalate to humans** — when the AI hands off, the Chatwoot conversation is released from the bot back into your normal human-agent queue, and optionally assigned to your chosen escalation team.
- **Verified delivery** — every incoming message is signature-checked with your webhook secret, so only your Chatwoot instance can talk to Hay.

Once connected, this all happens automatically: your AI agent answers Chatwoot conversations around the clock and quietly steps aside whenever a human should take over.
