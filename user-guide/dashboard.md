---
layout: docs.njk
title: Dashboard Overview
description: Your Hay dashboard is mission control for your AI support operation. Get a bird's eye view of everything happening with your customer conversations.
section: user-guide
---

## Inbox: what needs you right now

On a phone you land on **Inbox** when you log in; on a desktop you land on the **Dashboard** and the Inbox is one click away in the sidebar. The Inbox is not a report: it shows only the conversations that need a person.

- **Waiting for a human** - conversations the AI handed off, longest wait first. Each row shows the customer, the AI's summary, the channel and how long they have waited. Tap **Take over** to pick one up: the AI stops replying and the conversation opens for you.
- **Being handled** - conversations a team member has already taken over, with the handler's name.
- **Alerts on this device** - turn this on to get a notification on your phone or computer when a conversation starts waiting, even with the dashboard closed. The notification opens the conversation.

Inbox updates live as conversations arrive, are taken over or released. The sidebar badge next to **Inbox** (and the bell in the top bar on phones) shows how many conversations are waiting.

## Dashboard (metrics)

The **Dashboard** entry in the sidebar shows the most important metrics and activity at a glance. You'll see four KPI cards at the top:

- **Active Agents** - how many agents are currently configured and active
- **Total Conversations** - total conversations handled
- **Resolution Rate** - how many conversations were solved without human help
- **Avg Messages per Conversation** - average number of messages exchanged per conversation

Below the KPI cards you'll also find:

- **Activity** - a line chart of conversation volume over time
- **Top Performing Agents** - your agents ranked by conversation volume and resolution rate
- **Recent Conversations** - the latest conversations across your organization
- **Active Conversations (live)** - conversations currently in progress, across Hay and human agents
- **Human escalations** - escalation counts for today and this week
- **Attention Needed** - conversations awaiting a human response
- **Sentiment Score Gauge** - an overall sentiment score for the selected period
- **Sentiment Breakdown** - the split of positive, neutral, and negative messages
- **Document Status Overview** - your knowledge base documents grouped by status

## Understanding Your Metrics

### Active Agents

The number of agents currently configured and active in your workspace.

### Total Conversations

This shows how many customer conversations your Hay agent has handled.

- **Increasing?** Great! Your agent is helping more customers
- **Decreasing?** Check if integrations are working properly

### Resolution Rate

The percentage of conversations solved completely by AI without human intervention.

**What's a good resolution rate?**

- 60-70% is typical when starting out
- 80-90% is excellent after training and optimization
- Below 50%? Your agent may need more training documents

### Avg Messages per Conversation

The average number of messages exchanged across all conversations. Lower values typically indicate faster resolution.

### Human escalations

Displayed as a widget below the top KPI cards. Shows the number of conversations escalated to a human agent, broken out as **Today** and **This week**, with the percentage of conversations that escalated shown as a secondary detail under each count.

**Why do conversations escalate?**

- Customer explicitly asks for a human
- Agent isn't confident in its answer
- Conversation requires actions the AI can't perform
- Sentiment is negative or frustrated

## Quick Actions

From your dashboard, you can quickly:

### View Recent Conversations

Click on any conversation to:

- Read the full thread
- See how your agent responded
- Take over if needed
- Review customer feedback

### Check the Queue

See conversations waiting for human attention. The Queue is accessible at `/queue` but does not appear in the main sidebar navigation.

- Navigate to `/queue` to view pending conversations
- Open any conversation to respond
- Take over to chat directly with the customer

### Monitor Agents

- View which agents are handling the most conversations
- See performance per agent
- Check if any agents need adjustments

> **Note:** The resolution rate shown in the Top Performing Agents widget is currently sample/demo data. Real per-agent analytics are still being developed.

## Real-Time Updates

**Inbox** and the waiting-conversations badge update live over WebSocket. The metrics on the **Dashboard** page are loaded when you open it or change the date range; refresh the page to see the latest numbers.

## Customizing Your View

### Filter by Date Range

Use the date range selector to choose a preset period (e.g., Last 7 days, Last 30 days, This month) or select a custom date range.

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
→ Review recent conversations to see where the agent struggles

**Long response times?**
→ Check if your knowledge base is organized efficiently

## Common Dashboard Questions

### Why do some numbers seem delayed?

Some metrics are calculated every few minutes, not instantly. Refresh your browser to see the latest data.

### What are the possible conversation statuses?

Conversations move through these statuses: `open`, `processing`, `pending-human`, `human-took-over`, `resolved`, and `closed`. There is no "active" status.

### Can I export this data?

Exporting data isn't available from the Dashboard page itself. The Analytics Reports page (`/analytics/reports`) has a report-builder UI, but report generation and export are not implemented yet. The only working export today is **Export CSV** on the Feedback Insights page.

## Next Steps

- Learn about [Conversations](/docs/user-guide/conversations/) to understand the full conversation flow
- Explore [Analytics](/docs/user-guide/analytics/) for deeper insights
- Check out [Conversations](/docs/user-guide/conversations/) to handle escalations efficiently
