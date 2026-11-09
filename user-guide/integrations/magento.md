---
layout: docs.njk
title: Magento
description: Connect Magento to Hay so your AI agent can look up orders and tracking, cancel or refund orders, and check products and stock.
section: user-guide
---

## What is Magento?

Magento (Adobe Commerce) is a powerful open-source e-commerce platform popular with mid-size and enterprise stores that need deep catalog and multi-store flexibility. Connecting Magento to Hay.chat gives your AI agent live access to your store's orders, shipments, products and stock levels, so it can answer "where is my order?" and availability questions, and handle simple order changes, right inside the conversation.

## How to Connect Magento

1. Go to **Integrations** → **Marketplace** in the Hay dashboard
2. Find **Magento** and click **Connect**
3. Enter your **Magento Base URL** — your Magento 2 REST API address, e.g. `https://yourdomain.com/rest/V1`
4. Enter your **API Token**. Create one in your Magento admin under **System** → **Integrations**: add a new integration, grant it access to the Sales, Catalog and Customers resources, activate it, and copy the access token
5. Save — Hay tests the connection against your store and confirms the credentials work

## What Hay.chat Can Do with Magento

The integration gives the agent 13 tools, one for each support task. It does not read sales reports or change your catalog.

### Orders and customers

- Find a customer by email, including guest orders, and list their order history
- Look up an order by its id or order number, with its shipments and invoices
- Add a comment to an order, optionally visible to the customer and emailed to them, or resend the order confirmation email

### Tracking

- See the shipments of an order with their tracking numbers. Tracking that has not moved for longer than your **Stale tracking** thresholds is treated as stuck

### Cancellations, address changes and refunds

- **Cancel an order** only if it is new or awaiting payment and was never invoiced. Paid orders are pointed to a refund instead.
- **Change the shipping address** before any shipment exists.
- **Refund** by creating a credit memo: in full, by item or by amount, never more than what was paid and not yet refunded. The refund goes back through the payment gateway. If that is not possible, the agent hands the conversation to your team rather than recording a refund that moved no money.

Cancellations and refunds wait for a person to approve them by default. See [Action approvals](/docs/user-guide/action-approvals/). Magento has no coupon tool, so the agent cannot create discount codes.

### Products and stock

- Search enabled products by name, open a product by SKU, and check stock for a SKU

Your agent uses these tools automatically: when a shopper asks "is this in stock?" or "where is my order?", the agent queries your Magento store in real time and answers with actual data, escalating to your team whenever a human needs to step in.
