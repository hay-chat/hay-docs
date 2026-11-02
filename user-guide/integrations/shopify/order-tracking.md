---
layout: docs.njk
title: Can Hay.chat track a Shopify order for a shopper?
description: Hay.chat looks up a Shopify order in your store, reads its tracking and tells the shopper where it is, using real order data instead of a canned reply.
section: user-guide
---

> TLDR: Yes. Hay.chat looks up the order in your Shopify store, reads its tracking and tells the shopper where it is. If the shopper asks for a person, the chat goes to your team.

## What Hay.chat does with the order

When a shopper asks where their order is, Hay.chat looks the order up in your connected Shopify store and reads its tracking. The answer comes from the order itself, so the shopper hears what Shopify actually shows for it. These are the questions support teams call [WISMO](https://hay.chat/glossary/wismo/), short for "where is my order?".

## How a shopper asks

Shoppers ask in their own words. A typical exchange looks like this:

> **Shopper:** Hi, where's my order? It should have arrived by now.
>
> **The AI:** looks up the order, reads its tracking and tells the shopper where the parcel is.

## Does this need approval?

No. Looking up an order and reading its tracking doesn't move money, so nothing waits for your team. The actions that wait for a person by default are refunds, cancellations and discounts. See [Action approvals](/docs/user-guide/action-approvals/) for the full picture.

## When the chat goes to your team

Hay.chat hands the chat to a person when the shopper asks for one, a tool fails and needs manual work, the AI's confidence is low, an answer fails its action-claim check, a playbook reaches a **Handoff to human** step, or an approval expires. After the hand-off the shopper doesn't see a queue position. For alerts and opening hours, see [Team alerts and business hours](/docs/user-guide/team-alerts-and-hours/).

## Limits

Hay.chat connects one Shopify store per workspace.

## Related

- [Shopify](/docs/user-guide/integrations/shopify/)
- [Action approvals](/docs/user-guide/action-approvals/)
- [Hay.chat for Shopify stores](https://hay.chat/integrations/shopify/)
- [Human approval for refunds and cancellations](https://hay.chat/human-approval/)
