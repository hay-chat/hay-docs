---
layout: docs.njk
title: Wix
description: Connect Wix to Hay to look up orders, check shipments, cancel or refund orders, and answer product questions right in your support conversations.
section: user-guide
---

## What is Wix?

Wix is a website builder with a built-in e-commerce platform, Wix Stores, that many businesses use to sell products online. Connecting your Wix Stores site to Hay lets your AI agent see your real orders, payments, shipments, and product catalog. That means questions like "Where is my order?" or "Can I get a refund?" can be answered — and acted on — directly in the conversation, without anyone switching to the Wix dashboard. For the overview of what Hay.chat does for Wix stores, see [Hay.chat for Wix](https://hay.chat/integrations/wix-ai-chatbot/).

## How to Connect Wix

1. In the Hay dashboard, go to **Integrations** → **Marketplace**.
2. Find **Wix Stores** and click **Install** or **Connect**.
3. Get your Wix API key: in your Wix account, open **Settings → API Keys** and create a new key. Grant it only these permission scopes: Stores - Orders (read & manage), Stores - Order Transactions / Order Billing (for refunds), Stores - Order Fulfillments (read), and Stores - Catalog (read).
4. Paste the key into the **API Key** field in Hay. It's stored encrypted and treated as a secret.
5. Fill in the **Site ID** field — this identifies which Wix site the key belongs to. You can find it in your Wix dashboard URL (the long identifier that appears after `/dashboard/`).
6. Save. Hay verifies the connection by checking that the key can read your store's orders, and tells you exactly what to fix if something is off (for example, a missing permission).

Wix API keys are long-lived, so once connected the integration keeps working until you revoke the key in Wix. For more technical detail — including the exact permission scopes and how the connection works under the hood — see the [plugin README on GitHub](https://github.com/hay-chat/hay-core/tree/master/plugins/core/wix).

## What Hay.chat Can Do with Wix

The integration gives the agent 9 tools. It only sees the orders of the customer it is talking to: there is no store-wide order search, and it does not create or change shipments (that is a warehouse task, not a support task).

### Orders and customers

- Find a customer by email and list their order history
- Look up an order by its id or number, with its payments and fulfilments

### Tracking

- See the fulfilment status and, for each shipment, the carrier, tracking number and link

### Cancellations, address changes and refunds

- **Cancel an order** before it is dispatched, optionally restocking items and emailing the buyer. Cancelling does not refund.
- **Change the shipping address** before dispatch.
- **Refund** in full or in part. Hay.chat checks that the payment can be refunded through Wix and never refunds more than what is left. If the payment provider cannot be refunded through Wix, the agent tells your team to refund in the provider instead of pretending it worked.

Cancellations and refunds wait for a person to approve them by default. See [Action approvals](/docs/user-guide/action-approvals/).

### Products

- Search the catalog by name, and open a product with its variants, pricing and stock

Your AI agent uses these tools automatically during conversations. When a customer asks "Has my order shipped?", the agent looks up the order and its tracking info itself and replies with the answer.
