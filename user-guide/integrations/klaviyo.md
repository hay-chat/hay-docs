---
layout: docs.njk
title: Klaviyo
description: Connect Klaviyo to Hay to look up customer profiles, manage marketing subscriptions, and work with lists, segments, campaigns, and flows from your support conversations.
section: user-guide
---

## What is Klaviyo?

Klaviyo is a marketing automation platform used for email and SMS marketing — customer profiles, lists and segments, campaigns, and automated flows. Connecting Klaviyo to Hay lets your AI agent see a customer's marketing profile and act on it during a conversation: unsubscribing someone who asks to stop receiving emails, checking which lists they're on, or looking up how a campaign performed.

## How to Connect Klaviyo

1. In the Hay dashboard, go to **Integrations** → **Marketplace**.
2. Find **Klaviyo** and click **Install** or **Connect**.
3. Get your Klaviyo private API key: in Klaviyo, go to **Account → Settings → API Keys** and create a private key (it starts with `pk_`).
4. Paste it into the **Private API Key** field in Hay. It's stored encrypted and treated as a secret.
5. Save. Hay verifies the key against your Klaviyo account and tells you if the credentials aren't valid.

For more technical detail on how the integration works, see the [plugin README on GitHub](https://github.com/hay-chat/hay-core/tree/master/plugins/core/klaviyo).

## What Hay Can Do with Klaviyo

### Customer profiles

- Look up profiles (individually or by search), create new profiles, and update existing ones.

### Marketing subscriptions

- Subscribe a profile to email or SMS marketing.
- Unsubscribe a profile from marketing — handy when a customer asks to stop receiving messages.

### Lists & segments

- Browse your lists and open a specific list.
- Browse your segments and open a specific segment.

### Campaigns

- List campaigns (email, SMS, or mobile push), open a specific campaign, and create new campaigns.
- Assign an email template to a campaign message.
- Pull a campaign's performance report.

### Flows

- List your automated flows, open a specific flow, and pull a flow's performance report.

### Events & metrics

- View events (things customers did, like placing an order) and record new events.
- Browse your metrics and open a specific metric.

### Templates, images & catalog

- Create and retrieve email templates.
- Upload images to Klaviyo from a file or a web address.
- Browse your Klaviyo catalog items.
- View your account details.

Your AI agent uses these tools automatically during conversations. When a customer writes "Please stop emailing me", the agent finds their Klaviyo profile and unsubscribes them on the spot — and confirms it in the same reply.
