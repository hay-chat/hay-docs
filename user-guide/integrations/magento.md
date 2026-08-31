---
layout: docs.njk
title: Magento
description: Connect Magento to Hay so your AI agent can look up products, check stock, review a customer's purchase history, and report on sales.
section: user-guide
---

## What is Magento?

Magento (Adobe Commerce) is a powerful open-source e-commerce platform popular with mid-size and enterprise stores that need deep catalog and multi-store flexibility. Connecting Magento to Hay gives your AI agent live access to your store's product catalog, stock levels, customer purchase history, and sales figures — so it can answer product and availability questions and pull up what a customer has ordered, right inside the conversation.

## How to Connect Magento

1. Go to **Integrations** → **Marketplace** in the Hay dashboard
2. Find **Magento** and click **Install**
3. Enter your **Magento Base URL** — your Magento 2 REST API address, e.g. `https://yourdomain.com/rest/V1`
4. Enter your **API Token**. Create one in your Magento admin under **System** → **Integrations**: add a new integration, grant it access to the catalog, sales, and customer resources you want Hay to use, activate it, and copy the access token
5. Save — Hay tests the connection against your store and confirms the credentials work

## What Hay Can Do with Magento

### Products & Stock

- Look up a product by its SKU or by its numeric ID
- Search products by keyword, with support for browsing long result lists
- Run advanced product searches with custom filters and field criteria
- See which categories a product belongs to
- Find products related to a given product
- Check stock levels for a product by SKU
- View all of a product's attributes (base and custom)
- Update a product attribute (e.g. correct a description field)

### Customers

- Look up everything a customer has ordered by their email address, with detailed product information — perfect for "which one did I buy?" questions

### Sales Insights

- Total revenue for a date range, with order-status filtering
- Revenue broken down by country
- Order counts for a date range
- Quantity of products sold in a period, filterable by country and status

Your agent uses these tools automatically: when a shopper asks "is this in stock?" or "what did I order last month?", the agent queries your Magento store in real time and answers with actual catalog and order data, escalating to your team whenever a human needs to step in.
