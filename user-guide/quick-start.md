---
layout: docs.njk
title: Quick Start Guide
description: Get your Hay AI assistant up and running in less than 10 minutes! Follow these simple steps to start automating your customer support.
section: user-guide
---

## Before You Begin

Make sure you have:

- A Hay account (sign up at your organization's Hay dashboard)
- At least one support document or FAQ (we'll use this to train your agent)
- 10 minutes to complete the setup

## Step 1: Choose Your Integrations

Hay works where your team already does. You can connect integrations now or skip this and come back later.

1. Go to **Integrations** → **Marketplace** in the left sidebar _(admin/owner only)_
2. Browse available integrations (Shopify, WhatsApp, Zendesk, etc.)
3. Click on an integration you want to add
4. Follow the simple connection steps

**Popular integrations:**

- **Web Chat** - Add a chat widget to your website
- **WhatsApp** - Handle WhatsApp Business messages
- **Shopify** - Access order and product information
- **Zendesk** - Sync with your help desk

> **Tip:** You can start with just the Web Chat widget and add more integrations later!

## Step 2: Create Your First Agent

Your agent is your AI assistant. Give it a personality and purpose.

1. Go to **Settings** → **Agents**
2. Click **Create Agent**
3. Fill in the basic information:

   - **Name:** Something descriptive like "Customer Support Agent"
   - **Description:** What this agent handles (e.g., "Handles order questions and product inquiries")
   - **Tone:** Choose how your agent should communicate (Professional, Casual, or Enthusiastic)

4. (Optional) Add specific instructions:

   - How to greet customers
   - What topics to avoid
   - When to escalate to a human

5. Click **Create Agent**

> **Example:** "Support Agent" with a "Casual" tone that "helps customers with orders, shipping, and product questions"

## Step 3: Upload Training Documents

Teach your agent by uploading your existing support materials.

1. Go to **Documents** in the left sidebar
2. Add content using one of two buttons:

   - **Import Document** - Upload files (PDFs, text files) or import content directly from a website URL
   - **Write Document** - Create a document from scratch at `/documents/new`

3. When importing from a website, use the **Import from Website** option to auto-crawl the site or provide a sitemap and pull in multiple pages at once

4. Wait a few seconds while Hay processes and learns from your documents

**What makes good training material?**

- ✅ Frequently Asked Questions
- ✅ Product documentation
- ✅ Return and refund policies
- ✅ Shipping information
- ✅ Common support scenarios

> **Tip:** Start with your top 10-20 FAQs. You can always add more later!

## Step 4: Create a Playbook

> **Note:** While playbooks are optional for general usage, the onboarding wizard requires completing this step before you can unlock the playground testing step.

Playbooks are step-by-step instructions for handling specific situations.

1. Go to **Playbooks** in the left sidebar
2. Click the **+ icon** or **Generate Playbook**
3. Set up the basics:

   - **Title:** "Welcome New Customers"
   - **Trigger:** "greeting" or "hello"
   - **Instructions:** What should happen when triggered

4. Write instructions in plain language:

   ```
   Greet the customer warmly. Ask how you can help them today.
   If they ask about orders, check their order status.
   If they need help with products, provide relevant information.
   ```

5. Click **Create Playbook**

> **Common playbook examples:**
>
> - Order status lookups
> - Return and refund requests
> - Product recommendations

## Step 5: Test Your Agent

Before going live with customers, test how your agent responds.

1. Go to **Conversations** in the left sidebar
2. Click **Conversation Playground**
3. Start typing messages like a customer would:

   - "Hi, I need help with my order"
   - "What's your return policy?"
   - "Do you ship internationally?"

4. Review the responses - do they sound right?
5. If needed, go back and:
   - Add more training documents
   - Adjust agent instructions
   - Update your playbooks

> **Tip:** Test with real questions your support team receives daily!

## You're All Set! 🎉

Your Hay agent is now ready to start helping customers!

### What Happens Next?

- **Automatic responses:** Your agent handles incoming questions instantly
- **Human escalation:** Complex issues are automatically sent to your team
- **Feedback collection:** Review customer feedback to improve your agent over time

### Next Steps

Now that you're up and running:

1. **Monitor Conversations** - Go to **Conversations** in the sidebar to check for conversations that need human attention
2. **Review Dashboard** - See how your agent is performing at a glance
3. **Add More Documents** - Keep training your agent with new information
4. **Create More Playbooks** - Automate more types of requests

> **Need help?** Head to the [Dashboard](/docs/user-guide/dashboard/) guide to learn about monitoring your conversations.
