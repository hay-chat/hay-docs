---
layout: docs.njk
title: WooCommerce
description: Connect WooCommerce to Hay so your AI agent can look up orders, check products and stock, manage coupons and refunds, and support your store's customers.
section: user-guide
---

## What is WooCommerce?

WooCommerce is the most widely used e-commerce plugin for WordPress, powering online stores of every size on top of a regular WordPress site. Connecting WooCommerce to Hay gives your AI agent live access to your store's orders, products, customers, coupons, and reports — so it can answer order-status questions, check availability, and handle store tasks directly in customer conversations.

## How to Connect WooCommerce

1. Go to **Integrations** → **Marketplace** in the Hay dashboard
2. Find **WooCommerce** and click **Install**
3. Enter your **WordPress Site URL** — your WordPress/WooCommerce site address, e.g. `https://mystore.com`
4. Enter your **Consumer Key** and **Consumer Secret**. Generate these in your store's admin under **WooCommerce** → **Settings** → **Advanced** → **REST API** → **Add key** (give the key Read/Write permissions so the agent can update orders when you want it to)
5. Optionally, enter a **WordPress Username (Optional)** and **WordPress Password (Optional)** — only needed if you also want the agent to work with WordPress content like blog posts
6. Save — Hay tests the connection against your store and confirms it's working

## What Hay Can Do with WooCommerce

The WooCommerce integration is Hay's most extensive store toolkit, covering the whole WooCommerce REST API. In day-to-day support conversations the agent mostly uses the order, product, and customer tools; the rest are there when you build automations or playbooks around them.

### Orders

- Look up a specific order or browse recent orders
- Create or update an order (e.g. change its status)
- Read and add order notes
- View and create order refunds

### Products & Inventory

- Look up a product or browse and filter the product list
- Create, update, or remove products, product variations, and product attributes
- Work with product categories and tags
- Read and manage product reviews
- Check stock levels via the stock report

### Customers

- Look up a customer or browse the customer list
- Create or update customer records

### Coupons

- Look up existing coupons
- Create, update, or remove coupons — handy for goodwill discounts during support

### Store Reports

- Sales, orders, products, categories, customers, stock, coupons, and taxes reports over a chosen period

### Store Settings & More

- Read shipping zones and methods, tax classes and rates, payment gateways, store settings, and system status
- Look up store reference data like countries and currencies
- With the optional WordPress credentials, read and create WordPress posts

Your agent uses these tools automatically in conversations: when a customer asks "where is my order?" or "is this still in stock?", the agent queries your store in real time and answers with actual order and product data — no manual lookups needed.
