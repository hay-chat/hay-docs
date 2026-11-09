---
layout: docs.njk
title: WooCommerce
description: Connect WooCommerce to Hay so your AI agent can look up orders, check products and stock, track shipments, cancel or refund orders, and give capped goodwill coupons.
section: user-guide
---

## What is WooCommerce?

WooCommerce is the most widely used e-commerce plugin for WordPress, powering online stores of every size on top of a regular WordPress site. Connecting WooCommerce to Hay.chat gives your AI agent live access to your store's orders, tracking, products and customers, so it can answer order-status questions, check availability, and handle support tasks directly in customer conversations.

## How to Connect WooCommerce

1. Go to **Integrations** → **Marketplace** in the Hay dashboard
2. Find **WooCommerce** and click **Connect**
3. Enter your **WordPress Site URL** — your WordPress/WooCommerce site address, e.g. `https://mystore.com`
4. Enter your **Consumer Key** and **Consumer Secret**. Generate these in your store's admin under **WooCommerce** → **Settings** → **Advanced** → **REST API** → **Add key** (give the key Read/Write permissions so the agent can cancel and refund orders when you allow it to)
5. Save. Hay.chat tests the connection against your store and confirms it's working
6. Optionally set the goodwill coupon limits, the stale-tracking thresholds and a "How to return" text the agent uses when a shopper asks how to send something back

## What Hay.chat Can Do with WooCommerce

The integration gives the agent 20 tools, one for each support task. It does not expose your whole store: there is no store-wide order browsing, no store settings, and no product or customer administration.

### Orders and customers

- Find a customer by email or phone, including guest orders, and list their order history
- Look up an order by its id or order number
- Update a customer's details or saved address
- Add an internal note to an order, or resend the order confirmation email

### Tracking

The agent reads tracking from the places stores actually keep it: the Shipment Tracking plugin, Advanced Shipment Tracking (AST), VillaTheme, YITH, plain tracking meta fields, and order notes (including Correios tracking codes). Tracking that has not moved for longer than your **Stale tracking** thresholds (default 5 days domestic, 10 days international) is treated as stuck, so the agent can offer to help instead of repeating the carrier status.

### Cancellations, address changes and refunds

- **Cancel an order** only while it is pending, on hold or processing and has no tracking recorded. Cancelling does not refund.
- **Change the shipping address** under the same rule.
- **Refund** in full, by item or by amount, never more than what is left to refund. The agent can preview the amount first. The refund is requested from the payment gateway; if the gateway cannot refund through WooCommerce the agent hands the conversation to your team. A "record only" refund is allowed only for offline methods (bank transfer, cash on delivery, cheque).
- Send the Pix code again for an unpaid Pix order

Refunds, cancellations and discount coupons wait for a person to approve them by default. See [Action approvals](/docs/user-guide/action-approvals/).

### Goodwill coupons

The agent can create a single-use coupon as an apology. You set the limits in the integration settings: **Allow goodwill coupons** (on by default), **maximum percentage** (default 10%, 0 turns percentage coupons off), **maximum fixed amount** (default 0, which turns fixed coupons off) and **validity in days** (default 30). A coupon within the limits is created without approval. A coupon above them waits for a person.

### Products and stock

- Search products, open a product with its variations, or get one by its URL slug
- Check stock for a product or a specific variation SKU

Your agent uses these tools automatically in conversations: when a customer asks "where is my order?" or "is this still in stock?", the agent queries your store in real time and answers with actual order and product data.
