---
layout: docs.njk
title: Email
description: Let your AI agent send notification emails to a fixed list of recipients through Hay's built-in email service.
section: user-guide
---

## What is Email?

The Email integration gives your AI agent the ability to send emails — for example, alerting your team when something needs attention. It sends plain-text emails through Hay's own email service to a fixed list of recipients that you configure. It is an internal notification tool: it does not monitor an inbox and it is not a customer-facing channel.

> **Note:** This Email integration only _sends_ notifications to a fixed recipient list — it does not read or reply to customer emails.

## How to Connect Email

1. In the Hay dashboard, go to **Integrations** → **Marketplace**.
2. Find **Email** and click **Install**.
3. In **Email Recipients**, enter a comma-separated list of email addresses that should receive the emails (for example, your team's alert address). Addresses are validated when you save.
4. Save. The integration stays inactive until at least one recipient is configured.
5. Use the built-in health check to confirm the plugin is running and shows your recipient list correctly.

No credentials are needed on your side — emails are sent through the platform's email service using its default sender address.

Self-hosted or advanced setups: see the plugin README on GitHub at [https://github.com/hay-chat/hay-core/tree/master/plugins/core/email](https://github.com/hay-chat/hay-core/tree/master/plugins/core/email).

## What Hay Can Do with Email

**Sending notifications:**

- **Send Email** — sends a plain-text email with a subject and body to all configured recipients at once
- **Health Check** — reports whether the integration is working and which recipients are configured

**Good to know:**

- Emails always go to the full configured recipient list — the agent cannot pick recipients per email
- Plain text only for now; attachments, CC/BCC, and HTML formatting are not yet supported
- Nothing is received — this integration is one-way, outbound only

Your agent uses this automatically: whenever a conversation or playbook calls for notifying your team — an escalation, an alert, a summary — the agent can compose and send the email on its own.
