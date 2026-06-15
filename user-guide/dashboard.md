---
layout: docs.njk
title: Dashboard Overview
description: Your Hay dashboard is mission control for your AI support operation. Get a bird's eye view of everything happening with your customer conversations.
section: user-guide
---

## Overview

The dashboard shows you the most important metrics and activity at a glance. When you log in, you'll see:

- **Total conversations** today, this week, and this month
- **Active Agents** - count of enabled agents
- **Resolution rate** - how many conversations were solved without human help
- **Avg Messages Per Conversation** - average number of messages exchanged per conversation

## Understanding Your Metrics

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

### Average Response Time

How quickly customers receive their first response.

**Hay typically responds in:**

- ⚡ 1-3 seconds for simple questions
- ⚡ 3-8 seconds for questions requiring document lookup
- ⚡ 8-15 seconds for complex multi-step queries

### Escalation Rate

The percentage of conversations transferred to human agents.

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

See conversations waiting for human attention:

- Navigate to `/queue` in your browser (the Queue is not currently accessible from the sidebar)
- Open any conversation to respond
- Take over to chat directly with the customer

### Monitor Agents

- View which agents are handling the most conversations
- See performance per agent
- Check if any agents need adjustments

## Real-Time Updates

Your dashboard updates automatically:

- **Notifications** appear for important events:
  - Conversations needing urgent attention
  - Customers waiting too long
  - System alerts or issues

## Customizing Your View

### Filter by Date Range

Use the date selector to view metrics for:

- Last 7 days
- Last 30 days
- Last 90 days
- Custom Range

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

## Next Steps

- Learn about [Conversations](/docs/user-guide/conversations/) to understand the full conversation flow
- Explore [Analytics](/docs/user-guide/analytics/) for deeper insights
- Check out [Conversations](/docs/user-guide/conversations/) to handle escalations efficiently
