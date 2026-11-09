---
layout: docs.njk
title: Nuvemshop / Tiendanube
description: Connect a Nuvemshop (Brazil) or Tiendanube (Spanish-speaking Latin America) store so your AI agent can track orders, send Pix payment codes, cancel undispatched orders, record refund requests and give capped goodwill coupons.
section: user-guide
---

## What is Nuvemshop?

Nuvemshop (Brazil) and Tiendanube (Spanish-speaking Latin America) are the same e-commerce platform under two names. Connecting your store to Hay.chat lets your AI agent see real orders, payments and shipments, so it can answer "where is my order?" or "how do I pay?" in the conversation.

## How to Connect Nuvemshop

1. Go to **Integrations** → **Marketplace** in the Hay.chat dashboard
2. Find **Nuvemshop** and click **Connect**
3. Click **Connect with Nuvemshop**, sign in to your store admin and approve the Hay.chat app

Tokens do not expire, so there is nothing to refresh. The connection ends if you uninstall the app from your store.

**Using a custom app instead:** if the one-click connection is not available on your installation, create a custom app in your store admin (**Aplicativos a medida** in Portuguese, **Aplicaciones a medida** in Spanish) with read/write permission for orders, fulfillment orders, customers and coupons, and read permission for products. Paste its access token and your numeric store ID into the plugin settings. Hay.chat verifies them when you save.

## What Hay.chat Can Do with Nuvemshop

The integration gives the agent 18 tools. There is no store-wide order browsing, and the agent cannot edit products or customers.

### Orders, customers and tracking

- Find a customer by email or CPF/CNPJ/DNI (not by phone), and list their order history
- Look up an order by its id or the number the shopper sees
- See each shipment: status, tracking code and link, carrier, delivery window and the latest carrier event
- Add a note to an order

### Payments

- Send an unpaid order's Pix code, boleto or payment link, only after the shopper's email or phone matches the order. If the order does not expose a code, the agent says so and asks your team

### Cancellations and address changes

- **Cancel an order** only while it is open and has no tracking code or label, and every shipment is still unpacked, in preparation or packed. Paid orders need an explicit confirmation. Cancelling does not refund.
- **Change the shipping address** under the same rule. Not available for pickup orders.

### Refunds

Nuvemshop does not let apps return money: refunds are made by your payment provider. When a shopper asks for a refund, the agent checks the refund status, and can record a refund request on the order for your team. It never says money was returned, and it hands the conversation to your team.

### Goodwill coupons

The agent can create a single-use apology coupon, one per order, only for the shopper whose email matches the order. In the plugin settings: **Allow goodwill coupons** (on by default), **maximum percentage** (default 10%), **maximum fixed amount** (default 0, which turns fixed coupons off, because stores price in different currencies) and **validity in days** (default 30). A coupon above the limits waits for a person to approve it.

### Products and stock

- Search products, open a product with its variants, list categories and check stock for a SKU

Cancellations and coupons wait for a person to approve them by default. See [Action approvals](/docs/user-guide/action-approvals/).
