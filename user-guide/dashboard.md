---
layout: docs.njk
title: Dashboard Overview
description: Your Hay dashboard is mission control for your AI support operation. Get a bird's eye view of everything happening with your customer conversations.
section: user-guide
---

## Overview

The dashboard shows you the most important metrics and activity at a glance. When you log in, you'll see four KPI cards at the top:

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
- **Sentiment Breakdown** - the split of positive, neutral, and negative conversations
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

Your dashboard updates automatically:

- **Notifications** appear for important events:
  - Conversations needing urgent attention
  - Customers waiting too long
  - System alerts or issues

## Customizing Your View

### Filter by Date Range

Use the custom date picker to select any date range for the displayed metrics.

## Dashboard Best Practices

### Check Daily

Spend 2-3 minutes each morning reviewing:

1. Any conversations in the queue
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

### What's the difference between "open" and "active"?

- **Open** = conversation started but may be waiting for a response
- **Active** = actively exchanging messages right now

### Can I export this data?

Exporting data isn't available from the Dashboard page itself. You can export data from **Analytics > Reports** using the **CSV Export** option.

## Next Steps

- Learn about [Conversations](/docs/user-guide/conversations/) to understand the full conversation flow
- Explore [Analytics](/docs/user-guide/analytics/) for deeper insights
- Check out [Conversations](/docs/user-guide/conversations/) to handle escalations efficiently
