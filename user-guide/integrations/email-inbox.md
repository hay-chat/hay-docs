---
layout: docs.njk
title: Email inbox
description: Forward your support mailbox to Hay so the AI agent answers customer emails in the same thread, and take over any conversation from the dashboard.
section: user-guide
---

## What is the Email inbox channel?

Many customers still write to `support@yourstore.com`. The Email inbox channel turns that mailbox into a Hay channel without changing where your email lives: you keep your Gmail, Google Workspace, Outlook or Microsoft 365 account and simply forward a copy of incoming mail to a Hay address that is unique to your workspace. Hay's AI agent answers by email, every reply stays in the customer's thread, and you can step in from the dashboard at any time.

> **Different from the "Email" tool.** The [Email](/docs/user-guide/integrations/email/) integration lets the agent send notification emails to your team. The Email inbox channel receives and answers customer emails.

## How to Connect Your Inbox

1. In the Hay dashboard, go to **Integrations** → **Marketplace**
2. Find **Email inbox** and click **Install**
3. Enter **Your support address** — the address customers write to, for example `support@yourstore.com` — and save. Optionally set a **Sender name** (defaults to your workspace name) and turn on **Keep a copy in my inbox** if you want every reply Hay sends BCC'd to your support address
4. Below the settings, copy **Your Hay forwarding address**. It looks like `in-3f9a2b7c1d@inbound.hay.chat` and only your workspace can use it
5. Set up forwarding in your mail provider:
   - **Gmail / Google Workspace** — as the support mailbox, open Settings → See all settings → Forwarding and POP/IMAP → Add a forwarding address → paste the Hay address → Next. Google emails Hay a confirmation code; it appears on the Hay setup page within a minute. Enter it in Gmail, choose *Forward a copy of incoming mail* and save
   - **Outlook / Microsoft 365** — as the support mailbox, open Settings → Mail → Forwarding → Enable forwarding → paste the Hay address → tick *Keep a copy of forwarded messages* → Save. Some Microsoft 365 tenants need an administrator to allow external forwarding
   - **Other providers** — any mailbox that can auto-forward works. Forward a copy rather than redirect-and-delete so your inbox keeps the original
6. Send a test email to your support address from a personal account. It appears under **Conversations** as an email conversation and the agent replies to you by email

Then make sure an agent handles the channel: open the agent's settings and under **Channels** select **Email inbox**. If no agent is assigned, your default agent answers.

## What Hay Can Do with Email

- **Receive customer emails** — forwarded messages become conversations titled with the email subject. Quoted history and signatures are stripped so the agent reads only what the customer wrote this time; attachments are kept with the message
- **Reply in the thread** — replies are sent as *"Your store name" <support@…hay.chat>* with the headers email clients use to group a thread, so the customer sees one conversation. Replying to Hay's email lands in the same Hay conversation
- **Human takeover** — replies you write in the dashboard go out by email too. If someone at your store answers a customer directly from your own mailbox, Hay notices the customer's next reply refers to a message it never sent, stops the AI on that conversation and lists it under **Needs attention**
- **Stay out of trouble** — bounces, out-of-office auto-replies and mailing-list traffic are never answered. Emails that fail the sender's DMARC check are stored but held for a human instead of being answered automatically

## Good to Know

- Replies currently come from an address on Hay's domain with your store name as the sender. Sending from your own domain is planned
- Hay does not read your mailbox or hold its credentials; it only receives what you forward
- The forwarding address is unique to your workspace. Mail sent to it that is not forwarded from your support address is ignored
- Your inbox keeps its own copy of every incoming email. Turn on **Keep a copy in my inbox** to also receive Hay's replies there
