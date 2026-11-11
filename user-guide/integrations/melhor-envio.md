---
layout: docs.njk
title: Melhor Envio
description: Connect your Melhor Envio account so your AI agent can answer "where is my order?" with the shipment's status, carrier, tracking code and link, and flag parcels that look stuck.
section: user-guide
---

## What is Melhor Envio?

Melhor Envio is a Brazilian shipping platform that lets stores buy labels from Correios, Jadlog and other carriers without a contract of their own. Connecting it to Hay.chat lets your AI agent see the shipments you created there, so it can tell a customer where their parcel is right in the conversation.

The integration is read-only. It does not quote freight, buy, print or cancel labels, and it never uses your wallet balance.

## How to Connect Melhor Envio

1. Go to **Integrations** → **Marketplace** in the Hay.chat dashboard
2. Find **Melhor Envio** and click **Connect**
3. Click **Connect with Melhor Envio**, sign in to your Melhor Envio account and approve the Hay.chat app

Hay.chat asks for one permission only: viewing your labels. The connection is renewed automatically. It ends if you revoke the app in your Melhor Envio panel under **Integrações** → **Catálogo** → **Integrações ativas**.

**Using an access token instead:** if the one-click connection is not available on your installation, set **Connection mode** to **Access token**. Generate a token in your Melhor Envio panel (**Integrações** → **Permissões de acesso**) with permission to view labels, then paste it together with a technical contact email, which Melhor Envio requires on every request. Hay.chat verifies the token when you save.

## What Hay.chat Can Do with Melhor Envio

The integration gives the agent 2 tools.

### Track a shipment

- Look up a shipment by its carrier tracking code, its Melhor Envio protocol (`ORD-…`) or its shipment id
- See the status, carrier and service, tracking code and link, posting and delivery dates and the delivery estimate

### Find a customer's shipments

- List the shipments sent to a customer by their CPF or CNPJ, most recent first
- This only runs for a customer whose identity is verified, and only returns shipments whose recipient email or phone is theirs

### Parcels that look stuck

A posted shipment that has not been delivered after more than **Stale tracking (days)** (default 5) is flagged as stuck, so the agent can say so and offer a hand-off instead of repeating the same status.

Melhor Envio reports each shipment's stage and dates, not every carrier scan. For the full scan history the agent gives the customer the tracking link.
