---
layout: docs.njk
title: Correios
description: Connect your Correios contract so your AI agent can track a parcel by its code and tell the customer its status, latest movement and delivery date.
section: user-guide
---

## What is Correios?

Correios is the Brazilian postal service. Connecting your Correios contract to Hay.chat lets your AI agent track the parcels you posted, so it can answer "where is my order?" with what the carrier actually recorded.

The integration is read-only: it tracks parcels and does nothing else.

## Before You Start

You need a commercial contract with Correios. Correios only lets a contract track the parcels posted under it, so the agent can track your own shipments and nothing else.

If you buy your labels through a platform such as Melhor Envio, or post at the counter without a contract, connect [Melhor Envio](/docs/user-guide/integrations/melhor-envio/) instead.

## How to Connect Correios

1. Go to **Integrations** → **Marketplace** in the Hay.chat dashboard
2. Find **Correios** and click **Connect**
3. Enter your **Correios user**: the Meu Correios user of your company account, usually the CNPJ
4. Enter your **API access code**. Generate it at cws.correios.com.br under **Gestão de acesso a APIs**. It is not your Meu Correios password
5. Enter your **Cartão de postagem**, your **Contract number**, or both
6. Save. Hay.chat checks the credentials with Correios and tells you which one was refused if the check fails

## What Hay.chat Can Do with Correios

The integration gives the agent 1 tool.

### Track a parcel

- Look up a parcel by its 13-character tracking code, for example `AA123456789BR`
- See the current status (in transit, out for delivery, waiting for pickup, delivered), the latest movement with its place and time, the movement history, and the delivery date or estimate
- Give the customer the public Correios tracking link

The agent does not see or share who signed for a parcel.

### Parcels that look stuck

A parcel with no movement for more than **Stale tracking (days)** (default 5) is flagged as stuck, so the agent can say so and offer a hand-off instead of repeating the same status.

### When a parcel is not found

Correios answers "not found" for any code that was not posted under your contract. The agent explains this and gives the public tracking link. It does not tell the customer the parcel is lost.
