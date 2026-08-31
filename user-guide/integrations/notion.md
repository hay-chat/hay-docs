---
layout: docs.njk
title: Notion
description: Import your Notion pages and databases into Hay's knowledge base so your AI agent can answer questions from your existing docs.
section: user-guide
---

## What is Notion?

Notion is a popular workspace tool where teams write docs, wikis, and knowledge bases, and organize information in pages and databases. Connecting Notion to Hay imports that content into Hay's knowledge base as documents, so your AI agent can use everything your team has already written — help articles, policies, product docs — to answer customer questions. You stay in control of the scope: only the pages and databases you explicitly share with the integration are imported.

## How to Connect Notion

1. In Notion, go to [notion.so/my-integrations](https://www.notion.so/my-integrations) and create an **internal integration**. Copy its token (it starts with `ntn_` or `secret_`).
2. Still in Notion, **share** every page and database you want imported with that integration. Notion only lets integrations see content that has been shared with them — this is how you control exactly what Hay can import.
3. In the Hay dashboard, go to **Integrations** → **Marketplace** and find **Notion**.
4. Click **Connect** and paste the token into the **Internal Integration Token** field. (There is also a **Notion API version** field — leave the default unless Notion tells you otherwise.)
5. Save. Hay verifies the token by reaching Notion with it — if verification fails, double-check the token and make sure at least one page or database is shared with the integration.
6. Create a document source and pick what to import: either **all shared pages** (everything the integration can see) or a **single database** (just that database's entries). The two overlap, so pick the workspace option for everything or one database for a focused subset.

Self-hosted or advanced setups: see the plugin README on GitHub at [https://github.com/hay-chat/hay-core/tree/master/plugins/core/notion](https://github.com/hay-chat/hay-core/tree/master/plugins/core/notion).

## What Hay Can Do with Notion

**Document importing:**

- Imports Notion pages and database entries into Hay's knowledge base as readable documents
- Lets you import everything shared with the integration, or narrow the scope to a single database
- Converts page content (including nested blocks) into clean text your agent can search

**Keeping content in sync:**

- Runs full syncs that sweep everything in the selected scope
- Picks up edits incrementally by checking which pages changed since the last sync
- Removes documents from the knowledge base when the source pages are deleted or trashed in Notion (reconciled during full syncs)

Once imported, your agent uses this content automatically — when a customer asks something covered in your Notion docs, the agent finds the relevant pages and answers from them, no extra setup needed.
