---
layout: docs.njk
title: Quick Start Guide
description: Get your Hay AI assistant up and running in less than 10 minutes! Follow these simple steps to start automating your customer support.
section: user-guide
---

## Before You Begin

Make sure you have:

- A Hay account (sign up at your organization's Hay dashboard)
- Your website address, or at least one support document or FAQ (we'll use this to train your assistant)
- 10 minutes to complete the setup

## The fast way: the Set up board

**Set up** is the first item in the left sidebar. It asks three questions (what kind of business you run, what tools you use, what customers ask most), reads your website, tests your assistant on each question and tells you what to fix next, with a test chat alongside. See [Set Up Your Assistant](/docs/user-guide/setup/). The steps below are the manual route and cover the same ground.

## Step 1: Choose Your Integrations

Hay works where your team already does. You can connect integrations now or skip this and come back later.

1. Go to **Integrations** → **Marketplace** in the left sidebar _(admin/owner only)_
2. Browse available integrations (Shopify, WhatsApp, Zendesk, etc.)
3. Click **Connect** on an integration you want to add
4. Follow the connection steps on its settings page

**Popular integrations:**

- **WhatsApp** - Handle WhatsApp Business messages
- **Shopify** - Access order and product information
- **HubSpot** - Sync contacts and tickets from your CRM
- **Zendesk** - Sync with your help desk

> **Tip:** You can also add a chat widget to your website under **Settings** → **Channels** → **Webchat**.

## Step 2: Set Up Your Assistant

Your assistant is the AI that answers your customers. Hay has one per organization; you give it a personality and rules.

1. Go to **Settings** → **Assistant** (the **Identity** tab)
2. Fill in:

   - **Name:** shown to customers in the chat widget
   - **Description:** an internal note
   - **Instructions:** the assistant's general behavior, in the rich text editor (how to greet customers, what topics to avoid, when to escalate)
   - **Initial Greeting Message** _(optional)_**:** the first message customers see
   - **Tone:** Professional, Casual or Enthusiastic

3. Click **Save Changes**

> **Example:** a "Casual" tone with instructions to help customers with orders, shipping and product questions.

## Step 3: Upload Training Documents

Teach your assistant by adding your existing support materials.

1. Go to **Documents** in the sidebar (under **Knowledge**)
2. Add content using the buttons at the top:

   - **Connected sources** - manage connected data sources
   - **Write Document** - create a document from scratch
   - **Import Document** - upload files (PDF, TXT, MD, DOC, DOCX, PPT, PPTX, HTML, JSON, CSV) or import from a website

3. When importing from a website, choose **Import from Website**, enter the URL, click **Discover Pages**, pick the pages you want and finish the import
4. Wait a few seconds while Hay processes and learns from your documents

**What makes good training material?**

- ✅ Frequently Asked Questions
- ✅ Product documentation
- ✅ Return and refund policies
- ✅ Shipping information
- ✅ Common support scenarios

> **Tip:** Start with your top 10-20 FAQs. You can always add more later!

## Step 4: Create a Playbook (Optional but Recommended)

Playbooks are step-by-step instructions for handling specific situations.

1. Go to **Playbooks** in the sidebar (under **Knowledge**)
2. Click **Generate Playbook** (the AI wizard), **From template**, or the **+** button to write one yourself
3. Set up the basics:

   - **Title:** "Welcome New Customers"
   - **Trigger:** "This playbook activates when the customer greets or says hello"
   - **Description** _(optional)_
   - **Status:** new playbooks default to "Draft" and won't trigger until activated. Set it to "Active"

4. Write instructions in plain language:

   ```
   Greet the customer warmly. Ask how you can help them today.
   If they ask about orders, check their order status.
   If they need help with products, provide relevant information.
   ```

5. Click **Create Playbook**. Later edits to the instructions go live when you click **Publish**

> **Common playbook examples:**
>
> - Order status lookups
> - Return and refund requests
> - Product recommendations

## Step 5: Test Your Assistant

Before going live with customers, test how your assistant responds.

1. Use the **Test your assistant** chat on the **Set up** board, or go to **Conversations** and click **Conversation Playground**
2. Start typing messages like a customer would:

   - "Hi, I need help with my order"
   - "What's your return policy?"
   - "Do you ship internationally?"

3. Review the responses - do they sound right?
4. If needed, go back and:
   - Add more training documents
   - Adjust the assistant's instructions
   - Update your playbooks

To test against real history, use **Tests** under **Quality**: it replays your past conversations and shows what the assistant would have done. See [Tests](/docs/user-guide/tests/).

> **Tip:** Test with real questions your support team receives daily!

## You're All Set! 🎉

Your Hay assistant is now ready to start helping customers!

### What Happens Next?

- **Automatic responses:** Your assistant handles incoming questions instantly
- **Human escalation:** Complex issues are automatically sent to your team
- **Continuous learning:** Your assistant improves as you add documents and playbooks

### Next Steps

Now that you're up and running:

1. **Check the Inbox** - it lists conversations that need a person, and you can turn on **Alerts on this device**
2. **Review the Dashboard** - see how your assistant is performing at a glance
3. **Add More Documents** - Keep training your assistant with new information
4. **Create More Playbooks** - Automate more types of requests

> **Need help?** Head to the [Dashboard](/docs/user-guide/dashboard/) guide to learn about monitoring your conversations.
