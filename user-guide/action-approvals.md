---
layout: docs.njk
title: Action Approvals
description: Choose which actions the AI may take in your store, such as refunds, cancellations and discounts, and approve or decline the ones that wait for a person.
section: user-guide
---

## Why approvals exist

When your store is connected (Shopify, WooCommerce, Magento, Wix, Nuvemshop, Stripe and other integrations), the AI can do more than answer questions: it can refund an order, cancel it, create a discount code or swap an item. Hay.chat holds the actions that move money until a person says yes.

## What waits for approval by default

Approval is on for new workspaces. By default these actions wait for a person:

- Refunds
- Cancellations
- Discounts and coupons
- Exchanges and item swaps

Everything else runs straight away: reading orders and tracking, address changes, returns, customer detail changes and order notes. You can switch any of these categories to "needs approval" too.

Two things always wait while approval is on, whatever you choose: deletions and setup changes, and actions from an integration that does not say what it does (Hay.chat treats an action it cannot classify as risky).

Reading information (looking up an order, checking stock) never needs approval.

## What the shopper sees

When an action is held, the AI tells the shopper it has requested the action and waits. The conversation gets the **Awaiting approval** status. Once someone approves, the action runs and the AI tells the shopper it is done. If someone declines, they can add a reason and the AI uses it to explain to the shopper.

The AI also asks the shopper "Shall I go ahead?" before an action in your store (**Customer confirmation**, on by default and switchable in Settings). For cancellations this is always on.

## Approving or declining

Pending requests appear in the conversation, in the Inbox, and on the **Action log** page. For each request you can:

- **Approve** it
- **Lower the amount** and approve (you can reduce a refund or discount, never raise it)
- **Decline** it with a reason
- Select several requests of one kind and approve them together

Owners and Admins can approve by default. The Owner can allow Members and Contributors too.

If nobody decides within **48 hours**, the request expires and the conversation is handed to your team. Hay.chat never approves anything on its own.

## Settings

Go to **Settings** → **Assistant** → **Actions**:

- **Ask a person before these actions run**: the master switch, and a checkbox per category. Money categories are marked **Money**.
- **Auto-approve refunds and discounts up to**: refunds and discounts at or below this amount (in the order's currency) run without approval. The default is **0**, so every refund and discount waits. Cancellations always wait while they are switched on.
- **Ask the customer to confirm first**
- **Who can approve**: the Owner always can; choose which other roles can.

Only the Owner can switch approval off, raise the auto-approve amount or change who approves. Switching it off asks you to type **TURN OFF** to confirm, and lists the actions that would then run unseen. Anyone with access can make approval stricter.

After you have approved a run of refunds or discounts, Hay.chat may suggest an auto-approve amount. It never applies it without you.

### Approval for one playbook

A playbook can require approval for extra categories while it is active (for example, always approve refunds in the "Damaged item" playbook). A playbook can only add approval, never remove what Settings requires.

### Goodwill coupons

Integrations that create discount codes (Shopify, WooCommerce, Nuvemshop) have their own limits in the integration settings: a maximum percentage, a maximum fixed amount and a maximum validity. A coupon within the limits needs no approval; a coupon above them waits for a person. See each integration's page.

## Approving AI replies is separate

Action approval is about what the AI does in your store. Reviewing the AI's written replies before they send is a different setting, **Test Mode Default** in [Settings](/docs/user-guide/settings/) (also called Require approval for AI messages). It is off by default.

## Billing

An action sent for approval counts as an AI resolution. See [Billing and Usage](/docs/user-guide/billing-and-usage/).
