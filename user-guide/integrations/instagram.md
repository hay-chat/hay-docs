---
layout: docs.njk
title: Instagram
description: Connect your Instagram Professional account so Hay's AI agent can answer your direct messages automatically.
section: user-guide
---

## What is Instagram?

Instagram is one of the most popular places for customers to reach businesses — many people prefer sending a DM over writing an email. Connecting Instagram to Hay routes the direct messages sent to your Instagram Professional account into your Hay inbox, where your AI agent answers them automatically and replies land straight back in the customer's Instagram thread.

## How to Connect Instagram

Before you start, you need an Instagram **Professional account** (Business or Creator) — personal accounts can't use Instagram's messaging tools.

1. In the Hay dashboard, go to **Integrations** → **Marketplace**
2. Find **Instagram** and install it
3. Click **Connect Instagram**
4. You'll be taken to Instagram to sign in. Log in with the Instagram Professional account you want Hay to manage
5. Approve the requested permissions (basic account access and permission to manage messages)
6. You're returned to Hay with the connection saved — there are no tokens or keys to copy and paste, and Hay keeps the connection refreshed automatically

Finally, assign the channel to an agent: open your agent's settings, and under **Channels** select Instagram. Incoming DMs are then routed to that agent for automatic replies.

**Self-hosted?** Instagram uses a shared Hay-managed Meta app, so self-hosted deployments need their own Meta app credentials configured at the platform level. See the [plugin README on GitHub](https://github.com/hay-chat/hay-core/tree/master/plugins/core/instagram) for details.

## What Hay Can Do with Instagram

- **Receive DMs** — new direct messages to your connected account appear as conversations in your Hay inbox, with the sender's Instagram username and name attached to the customer record when available
- **AI replies** — the agent assigned to the Instagram channel answers automatically, and its replies are delivered back into the customer's Instagram thread
- **Human takeover** — your team can step in on any Instagram conversation from the Conversations view, just like any other channel

**Current limitations (worth knowing):**

- **Text only.** Hay handles text messages in both directions. Inbound images, stories, reactions, and other attachments are ignored, and Hay's replies are sent as plain text. Rich content support is planned
- **24-hour reply window.** Instagram only allows businesses to send a free-form reply within 24 hours of the customer's last message. If that window has closed, Hay can't deliver a reply until the customer messages you again — this is an Instagram rule, not a Hay setting

If a customer's message goes unanswered because the window expired, the conversation stays in your inbox so you can follow up as soon as they write back.
