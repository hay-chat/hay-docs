---
layout: docs.njk
title: Dashboard Overview
description: Your Hay dashboard is mission control for your AI support operation. Get a bird's eye view of everything happening with your customer conversations.
section: user-guide
---

## Where you land

On a phone you land on **Inbox** when you open Hay; on a desktop you land on the **Dashboard**. New workspaces that haven't finished setup land on the **Set up** board first. The Inbox is covered in [Inbox](/docs/user-guide/inbox/): it lists only the conversations that need a person, and the **Metrics and reports** link at the bottom of the Inbox opens this Dashboard.

## Dashboard (metrics)

The **Dashboard** entry in the sidebar shows the most important metrics and activity at a glance. At the top, a **date range** selector (Last 7 days, Last 30 days, Last 90 days, This week, This month, This year, or Custom range) and a **Refresh** button. Four metric cards follow:

- **Total Conversations** - conversations in the period, with how many were resolved
- **Resolution Rate** - the share of conversations that were resolved
- **Avg Messages** - average number of messages per conversation
- **Avg Response Time** - how long the assistant takes to reply, with the median. It reads "No replies in this period" when there is nothing to measure.

Below the cards you'll find:

- **Conversation Activity** - daily conversation volume
- **Recent Conversations** - the latest conversations, each with its status and how long ago (**View All** opens Conversations)
- **Active conversations** - a live list across Hay.chat and humans
- **Human escalations** - takeovers for **Today** and **This week**
- **Attention Needed** - conversations awaiting a human response
- **Customer Sentiment Score** - an overall score for the period, with a **Sentiment Breakdown** of positive, neutral and negative messages
- **Knowledge Base** - your documents grouped by status, with **Add Documents** when there are none

## Understanding Your Metrics

### Total Conversations

How many customer conversations your assistant has handled in the selected period.

- **Increasing?** Great! Your assistant is helping more customers
- **Decreasing?** Check if integrations are working properly

### Resolution Rate

The percentage of conversations that ended resolved.

**What's a good resolution rate?**

- 60-70% is typical when starting out
- 80-90% is excellent after training and optimization
- Below 50%? Your assistant may need more training documents

### Avg Messages

The average number of messages exchanged across all conversations. Lower values typically indicate faster resolution.

### Human escalations

Shows the number of conversations a team member took over, broken out as **Today** and **This week**.

**Why do conversations escalate?**

- Customer explicitly asks for a human
- The assistant isn't confident in its answer
- Conversation requires actions the AI can't perform
- Sentiment is negative or frustrated

## Quick Actions

From your dashboard, you can quickly:

### View Recent Conversations

Click on any conversation to:

- Read the full thread
- See how your assistant responded
- Take over if needed
- Review customer feedback

### Handle what needs a person

Open the [Inbox](/docs/user-guide/inbox/) to see conversations waiting for a human, take one over, or answer the AI's quick questions. The **Attention Needed** card on the Dashboard shows the same waiting conversations.

> **Not the same as `/queue`.** The `/queue` page (not in the sidebar) is the **Job Queue** of background jobs such as document processing, emails and exports. It does not list conversations.

## Real-Time Updates

**Inbox** and the waiting-conversations badge update live over WebSocket. The metrics on the **Dashboard** page are loaded when you open it or change the date range; refresh the page to see the latest numbers.

## Customizing Your View

### Filter by Date Range

Use the date range selector at the top to choose a preset period (Last 7 days, Last 30 days, Last 90 days, This week, This month, This year) or a **Custom range**.

## Dashboard Best Practices

### Check Daily

Spend 2-3 minutes each morning reviewing:

1. Anything waiting on **Inbox**
2. Resolution rate trends
3. Unusual spikes or drops in volume

### Weekly Review

Once per week, look at:

- Which questions are most common (add to your documents!)
- Why conversations are escalating (can you improve training?)
- Response quality (read some full conversations)

### Act on Insights

When you notice patterns:

**High escalation on specific topics?**
→ Add more training documents about those topics

**Low resolution rate?**
→ Review recent conversations to see where the assistant struggles

**Long response times?**
→ Check if your knowledge base is organized efficiently

## Common Dashboard Questions

### Why do some numbers seem delayed?

Some metrics are calculated every few minutes, not instantly. Refresh your browser to see the latest data.

### What are the possible conversation statuses?

Conversations move through these statuses: `open`, `processing`, `awaiting-approval`, `pending-human` (shown as **Needs Attention**), `human-took-over` (shown as **Manual Control**), `resolved`, and `closed`. There is no "active" status. See [Conversations](/docs/user-guide/conversations/#conversation-statuses).

### Can I export this data?

Exporting data isn't available from the Dashboard page itself. You can export a single conversation as PDF or CSV, download the **Resolution ledger** as CSV from **Settings** → **Billing & Usage** → **Usage**, and export team ratings with **Export CSV** on the **Feedback** page. See [Exports, evidence packs and AI reply review](/docs/user-guide/exports-and-evidence/).

## Next Steps

- Learn about [Conversations](/docs/user-guide/conversations/) to understand the full conversation flow
- Explore [Quality: Insights, Feedback and Tests](/docs/user-guide/analytics/) for deeper analysis
- Use the [Inbox](/docs/user-guide/inbox/) to handle escalations efficiently
