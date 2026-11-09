---
layout: docs.njk
title: Analytics & Performance Metrics
description: Track your AI agent's performance with real conversation metrics, sentiment analysis, and message feedback insights.
section: user-guide
---

## Where Analytics Live Today

Hay's analytics are found in three places:

- **The Dashboard** (`/dashboard`) — real conversation metrics, sentiment analysis, and live widgets. See [Dashboard Overview](/docs/user-guide/dashboard/).
- **Insights** (`/insights`) — AI-powered conversation cluster analysis with polarity, state breakdowns, and action types.
- **Feedback** (`/insights/feedback`) — analysis of message feedback given by your team, with CSV export.

The Dashboard, Insights, and Feedback all have sidebar entries (Insights and Feedback are under the **Quality** section).

## Real Metrics (Dashboard)

The Dashboard shows metrics calculated from your actual conversations. All metric cards accept a date range filter and default to the last 30 days.

**Metric Cards**

- **Total Conversations** — count of conversations in the selected period
- **Resolution Rate** — percentage of conversations marked resolved
- **Avg Messages per Conversation** — average number of messages exchanged
- **Avg Response Time** — average and median time from a customer message to the first reply

**Conversation Activity**

A daily count of conversations over time, shown as the Activity chart.

**Sentiment Analysis**

Hay's AI classifies the sentiment of customer messages as they arrive. The Dashboard shows an overall sentiment score plus the breakdown of positive, neutral, and negative messages for the selected period.

**Document Status Overview**

Your knowledge base documents grouped by processing status.

**Additional Widgets**

- **Recent Conversations** — last 4 conversations with status and last message
- **Active Conversations** — live widget showing currently active conversations
- **Escalations** — conversations that have been escalated
- **Attention Needed** — conversations requiring attention

For a walkthrough of each Dashboard widget, see [Dashboard Overview](/docs/user-guide/dashboard/).

## Feedback Insights

**Feedback** (`/insights/feedback`) lets you analyze AI response quality through user feedback.

### How feedback is collected

Feedback comes from **your team, not from customers**. When a dashboard user reviews a conversation, each AI message shows thumbs up / thumbs down controls. Clicking one opens a dialog with:

- **Good** / **Bad** buttons
- An optional **Comment (optional)** field ("What could be improved?")

Once submitted, the message shows "Feedback recorded" with an **Edit** link to change it later.

Ratings are stored as `good` or `bad` (a `neutral` value exists in the schema but the feedback UI only offers good and bad) - there is **no star rating and no 1-5 scale**. End customers cannot rate messages: the webchat widget has no feedback UI. Every rating is recorded with the reviewer who gave it.

### Stats cards

At the top of the page:

- **Total Feedback** - total number of ratings recorded
- **Positive** - count and percentage of `good` ratings
- **Negative** - count and percentage of `bad` ratings
- **Success Rate** - percentage of ratings that are positive

### Filters

Narrow the feedback list by:

- **Rating** - All Ratings, Good, Bad, or Neutral
- **Source** - the channel the rated message came from
- **Start Date** / **End Date**

### Feedback History

Each entry in the list shows the rating badge, the source channel, the rated message's text, any comment left, a shortened message ID, and the reviewer's email. The list is paginated (20 entries per page). Click **Open Conversation** on an entry to navigate to the conversation scrolled to the rated message.

### Export CSV

Click **Export CSV** to download your feedback as `feedback-export-<date>.csv`. The export respects the **Rating**, **Start Date**, and **End Date** filters (the **Source** filter is not applied to the export).

## Using Feedback to Improve

A practical review routine:

1. Check the **Success Rate** weekly - is it trending up?
2. Filter by **Bad** ratings and read the comments
3. Look for patterns: a topic the agent keeps getting wrong usually means a missing or outdated document
4. After changing agent instructions or documents, watch whether negative feedback on that topic drops

While testing a new agent, rate its messages liberally - the feedback history becomes your quality log.

## Insights

**Insights** (`/insights`) provides AI-powered conversation cluster analysis. It groups recent conversations into clusters based on topic similarity, showing:

- **Polarity** — how conversations in each cluster skew (positive, negative, neutral)
- **States** — the resolution outcomes within each cluster
- **Action types** — what actions (tool calls, escalations, etc.) were taken

Use the period filter to change the time window. A backfill indicator shows when cluster data is still being generated. Admins can configure cluster analysis settings from the settings dialog on this page.

## Common Analytics Questions

### Can customers rate my agent's responses?

Not yet. Rating is an internal review tool: dashboard users rate AI messages with thumbs up/down inside the conversation view. The webchat widget shown to customers has no rating UI.

### Where did the "Customer Satisfaction" score come from?

If you saw it on the `/analytics` page, it is demo data. Hay's real quality signal is the **Success Rate** on Feedback Insights, calculated from your team's good/bad ratings.

### Can I export analytics data?

Yes — click **Export CSV** on the Feedback page to download your feedback data. PDF exports and scheduled reports are not yet available.

### What's a "good" resolution rate?

Depends on your industry and complexity, but generally:

- **Simple FAQ-based support:** 80-90%
- **E-commerce support:** 70-85%
- **Technical support:** 60-75%
- **Complex B2B:** 50-70%

Brand new agents need time: expect metrics to improve over 2-4 weeks as you add documents, refine playbooks, and adjust agent instructions.

## Next Steps

- Review real metrics on your [Dashboard](/docs/user-guide/dashboard/)
- Use feedback insights to improve your [AI Assistant](/docs/user-guide/agents/)
- Add [Documents](/docs/user-guide/documents/) for topics with negative feedback
- Monitor your [Conversations](/docs/user-guide/conversations/) for escalation patterns
