---
layout: docs.njk
title: Stripe
description: Connect Stripe to Hay to look up payments, customers, subscriptions, and invoices — and handle refunds — inside your support conversations.
section: user-guide
---

## What is Stripe?

Stripe is one of the most widely used payment platforms, handling payments, subscriptions, invoices, and billing for millions of businesses. Connecting Stripe to Hay gives your AI agent a live view of your billing data, so it can answer "Was I charged twice?", "When does my subscription renew?", or "Can I get a refund?" with real information — and take action when needed.

## How to Connect Stripe

1. In the Hay dashboard, go to **Integrations** → **Marketplace**.
2. Find **Stripe** and click **Install** or **Connect**.
3. Get your Stripe API key: log in to your Stripe Dashboard and go to **Developers → API keys**. For production, create a restricted key with only the permissions you need; for testing, use your test mode secret key. The key starts with `sk_test_` or `sk_live_`.
4. Paste it into the **API Key** field in Hay. It's stored encrypted and treated as a secret.
5. Save. Hay verifies the key by making a test call to your Stripe account and tells you if the credentials aren't valid.

There is also an OAuth-based "Connect with OAuth" path intended for managed and cloud deployments, which requires allowlisting by Stripe. If you're self-hosting and want to explore it, see the [plugin README on GitHub](https://github.com/hay-chat/hay-core/tree/master/plugins/core/stripe) — for most setups, the API key is the recommended route.

## What Hay Can Do with Stripe

Hay connects to Stripe's own tool server, which provides more than two dozen actions across your account:

### Account & balance

- Get your account information and current balance.

### Customers

- Create new customers and list or search existing ones.

### Payments & refunds

- List recent payments, create payment links, and process refunds.

### Subscriptions

- List subscriptions, update them, and cancel them (with prorating options).

### Invoices

- Create invoices, add invoice items, finalize them, and list or filter existing invoices.

### Products, prices & coupons

- Create and list products and prices (one-time or recurring), and create or list discount coupons.

### Disputes

- List disputes by status and update or submit dispute evidence.

### Search

- Search across your Stripe data, fetch a specific record by ID, and search Stripe's documentation.

Your AI agent picks the right tool automatically mid-conversation. When a customer asks "Why was I charged $49 yesterday?", the agent finds the payment, checks the subscription or invoice behind it, and explains — or issues the refund if that's what your playbook allows.
