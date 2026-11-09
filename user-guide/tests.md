---
layout: docs.njk
title: Tests
description: Replay past conversations against your assistant before it talks to customers, and save test cases that check every playbook draft before you publish it.
section: user-guide
---

## What is Tests?

**Quality** → **Tests** lets you "replay past conversations against your AI agent before it talks to customers". Replies are never sent, actions that change data are simulated, and test runs are never billed. A line at the top shows your allowance, for example "100 of 100 free test replies left this month".

## Replay past conversations

Each shopper message is answered once by the AI, with the real history up to that point. The AI's reply is compared with what your team actually sent.

1. Under **Replay past conversations**, choose **Conversations to replay**: **My most recent closed conversations**, or **Imported conversations**.
2. Set **How many conversations** and **Shopper messages to replay per conversation**. Give it a **Name (optional)**.
3. Click **Start replay**.

### Import conversations

To test on history from another tool, click **Import conversations**, choose a **Format** and click **Import**:

- **Pasted text** (lines like `Customer: ...` and `Agent: ...`)
- **CSV (any tool)**
- **WhatsApp chat export** (enter **Your name in the WhatsApp chat**)
- **Tidio CSV export**
- **Gorgias (recent tickets)** (enter the subdomain, user email and API key; the email and key are used for this import only and are not stored; it runs in the background)

Imported conversations are kept for your organization's retention period and only used for tests.

## Reading the report

Open a run from **Test runs** with **Open report**. Each replayed shopper message lands in one group:

- **Would have answered** - the AI answered on its own
- **Would have handed off** - the AI passed it to your team
- **Would have needed an action** - the AI would have changed something in your store; the call was simulated
- **Answer looks wrong or risky** - fix these first: wrong facts, risky promises, or a claim no action backs
- **Knowledge missing** - the AI lacked store information. Add it to your knowledge or playbook.

Each item shows the **Shopper** message, the **AI's reply**, **What your team sent** and **Why** the judge decided that. A summary compares the AI with your team: same answer, partly the same, different. It is an estimate from a sample of past conversations: it shows what to fix and does not predict an exact rate. Store data may have changed since, so the AI reads today's orders and stock.

From an item you can **Improve playbook**, **Improve knowledge**, **View conversation** or **Save as test case**. **Stop run** cancels a running replay. A run can also stop at its usage limit ("Stopped at the token limit").

## Test cases

A test case is a conversation up to one shopper message, saved with what a good answer does and whether it must hand off. Save one from a test report, an edited or blocked reply, feedback, or an AI reply in a conversation (**Save as test case**). The **Test cases** list shows where each came from, with **Run all test cases** and **Delete**.

**Before you publish a playbook draft**, its test cases run against it and you are warned about any that fail. You can still publish; open the test report to see what changed.

## Testing a playbook draft

In the playbook editor, the **More actions** menu has **Test this draft** (start a test conversation that uses the draft) and **Replay past conversations** (replay history against the draft). Replies use the draft, not the published version. See [Playbooks](/docs/user-guide/playbooks/).

## Soft-launch readiness

The **Soft-launch readiness** card tells you whether it is safe to stop reviewing replies by hand. It reads the review history of replies held for approval (see [Settings](/docs/user-guide/settings/)). It counts replies your team **Approved as written**, **Edited before sending** or **Blocked** while **Require Approval** was on, per week. If your team changed fewer than 1 in 20 replies over the window, it says it may be safe to let the AI send on its own. If approval is off it says so and has nothing to show. Action approvals are separate: see [Action approvals](/docs/user-guide/action-approvals/).

## Related

- [Insights](/docs/user-guide/insights/)
- [Billing and usage](/docs/user-guide/billing-and-usage/) - tests are never billed
