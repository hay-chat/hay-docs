---
layout: docs.njk
title: Billing and Usage
description: What Hay.chat counts as a billable resolution, what is never billed, how conversations close on their own, and how to review or exclude resolutions on the Usage page.
section: user-guide
---

> TLDR: Hay.chat charges a monthly plan that includes a set number of resolutions, with extra resolutions billed per resolution on top. For new accounts, spend is capped at 150% of the plan: at the cap the AI pauses, new chats go to your team and anything past the cap is waived.

## What does Hay.chat bill for?

Hay.chat bills per **resolution**: a conversation the AI actually resolved. You are not billed per message, per seat of the AI, or for conversations that went nowhere.

When a conversation closes, Hay.chat gives it one outcome. These outcomes are billed:

- **Confirmed**: the AI resolved it and the shopper said so ("thanks, that solved it").
- **Assumed**: the AI resolved it and the shopper simply stopped writing.
- **Planned hand-off**: a playbook's **Handoff to human** step sent the chat to your team on purpose. The AI did its job by following your playbook, so it counts.
- **An action sent for approval**: the AI asked for a refund, cancellation or similar action to be approved (see [Action approvals](/docs/user-guide/action-approvals/)). The AI did the work when it asked, whatever your team decides afterwards.

If the AI resolved the conversation and a person was involved afterwards, it is still billed.

## What is never billed?

- Greeting-only chats ("hi", then nothing) and empty conversations
- Chats where the shopper never asked for anything real, or the AI never gave a real answer
- Spam, including spam Hay.chat held before the AI answered
- Chats where the shopper closed the conversation unhappy ("this didn't help")
- Conversations handed to your team without the AI resolving them
- Chats where a teammate supplied the answer to a quick check (see [Business hours](/docs/user-guide/team-alerts-and-hours/#quick-check-with-the-team))
- Tests: the playground, test mode and replays of past conversations

### If the shopper comes back

An assumed resolution can be reversed. If the shopper writes again within **72 hours**, the charge is reversed and the conversation counts as one conversation again. Reversed resolutions show on the Usage page.

## When does a quiet conversation close?

When a shopper goes quiet, Hay.chat closes the conversation for you. With the default timing:

| After the shopper's last message | What happens |
| --- | --- |
| 15 minutes | The AI sends a short "are you still there?" message |
| 30 minutes | The conversation closes with a sign-off message |
| 60 minutes | If there was no warning and no reply, it closes silently, with no message |

The close is when the outcome (and the billing decision) is recorded. Self-hosted installations can change the timing with the `CONVERSATION_INACTIVITY_INTERVAL` setting (the 30-minute close is the default value; the warning is half of it and the silent close is double).

## Where can I see my usage?

Go to **Settings** → **Billing & Usage** → **Usage** (Admins and Owners). It shows:

- **Billable resolutions** for the current billing period, using the same dates as your invoice. Your plan's allowance and any extra-resolution charges are on your [billing page](https://hay.chat/billing/).
- **Breakdown**: how many were billed, how many were reversed because the shopper came back within 72 hours, how many your team excluded, and the conversations that were **not billed** and why (handed off, shopper left unhappy, no real question, tests, spam).
- **Resolution ledger**: every billed resolution in the period with the reason it counted ("Shopper confirmed it was solved" or "AI answered, shopper did not come back"), a link to the conversation, and an **Export CSV** button.

If your plan's resolutions are used up and extra resolutions are switched off, the page tells you the AI is paused and conversations wait for your team.

### Excluding a resolution

If a resolution should not have counted (for example it was your own test, or the shopper really needed a person), open it in the ledger and click **Exclude**. Give a reason.

- Each billing period you can exclude up to **5% of your resolutions** yourself. These leave the count right away.
- After that, an exclusion request goes to Hay.chat for review. The resolution stays counted until the review is done, and the ledger shows whether it was excluded or declined.
- Resolutions from a period that has already closed can no longer be excluded here.

## Is there a spending cap?

Yes, for new accounts. Overage is on by default, so once your plan's resolutions are used up, extra resolutions are billed on top. Spend is capped at **150%** of your plan. At the cap the AI pauses, new chats go to your team, and anything billed past the cap is waived.

You get an email at **50%**, **80%** and **100%** of your plan, then another for every further **10%** of overage. A banner appears on the dashboard at **80%**. Accounts billed through Shopify don't get these alert emails yet.

A busy month doesn't step your plan back down by itself.

## Is there a free trial?

Yes. The trial lasts **30 days** and needs no credit card. The clock starts when you sign up, not when you go live, so it's worth getting your docs and playbooks ready early.

When the trial ends without a plan, you get **7 days** with the AI paused. The dashboard stays open during that time.

## What happens when I change or cancel my plan?

An upgrade applies straight away. A downgrade takes effect from your next billing date.

You can cancel any time from your [billing page](https://hay.chat/billing/). Your plan keeps running to the end of the period you've paid for.

You can pay in EUR, USD, GBP or BRL.

## Are there setup or per-seat fees?

No. There are no per-seat fees and no setup fees. Plan prices and the resolutions each plan includes are on [hay.chat/pricing](https://hay.chat/pricing/).

## How many resolutions will I need?

A resolution is a conversation the AI resolved, confirmed or assumed, or an action sent for approval, whatever your team decides about it. Greeting-only chats, empty chats, spam, shoppers who left unhappy and test chats never count. Compare that with your own conversation volume, then pick a plan on [hay.chat/pricing](https://hay.chat/pricing/).

## Related

- [Settings](/docs/user-guide/settings/#why-is-there-a-usage-banner-at-the-top-of-the-dashboard) explains the usage banner
- [FAQ](/docs/user-guide/faq/) covers plans, currencies and what happens when you exceed your limit
