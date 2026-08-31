---
layout: docs.njk
title: Wix
description: Connect Wix to Hay to look up orders, check shipments, issue refunds, and answer product questions right in your support conversations.
section: user-guide
---

## What is Wix?

Wix is a website builder with a built-in e-commerce platform, Wix Stores, that many businesses use to sell products online. Connecting your Wix Stores site to Hay lets your AI agent see your real orders, payments, shipments, and product catalog. That means questions like "Where is my order?" or "Can I get a refund?" can be answered — and acted on — directly in the conversation, without anyone switching to the Wix dashboard.

## How to Connect Wix

1. In the Hay dashboard, go to **Integrations** → **Marketplace**.
2. Find **Wix Stores** and click **Install** or **Connect**.
3. Get your Wix API key: in your Wix account, open **Settings → API Keys** and create a new key. Grant it only these permission scopes: Stores - Orders (read & manage), Stores - Order Transactions / Order Billing (for refunds), Stores - Order Fulfillments (manage), and Stores - Catalog (read).
4. Paste the key into the **API Key** field in Hay. It's stored encrypted and treated as a secret.
5. Fill in the **Site ID** field — this identifies which Wix site the key belongs to. You can find it in your Wix dashboard URL (the long identifier that appears after `/dashboard/`).
6. Save. Hay verifies the connection by checking that the key can read your store's orders, and tells you exactly what to fix if something is off (for example, a missing permission).

Wix API keys are long-lived, so once connected the integration keeps working until you revoke the key in Wix. For more technical detail — including the exact permission scopes and how the connection works under the hood — see the [plugin README on GitHub](https://github.com/hay-chat/hay-core/tree/master/plugins/core/wix).

## What Hay Can Do with Wix

### Orders

- **Search orders** — find an order by order number, the customer's email, status, or date.
- **Get order** — pull up the full order: items, totals, payment status, and fulfilment status.
- **Get order transactions** — see every payment and refund on an order.

### Refunds

- **Check refundability** — verify whether a payment can be refunded automatically before promising anything.
- **Refund order** — issue a full or partial refund. If the payment provider can't be refunded automatically, Hay records the refund as handled externally so your order history stays accurate.

### Shipping & fulfilment

- **Get order fulfilments** — see what's shipped and the tracking info.
- **Create fulfilment** — mark items as shipped, with carrier and tracking number.
- **Update fulfilment** — correct or update tracking information.
- **Delete fulfilment** — undo a fulfilment, reverting items to not-shipped.

### Products

- **Search products** — find products in your catalog by name or filter.
- **Get product** — see a product's variants, pricing, and stock levels.

Your AI agent uses these tools automatically during conversations. When a customer asks "Has my order shipped?", the agent looks up the order and its tracking info itself and replies with the answer — no human lookup needed.
