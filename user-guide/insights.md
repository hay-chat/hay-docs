---
layout: docs.njk
title: Insights
description: Every night Hay looks for patterns in your conversations and your team's ratings, and tells you what the AI gets wrong, what works, and what to change.
section: user-guide
---

## What are Insights?

**Quality** → **Insights** shows "patterns across your conversations and your team's ratings: what the AI gets wrong, what works, and what to change". It is updated every night. An insight is a group of similar conversations, such as "customers asking about X get a wrong answer". Positive patterns appear too.

An insight only appears once the pattern shows up in at least a minimum number of conversations (the **Minimum conversations before an insight appears** setting). When you first use it, Hay reviews your conversations from the last 30 days and shows "Reviewing your conversations from the last 30 days. New insights will appear here after the analysis."

The **Agent** role cannot open Insights.

## The list

At the top, counters show **Open**, **New**, **Positive**, **Addressed** and **Fixed after addressed** (for example "2 of 3", or "Not measured yet").

Filter by **Polarity** (Problem, Working well, Neutral), **State**, **Suggested action**, **Period** and **Minimum conversations**. Each row shows the insight title, how many conversations it covers, when it was last seen, and badges such as **Came back** (the pattern returned after it was addressed), **Fixed**, **Improved** or **Not fixed**.

**Refresh** reloads the list. Admins also have **Settings**:

- **Minimum conversations before an insight appears**
- **Weekly digest email to owners and admins**
- **Advanced: similarity** (join and merge thresholds). The starting values work for most stores, and changes apply from the next nightly analysis.

## An insight in detail

Open an insight to see:

- **Likely cause** and the **Suggested action**: Edit a playbook, New playbook, Fix knowledge, Add knowledge, Setup, Unclear policy, Product bug, Site confusion, Team training, or No action
- Counts for the last 30 days and all time, and how many were handed to the team
- **Evidence**: short excerpts with personal data removed, with links that open the conversation while it is still kept. **What shoppers asked, in other words** shows paraphrases.
- **Recent conversations**
- **History** of every status change

### Working an insight

Use the status buttons: **Acknowledge**, **Start**, **Mark as addressed**, **Reject** (you can give a reason; nothing changes in your AI) and **Archive**. **Reopen** brings one back. An insight with no new conversations for 45 days is suggested for archiving.

To act on it, use the approve button that matches the suggested action:

- **Suggest a playbook edit** or **Create a playbook** - opens a suggested edit for you to review. Saving creates a **draft**; it only reaches new conversations after you publish the playbook. You can then replay the conversations behind the insight against the draft: see [Tests](/docs/user-guide/tests/).
- **Suggest a knowledge fix** - opens a suggested change to your knowledge for you to review before anything is saved. When you save it, the insight is marked as addressed.
- **Create a task** - for changes outside the AI. Copy the text into your task tracker, or keep it here, and mark the insight as addressed when it is done.

### Did the fix work?

After an insight is marked addressed, Hay compares the next days with the 30 days before and shows **Watching**, **Fixed**, **Improved** or **Not fixed yet**, with the weekly rate before and after.

## Related

- [Feedback](/docs/user-guide/analytics/) - your team's ratings feed Insights
- [Tests](/docs/user-guide/tests/)
- [Playbooks](/docs/user-guide/playbooks/)
