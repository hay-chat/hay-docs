---
layout: docs.njk
title: What can't Hay.chat do in Shopify?
description: Hay.chat connects one Shopify store per workspace and has no multi-store support. Refunds and cancellations wait for a person to approve them by default.
section: user-guide
---

> TLDR: Hay.chat connects one Shopify store per workspace and doesn't support multi-store. By default, refunds and cancellations wait until someone on your team approves them.

## One store per workspace

Hay.chat connects one Shopify store per workspace. There is no multi-store support, so a single workspace can't answer for several Shopify stores at once.

## What waits for you

Refunds and cancellations don't go through on the AI's say-so. By default each one waits for a person on your team to approve it. That default covers existing workspaces too, unless a workspace saved its own approval settings. If nobody decides within **48 hours**, the request expires and the chat goes to your team. [Action approvals](/docs/user-guide/action-approvals/) covers where requests appear and how to approve or decline them.

## When a chat goes to a person

Hay.chat hands a chat to your team in six cases:

- The shopper asks for a person
- A tool fails and the request needs manual work
- The AI's confidence is low
- An answer fails its action-claim check
- A playbook reaches a **Handoff to human** step
- An approval expires

After a hand-off, the shopper doesn't see a queue position. See [Team alerts and business hours](/docs/user-guide/team-alerts-and-hours/) for how your team is alerted.

## Related

- [Shopify](/docs/user-guide/integrations/shopify/)
- [Action approvals](/docs/user-guide/action-approvals/)
- [Hay.chat for Shopify stores](https://hay.chat/integrations/shopify/)
- [Human approval for refunds and cancellations](https://hay.chat/human-approval/)
