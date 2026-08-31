---
layout: docs.njk
title: Atlassian (Jira & Confluence)
description: One Atlassian connection that imports Confluence pages into your knowledge base and gives your agent Jira tools.
section: user-guide
---

## What is Atlassian?

Atlassian makes Jira (project and issue tracking) and Confluence (team documentation and wikis). Connecting Atlassian to Hay does two things with a single connection: your Confluence pages can be imported into Hay's knowledge base so your AI agent can answer from your documentation, and your agent gains Jira tools to look up and create issues during conversations.

## How to Connect Atlassian

1. In the Hay dashboard, go to **Integrations** → **Marketplace**.
2. Find **Atlassian** and click **Connect**.
3. Choose your **Auth method**:
   - **API Token (recommended)** — fill in **Atlassian site URL** (e.g. https://your-team.atlassian.net), your **Atlassian email**, and an **API token**. Create the token at id.atlassian.com under Manage profile → Security → API tokens. One token works for both Confluence and Jira, and it's stored encrypted.
   - **OAuth** — sign in through Atlassian instead of pasting a token. Hay requests read access to Confluence content and read/write access to Jira work items in one authorization.
4. Save. The same credentials power both the Confluence import and the Jira tools.
5. To bring your documentation in, add Confluence as a document source in your knowledge base: pick the spaces you want, and Hay imports their pages and keeps them in sync as they change.

For self-hosted or advanced setup details, see the plugin README on GitHub: https://github.com/hay-chat/hay-core/tree/master/plugins/core/atlassian

## What Hay Can Do with Atlassian

**Confluence — knowledge base import:**

- Browse your Confluence spaces and import their pages into Hay's knowledge base, converted into clean, readable documents.
- Keep imported content up to date — Hay detects pages that changed since the last sync and refreshes them.

**Jira — agent tools:**

- **List projects** — discover the Jira projects the connected account can see.
- **Search issues** — find issues with flexible queries (by project, status, assignee, date, and more).
- **Get issue details** — fetch the full details of a specific issue by its key.
- **My open issues** — a shortcut that lists the connected account's unresolved issues, most recently updated first.
- **Create issue** — open a new issue in any project, with type, description, priority, labels, assignee, and due date.

Your agent uses all of this automatically: it answers customer questions from your imported Confluence documentation, and can check on or file Jira issues mid-conversation — for example, creating a bug report when a customer describes a problem.
