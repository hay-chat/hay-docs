---
layout: docs.njk
title: Zendesk
description: Connect your Zendesk account so your AI agent can manage tickets, customers, and support workflows for you.
section: user-guide
---

## What is Zendesk?

Zendesk is a popular customer service platform where support teams manage tickets, customers, and help center content. Connecting Zendesk to Hay lets your AI agent work directly with your help desk — looking up and creating tickets, finding customer records, and searching your Zendesk data as part of a conversation, without anyone switching tools.

## How to Connect Zendesk

1. In the Hay dashboard, go to **Integrations** → **Marketplace**.
2. Find **Zendesk** and click **Connect**.
3. Fill in the configuration fields:
   - **Zendesk Subdomain** — your Zendesk subdomain, e.g. "mycompany" from mycompany.zendesk.com.
   - **Admin Email** — the email address of the Zendesk admin user the connection will act as.
   - **API Token** — a Zendesk API token. Generate one in Zendesk under **Admin Center** → **APIs** → **API Tokens**. The token is stored encrypted.
4. Save. Hay tests the connection against your Zendesk account right away and tells you if the subdomain, email, or token is wrong.

A **Setup Guide** page is also available inside the plugin's settings if you need a walkthrough. For self-hosted or advanced setups, see the plugin README on GitHub: https://github.com/hay-chat/hay-core/tree/master/plugins/core/zendesk

## What Hay Can Do with Zendesk

Once connected, your AI agent gets a full set of Zendesk actions:

- **Tickets** — list, look up, create, update, and delete tickets.
- **Users** — list, look up, create, update, and delete users (customers and agents).
- **Organizations** — list, look up, create, update, and delete organizations.
- **Agent groups** — list, look up, create, update, and delete groups.
- **Macros** — list, look up, create, update, and delete macros.
- **Views** — list, look up, create, update, and delete views.
- **Triggers** — list, look up, create, update, and delete triggers.
- **Automations** — list, look up, create, update, and delete automations.
- **Help Center articles** — list, look up, create, update, and delete articles.
- **Search & stats** — search across your Zendesk data, get Zendesk Talk statistics, and list Zendesk Chat conversations.

Your agent picks the right action automatically based on the conversation — for example, when a customer asks about an existing request, the agent can find the ticket and report its status, or open a new ticket when something needs follow-up. You can enable or disable individual tools in the plugin settings.
