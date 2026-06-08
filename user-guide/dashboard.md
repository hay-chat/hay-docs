---
layout: docs.njk
title: Dashboard Overview
description: Your Hay dashboard is mission control for your AI support operation. Get a bird's eye view of everything happening with your customer conversations.
section: user-guide
---

## Overview

The dashboard shows you the most important metrics and activity at a glance. When you log in, you'll see four metric cards:

- **Active Agents** - how many AI agents are currently deployed
- **Total Conversations** - how many customer conversations have been handled (with resolved count as subtitle)
- **Resolution Rate** - percentage of conversations solved without human help
- **Avg Messages Per Conversation** - average number of messages exchanged per conversation

## Understanding Your Metrics

### Active Agents

Shows how many AI agents are currently deployed and handling conversations.

- **Multiple agents?** Each agent can be configured for different use cases
- **No active agents?** Check your agent settings to ensure at least one is enabled

### Total Conversations

This shows how many customer conversations your Hay agent has handled, with the number of resolved conversations displayed as a subtitle.

- **Increasing?** Great! Your agent is helping more customers
- **Decreasing?** Check if integrations are working properly

### Resolution Rate

The percentage of conversations solved completely by AI without human intervention.

**What's a good resolution rate?**

- 60-70% is typical when starting out
- 80-90% is excellent after training and optimization
- Below 50%? Your agent may need more training documents

### Avg Messages Per Conversation

The average number of messages exchanged in each conversation. This helps you understand how efficiently issues are being resolved.

- **Low count (1-3)?** Issues are being resolved quickly
- **High count (10+)?** Conversations may need better training or playbooks

## Quick Actions

From your dashboard, you can quickly:

### View Recent Conversations

Click on any conversation to:

- Read the full thread
- See how your agent responded
- Take over if needed
- Review customer feedback

### Check the Queue

See conversations waiting for human attention:

- Click **Queue** to view pending conversations
- Open any conversation to respond
- Take over to chat directly with the customer

### Monitor Agents

- View which agents are handling the most conversations
- See performance per agent
- Check if any agents need adjustments

## Real-Time Updates

> **Planned:** Real-time dashboard updates (live activity indicators and push notifications) are not yet implemented. Metrics are refreshed when you reload the page or change filters.

## Customizing Your View

### Filter by Date Range

Use the date selector to view metrics for:

- Last 7 days
- Last 30 days
- Last 90 days
- This week
- This month
- This year
- Custom range

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

**Low resolution rate?**
→ Review recent conversations to see where the agent struggles

**High avg messages per conversation?**
→ Check if your knowledge base covers the most common questions

**Many conversations pending human?**
→ Add more training documents about those topics

## Common Dashboard Questions

### Why do some numbers seem delayed?

Some metrics are calculated every few minutes, not instantly. Refresh your browser to see the latest data.

### What do the conversation statuses mean?

Conversations use the following statuses:

- **Open** - conversation started, AI is handling it
- **Processing** - AI is generating a response
- **Pending Human** - waiting for a human agent
- **Human Took Over** - a team member is responding
- **Resolved** - issue was solved successfully
- **Closed** - conversation ended, no further action needed

## Next Steps

- Learn about [Conversations](/docs/user-guide/conversations/) to understand the full conversation flow
- Explore [Analytics](/docs/user-guide/analytics/) for deeper insights
- Check out [Conversations](/docs/user-guide/conversations/) to handle escalations efficiently
