---
layout: docs.njk
title: Twenty CRM
description: Connect Twenty CRM to Hay to let your AI agent look up and manage people, companies, notes, tasks, and any custom object in your workspace.
section: user-guide
---

## What is Twenty CRM?

Twenty is a modern, open-source CRM where teams track people, companies, notes, and tasks — and can add their own custom objects and fields. Connecting Twenty to Hay lets your AI agent recognize customers, pull up their company details, log notes and follow-up tasks, and work with any custom object you've defined — all while the conversation is happening.

## How to Connect Twenty CRM

1. In Twenty, go to **Settings** → **APIs & Webhooks** → **Generate API Key** and copy the key
2. In the Hay dashboard, go to **Integrations** → **Marketplace**
3. Find **Twenty CRM** and click **Connect**
4. Fill in **Twenty API URL** — if you use Twenty Cloud, this is `https://api.twenty.com`; if you self-host Twenty, use your own instance address (without any `/rest` at the end)
5. Fill in **API Key** with the key you copied in step 1 (it's stored encrypted)
6. Save — Hay verifies the connection against your workspace right away

The same setup works for both Twenty Cloud and self-hosted Twenty workspaces — only the API URL differs. For technical details, see the plugin page on GitHub: [https://github.com/hay-chat/hay-core/tree/master/plugins/core/twenty](https://github.com/hay-chat/hay-core/tree/master/plugins/core/twenty)

## What Hay Can Do with Twenty CRM

Once connected, your agent gets 26 Twenty tools:

### People

- Find a person by their email address
- Get a person's full details
- Search people by name or other criteria
- Create a new person
- Update a person's information
- List everyone who works at a given company

### Companies

- Find a company by its website domain or by name
- Get a company's full details
- Search or list companies
- Create a new company
- Update a company's information

### Notes & Tasks

- Create a note and attach it to a person or company
- List the notes on a person or company
- Create a task (for example, a follow-up reminder)
- Find and update existing tasks

### Custom Objects & Workspace Schema

- Discover the objects and fields in your workspace — including any custom objects you've added
- Look up the valid options for dropdown-style fields
- List, read, create, update, and delete records of **any** object type — so custom objects work just like the built-in ones

Your agent uses these tools automatically as conversations unfold: when a customer writes in, it can find them by email, check their company, log a note about the conversation, and create a follow-up task — no manual CRM work required.
