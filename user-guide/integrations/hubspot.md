---
layout: docs.njk
title: HubSpot
description: Connect HubSpot to Hay to give your AI agent live access to your CRM — contacts, companies, deals, tickets, and more.
section: user-guide
---

## What is HubSpot?

HubSpot is a popular CRM platform that keeps your contacts, companies, deals, and support tickets in one place. Connecting HubSpot to Hay lets your AI agent look up and update CRM records while chatting with customers — so it can recognize who it's talking to, check the status of a deal or ticket, and log new contacts without anyone leaving the conversation.

## How to Connect HubSpot

1. Go to **Integrations** → **Marketplace** in the Hay dashboard
2. Find **HubSpot** and click **Connect**
3. Click the **HubSpot OAuth** button — you'll be sent to HubSpot to sign in
4. Sign in with a HubSpot account that has access to your CRM and approve the requested permissions (reading and writing contacts, companies, deals, tickets, and related records)
5. You're redirected back to Hay — the connection is confirmed

There are no API keys to copy: HubSpot uses a secure sign-in (OAuth) flow, and Hay handles the rest.

If you run your own Hay installation, an administrator may first need to fill in the **OAuth Client ID** and **OAuth Client Secret** fields with credentials from a HubSpot developer app. See the plugin page on GitHub for details: [https://github.com/hay-chat/hay-core/tree/master/plugins/core/hubspot](https://github.com/hay-chat/hay-core/tree/master/plugins/core/hubspot)

## What Hay Can Do with HubSpot

Once connected, your agent gets 15 HubSpot tools:

### Contacts & Companies

- **Create Contact** — add a new contact to HubSpot, with duplicate detection so the same person isn't added twice
- **Create Company** — add a new company, also with duplicate detection
- **Update Contact** — modify existing contact information
- **Update Company** — modify existing company information
- **Search Contacts** — find contacts using various criteria
- **Search Companies** — find companies using various criteria
- **Get Active Contacts** — list recently modified contacts
- **Get Active Companies** — list recently modified companies

### Activity & Engagement

- **Get Company Activity** — pull a company's full engagement history: emails, calls, meetings, notes, and tasks
- **Get Recent Engagements** — see recent activity across all CRM records

### Sales & Support Records

- **Get Deals** — look up deals in your pipeline
- **Get Tickets** — look up support tickets
- **Get Products** — look up products
- **Get Quotes** — look up quotes
- **Get Invoices** — look up invoices

Your agent picks the right tool on its own during a conversation — for example, when a customer asks about their quote, the agent can search for their contact record and pull up the matching quote without any manual steps.
