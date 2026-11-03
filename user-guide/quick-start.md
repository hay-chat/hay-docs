---
layout: docs.njk
title: How do I set up Hay.chat?
description: "Set up Hay.chat in five steps: connect your store, import your help docs, write playbooks, test in the Playground, then switch the AI on for shoppers."
section: user-guide
---

> TLDR: Connect your store, import your help docs, write a few playbooks, test in the Playground, then switch the AI on. How long it takes depends mostly on how ready your docs and policies are, and your 30-day free trial starts at sign-up, not at go-live.

## Before you begin

You'll need:

- A Hay.chat account. The 30-day free trial starts when you sign up and needs no credit card.
- The **Admin** or **Owner** role, if you're connecting integrations.
- Your help docs and policies (returns, refunds, shipping) in whatever form you already have them.

The **Getting Started** page in the left sidebar walks you through these steps as a checklist, tracks your progress and links straight into each one.

## Step 1: Connect your store

1. Go to **Integrations** → **Marketplace** in the left sidebar.
2. Find your store platform and follow its connection steps.

Hay.chat has store plugins for [Shopify](/docs/user-guide/integrations/shopify/), [WooCommerce](/docs/user-guide/integrations/woocommerce/), [Magento](/docs/user-guide/integrations/magento/), [Wix](/docs/user-guide/integrations/wix/) and [Nuvemshop](/docs/user-guide/integrations/nuvemshop/). If your shop is custom-built or headless, see [Can Hay.chat support a custom or headless store?](/docs/user-guide/integrations/no-store-plugin/)

Once a store is connected, refunds, cancellations and discounts wait for your approval by default. See [Action approvals](/docs/user-guide/action-approvals/).

## Step 2: Import your help docs

Go to **Documents** in the left sidebar and add your material:

- **Write Document** to create one from scratch
- **Import Document** to upload files (PDF, TXT, MD, DOC, DOCX, PPT, PPTX, HTML, JSON, CSV)
- **Import from Website** to crawl your site or a sitemap and pull in several pages at once

Hay.chat reads what you add and answers shoppers from it.

**What makes good material?**

- Frequently asked questions
- Product documentation
- Return and refund policies
- Shipping information
- Common support scenarios

Start with the questions you're asked most. You can add more at any time.

## Step 3: Write a few playbooks

If you haven't made an agent yet, create one under **Settings** → **Agents**. Give it a name, a description of what it handles, a tone and some general instructions, then click **Create Agent**.

Then go to **Playbooks** and create a playbook for a situation you handle often. Give it a title, a trigger and instructions, and set its **Status** to **Active** (new playbooks start as Draft and won't run until you change this). Write the instructions in plain language:

```
Greet the customer warmly. Ask how you can help them today.
If they ask about orders, check their order status.
If they need help with products, provide relevant information.
```

For anything you always want a person to handle, add a **Handoff to human** step to the playbook. See [Playbooks](/docs/user-guide/playbooks/) for more.

## Step 4: Test in the Playground

Go to **Conversations** → **Conversation Playground** and type the way a shopper would:

- "Hi, I need help with my order"
- "What's your return policy?"
- "Do you ship internationally?"

Check the answers. If something's off, fix the doc or playbook behind it and try again. Test chats are never billed.

Before going live, check your approval settings under **Settings** → **Assistant** → **Actions**.

## Step 5: Switch the AI on for shoppers

Add the web chat widget to your site under **Settings** → **Webchat**. The channels that are live are web chat, the [email inbox](/docs/user-guide/integrations/email-inbox/) and [Instagram DMs](/docs/user-guide/integrations/instagram/).

Then set your business hours under **Settings** → **Channels** → **Business Hours**, and turn on phone alerts from the **Inbox** with **Alerts on this device**, so hand-offs reach you. See [Team alerts and business hours](/docs/user-guide/team-alerts-and-hours/).

## How long does setup take?

It depends mostly on how ready your docs and policies are, and on whether your store connects straight away. If your help centre is up to date, most of the work is importing it and checking the answers in the Playground. If your policies live in people's heads, writing them down is the slow part.

Your trial starts at sign-up, not at go-live, so it helps to have your docs and policies to hand before you sign up.

## What happens next

- Chats that need a person land in the [Inbox](/docs/user-guide/conversations/).
- Billable resolutions show on the Usage page under **Settings** → **Billing & Usage** → **Usage**. See [Billing and usage](/docs/user-guide/billing-and-usage/).
- Add docs and playbooks as new questions come in.
