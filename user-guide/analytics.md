---
layout: docs.njk
title: Quality - Feedback, Insights and Tests
description: Find out where your AI assistant gets things wrong and fix it. Rate replies, review the patterns Hay finds each night, and replay past conversations before you publish a change.
section: user-guide
---

## The Quality section

**Quality** in the left sidebar groups three pages:

- **Tests** - replay past conversations against your assistant and save test cases. See [Tests](/docs/user-guide/tests/).
- **Insights** - patterns across your conversations and your team's ratings, updated every night. See [Insights](/docs/user-guide/insights/). Not shown to the **Agent** role.
- **Feedback** - the ratings your team gave to AI replies (this page). Not shown to the **Agent** role.

The numbers about volume, resolution and sentiment are on the [Dashboard](/docs/user-guide/dashboard/). There is no separate Analytics page: the old `/analytics` and `/analytics/reports` previews have been removed.

## Rating AI replies

Feedback comes from **your team, not from customers**. When a dashboard user reviews a conversation, each AI message shows thumbs up / thumbs down controls. Clicking one opens a dialog with:

- **Good** / **Bad** buttons
- An optional comment ("What could be improved?")

Once submitted, the message shows "Feedback recorded" with an **Edit** link to change it later.

Ratings are **good** or **bad** (a neutral value exists, but the controls only offer good and bad). There is no star rating. End customers cannot rate individual messages in the chat widget. If you turn on **Customer Satisfaction (CSAT)** under **Settings** → **Channels** → **Webchat**, web chat visitors are asked for a 1-5 emoji rating when a conversation ends; the **Satisfaction** column in Conversations shows it.

## The Feedback page

**Quality** → **Feedback** ("Ratings your team gave to AI replies. Open any rating to see the reply in its conversation.").

**Stat cards:** **Total Feedback**, **Positive**, **Negative** and **Positive Rate**.

**Filters:** **Rating** (All ratings, Good, Bad, Neutral), **Source** (the channel the rated message came from), **Start date** and **End date**.

**Feedback History:** each entry shows the rating, the source channel, any comment, and "Rated by" the reviewer's email. Click **Open conversation** to jump to the rated reply. The list shows 20 entries per page with **Previous** and **Next**.

**Export CSV** downloads `feedback-export-<date>.csv`. The export respects the **Rating**, **Start date** and **End date** filters; the **Source** filter is not applied to it.

## Using feedback to improve

1. Check the **Positive Rate** weekly - is it trending up?
2. Filter by **Bad** and read the comments
3. Look for patterns: a topic the assistant keeps getting wrong usually means a missing or outdated document, or a playbook that needs a step
4. Use **Improve playbook** or **Improve knowledge** from a conversation to get a suggested fix you can review
5. After changing documents or playbooks, watch whether negative feedback on that topic drops

While testing a new setup, rate its messages liberally - the feedback history becomes your quality log.

## Common questions

### Can customers rate my assistant's responses?

Per message, no. Web chat can ask for a satisfaction rating at the end of a conversation if you enable CSAT in the Webchat settings.

### Can I export this data?

Yes: **Export CSV** on the Feedback page, **Export as PDF** / **Export as CSV** on a conversation, and the resolution ledger on the Usage page. PDF exports of the Dashboard and scheduled reports do not exist.

## Next steps

- Review real metrics on your [Dashboard](/docs/user-guide/dashboard/)
- Read the nightly [Insights](/docs/user-guide/insights/)
- Replay past conversations with [Tests](/docs/user-guide/tests/)
- Add [Documents](/docs/user-guide/documents/) for topics with negative feedback
