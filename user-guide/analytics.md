---
layout: docs.njk
title: Analytics & Performance Metrics
description: Track your AI agent's performance with real conversation metrics, sentiment analysis, and message feedback insights.
section: user-guide
---

## Where Analytics Live Today

Hay's working analytics are found in two places:

- **The Dashboard** (`/dashboard`) - real conversation metrics, sentiment analysis, and live widgets. See [Dashboard Overview](/docs/user-guide/dashboard/).
- **Feedback Insights** (`/insights/feedback`) - real analysis of message feedback given by your team, with CSV export.

> **Important:** The pages at `/analytics`, `/analytics/reports`, and `/insights` also exist, but they are **UI previews populated with demo data** - the numbers shown there are not calculated from your account. They are covered at the end of this guide so you can tell them apart from real data.

Only the Dashboard has a sidebar entry. There is no Analytics or Insights entry in the sidebar - Feedback Insights and the preview pages are reached by direct URL.

## Real Metrics (Dashboard)

The Dashboard shows metrics calculated from your actual conversations. All of them accept a date range filter and default to the last 30 days:

**Conversation Metrics**

- **Total Conversations** - count of conversations in the selected period
- **Resolution Rate** - percentage of conversations marked resolved
- **Avg Messages per Conversation** - average number of messages exchanged

**Conversation Activity**

A daily count of conversations over time, shown as the Activity chart.

**Sentiment Analysis**

Hay's AI classifies the sentiment of customer messages as they arrive. The Dashboard shows an overall sentiment score plus the breakdown of positive, neutral, and negative messages for the selected period.

**Document Status Overview**

Your knowledge base documents grouped by processing status.

> **Not yet implemented:** Response time analytics exist as an API endpoint but currently always return zero - the calculation is a placeholder. Any "average response time" figure you see in the product is not real data yet.

For a walkthrough of each Dashboard widget, see [Dashboard Overview](/docs/user-guide/dashboard/).

## Feedback Insights

**Feedback Insights** (`/insights/feedback`) is the one fully functional analytics page beyond the Dashboard. Its purpose: "Analyze AI response quality through user feedback."

### How feedback is collected

Feedback comes from **your team, not from customers**. When a dashboard user reviews a conversation, each AI message shows thumbs up / thumbs down controls. Clicking one opens a dialog with:

- **Good** / **Bad** buttons
- An optional **Comment (optional)** field ("What could be improved?")

Once submitted, the message shows "Feedback recorded" with an **Edit** link to change it later.

Ratings are stored as `good`, `bad`, or `neutral` - there is **no star rating and no 1-5 scale**. End customers cannot rate messages: the webchat widget has no feedback UI. Every rating is recorded with the reviewer who gave it.

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

Each entry in the list shows the rating badge, the source channel, any comment left, a shortened message ID, and the reviewer's email. The list is paginated (20 entries per page). The rated message's text itself is not shown, and the link icon next to each entry does not navigate anywhere yet.

### Export CSV

Click **Export CSV** to download your feedback as `feedback-export-<date>.csv`. The export respects the **Rating**, **Start Date**, and **End Date** filters (the **Source** filter is not applied to the export). This is currently the only working data export in Hay's analytics.

## Using Feedback to Improve

A practical review routine:

1. Check the **Success Rate** weekly - is it trending up?
2. Filter by **Bad** ratings and read the comments
3. Look for patterns: a topic the agent keeps getting wrong usually means a missing or outdated document
4. After changing agent instructions or documents, watch whether negative feedback on that topic drops

While testing a new agent, rate its messages liberally - the feedback history becomes your quality log.

## Demo / Preview Pages

The following pages exist in the product but display **hardcoded demo data**. Do not use them to make decisions.

### `/analytics` - Analytics

Shows KPI cards (Total Conversations, Resolution Rate, Avg Response Time, Customer Satisfaction), Response Time Distribution, Top Issues, an Agent Performance table, and Channel Performance. All of these figures are demo placeholders, including the star-based satisfaction scores. The timeframe selector, **Export**, and **Refresh** buttons do nothing yet. The two chart areas ("Conversation Volume", "Resolution Rate Trend") are empty placeholders. The only real element is the list of channels itself, which reflects your enabled channel plugins - the metrics next to them do not.

### `/analytics/reports` - Custom Reports

A preview of a report builder (Report Configuration, Metrics Selection, Grouping & Filters, Visualization, Quick Templates, Recent Reports). Generating, saving, scheduling, viewing, and downloading reports are all not implemented - the buttons do nothing yet, and the "Recent Reports" list is demo data.

### `/insights` - Insights

Shows sample AI-generated improvement suggestions. Accepting or rejecting them only updates the demo list on screen - nothing is saved - and the page itself displays a "Coming soon" notice.

## Common Analytics Questions

### Can customers rate my agent's responses?

Not yet. Rating is an internal review tool: dashboard users rate AI messages with thumbs up/down inside the conversation view. The webchat widget shown to customers has no rating UI.

### Where did the "Customer Satisfaction" score come from?

If you saw it on the `/analytics` page, it is demo data. Hay's real quality signal is the **Success Rate** on Feedback Insights, calculated from your team's good/bad ratings.

### Can I export analytics data?

Yes, one export works today: **Export CSV** on the Feedback Insights page. PDF exports and scheduled reports are not implemented.

### What's a "good" resolution rate?

Depends on your industry and complexity, but generally:

- **Simple FAQ-based support:** 80-90%
- **E-commerce support:** 70-85%
- **Technical support:** 60-75%
- **Complex B2B:** 50-70%

Brand new agents need time: expect metrics to improve over 2-4 weeks as you add documents, refine playbooks, and adjust agent instructions.

## Next Steps

- Review real metrics on your [Dashboard](/docs/user-guide/dashboard/)
- Use feedback insights to improve your [Agents](/docs/user-guide/agents/)
- Add [Documents](/docs/user-guide/documents/) for topics with negative feedback
- Monitor your [Conversations](/docs/user-guide/conversations/) for escalation patterns
