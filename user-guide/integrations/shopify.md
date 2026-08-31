---
layout: docs.njk
title: Shopify
description: Connect Shopify to Hay so your AI agent can look up orders, track shipments, handle refunds and returns, and recommend real products from your catalog.
section: user-guide
---

## What is Shopify?

Shopify is one of the world's most popular e-commerce platforms, used by millions of merchants to run their online stores. Connecting Shopify to Hay gives your AI agent direct access to your store's orders, customers, products, and inventory — so it can answer "where is my order?" questions, process cancellations, refunds, and returns, and recommend products from your real catalog, all without a human agent touching the keyboard.

## How to Connect Shopify

1. Go to **Integrations** → **Marketplace** in the Hay dashboard
2. Find **Shopify** and click **Install**
3. Leave **Connection mode** set to **Managed (recommended)**
4. Enter your **Store domain** — your myshopify.com address, e.g. `mystore.myshopify.com` (without https://). You can find it in your Shopify admin under **Settings** → **Domains**
5. Save, then click **Connect with Shopify**
6. Approve the requested permissions on the Shopify screen that opens
7. Done — Hay confirms the connection and your agent can start using the store

The plugin also has a **Self-hosted (own app)** connection mode for teams that run their own Hay installation and their own Shopify app: instead of the one-click connect, you enter your app's **Client ID** and **Client secret**, and Hay refreshes the short-lived access token for you automatically every 20 hours. See the [Shopify plugin README](https://github.com/hay-chat/hay-core/tree/master/plugins/core/shopify) for the full self-hosted setup.

## What Hay Can Do with Shopify

### Orders

- Look up an order by its order number (e.g. #1001)
- Get an order's items, totals, shipping address, tracking, and support eligibility
- List a customer's orders, most recent first
- Get an order's fulfillment and shipment tracking status
- Change where an undispatched order will ship (refused once fulfillment has started)
- Add or replace the internal note on an order
- Cancel an order — optionally restocking items, refunding, and notifying the customer

### Refunds & Returns

- Preview the correct refund amounts for an order before executing it
- Execute a refund, bounded by Shopify's maximum refundable amount
- List the fulfilled items on an order that are eligible for a return
- Open a return for eligible items

### Customers

- Find customers by email address and/or phone number
- Get a customer's details, including their most recent orders
- Update a customer's basic profile fields
- Update a saved address on a customer's profile (does not change existing orders)

### Products & Inventory

- Get a product by id or by its URL handle, including its variants
- Search the store's products by title
- Check stock levels for a product variant by id or SKU across all locations
- Get basic information about the connected store (name, domain, plan, currency)

### Product Catalog Sync

Beyond the tools above, the Shopify plugin syncs your full product catalog into Hay in the background. That means your agent can search and recommend real products — with accurate prices, options, images, and availability — directly in conversations. The catalog re-syncs automatically on a schedule, and in self-hosted mode a background job also keeps your Shopify access token fresh so the connection never silently expires.

Your agent uses all of these tools automatically: when a customer asks "where's my order #1042?" or "can I get a refund?", the agent picks the right tool, checks your store, and answers with real data — escalating to your team when a decision needs a human.
