---
layout: docs.njk
title: Integrations Setup Guide
description: Connect Hay to the tools and platforms you already use. Integrations let your AI agent work where your customers are - from your website to WhatsApp to your e-commerce platform.
section: user-guide
---

## What Are Integrations?

Integrations connect Hay with:

- **Communication channels** - Where customers reach you (WhatsApp, web chat, email)
- **Business tools** - Your e-commerce, CRM, help desk systems
- **Data sources** - Access to order info, customer data, inventory

> **Access:** Integrations are accessible only to users with the **Admin** or **Owner** role.

> **Store actions need your say-so.** When a store integration is connected, refunds, cancellations, discounts and exchanges wait for a person to approve them by default. See [Action approvals](/docs/user-guide/action-approvals/).

## Available Integrations

Each integration has its own guide covering what it is, how to connect it, and what your agent can do with it:

**Communication channels**

- [WhatsApp](/docs/user-guide/integrations/whatsapp/) - Answer WhatsApp messages through Meta's Cloud API
- [Messenger](/docs/user-guide/integrations/messenger/) - Answer Facebook Page messages
- [Instagram](/docs/user-guide/integrations/instagram/) - Answer Instagram DMs
- [Chatwoot](/docs/user-guide/integrations/chatwoot/) - Run Hay as an agent bot inside Chatwoot inboxes
- [Email inbox](/docs/user-guide/integrations/email-inbox/) - Forward your support mailbox, answer customer emails in-thread, and let the agent email your team

**E-commerce**

- [Shopify](/docs/user-guide/integrations/shopify/) - Orders, refunds, returns, exchanges, customers, products, and catalog sync
- [WooCommerce](/docs/user-guide/integrations/woocommerce/) - Orders, tracking, cancellations, refunds, goodwill coupons, and products
- [Magento](/docs/user-guide/integrations/magento/) - Orders, tracking, cancellations, refunds, and products
- [Wix](/docs/user-guide/integrations/wix/) - Orders, tracking, cancellations, refunds, and products
- [Nuvemshop / Tiendanube](/docs/user-guide/integrations/nuvemshop/) - Orders, Pix payment links, tracking, goodwill coupons, and products

**Shipping**

- [Correios](/docs/user-guide/integrations/correios/) - Track parcels posted under your Correios contract
- [Melhor Envio](/docs/user-guide/integrations/melhor-envio/) - Shipment status, carrier, and tracking for labels bought through Melhor Envio

**Payments & marketing**

- [Stripe](/docs/user-guide/integrations/stripe/) - Payments, refunds, subscriptions, and invoices
- [Klaviyo](/docs/user-guide/integrations/klaviyo/) - Profiles, subscriptions, campaigns, and flows

**CRM & scheduling**

- [HubSpot](/docs/user-guide/integrations/hubspot/) - Contacts, companies, deals, and tickets
- [Twenty CRM](/docs/user-guide/integrations/twenty/) - People, companies, notes, and tasks
- [Cal.com](/docs/user-guide/integrations/calcom/) - Check availability and manage bookings

**Help desk & knowledge**

- [Zendesk](/docs/user-guide/integrations/zendesk/) - Tickets, users, macros, and Help Center articles
- [Atlassian (Jira & Confluence)](/docs/user-guide/integrations/atlassian/) - Jira issues plus Confluence page import
- [Notion](/docs/user-guide/integrations/notion/) - Import Notion pages into your knowledge base

### Web Chat

Add a live chat to your website. Web Chat is a built-in Hay feature, not a marketplace plugin, and it is always the first card in **Channels**.

**What it does:**

- Adds a chat button to your site, in your brand colour
- Visitors ask a question from the chat's home view and the AI answers straight away
- Conversations go straight to your Hay inbox, where your team can take over
- Your logo and brand colour come from your website automatically
- Shoppers can attach photos (JPG, PNG, GIF, WebP) or PDFs up to 10 MB, and the AI reads them
- Optional bot protection with Cloudflare Turnstile (see below)

**How to set up:**

1. Set your website in **Settings** → **Workspace** → **General** so Hay can bring in your logo and colour
2. Go to **Settings** → **Channels** and click the **Webchat** card
3. On the **Install** tab, copy the code and paste it before the closing `</body>` tag on every page
4. Adjust the look on **Appearance** and the wording on **Content**, watching the live preview
5. Save. The Install tab shows **Working** once a page with the chat loads

**Bot protection:** install the **Turnstile** plugin to stop bots from opening conversations. **Invisible** (recommended) checks visitors in the background and asks for a click only when Cloudflare is unsure; **Challenge everyone** shows the check before a visitor's first message; **Off** disables it. If one workspace suddenly gets a flood of new chats, Hay.chat can challenge every visitor for a few hours automatically.

**Customization options:**

- Logo and any brand colour
- Theme: Light, Dark, or Match the visitor
- Button position (bottom right or bottom left) and the chat button icon
- The assistant's name and avatar
- Home heading, greeting, and a message next to the chat button (see [Message next to the chat button](/docs/user-guide/webchat/#message-next-to-the-chat-button))
- Pre-chat form and end-of-chat rating

See the [Webchat](/docs/user-guide/webchat/) guide for every setting and for what your visitors see.

## Managing Integrations

### Viewing Installed Integrations

**Integrations** in the sidebar is not itself a page — it's a parent item that expands to show a **Marketplace** link plus a link for each integration you've enabled.

- The **Marketplace** page starts with a **Connected** list of every integration you have added, each marked **Connected** or **Finish setup**
- In the sidebar, an amber dot next to an integration means it still needs setup
- Each integration's own settings page shows its status (**Connected**, **Needs action** or **Not working**) and has three tabs: **Setup**, **What it can do** and, when the integration has technical values, **Advanced**
- **Settings** → **Channels** → **Channel list** shows **Webchat** first, then your active channel integrations; click a card to open its settings

### Configuring an Integration

To adjust settings:

1. Open the integration from the **Integrations** sidebar entry, or click **Configure** next to it in the Marketplace's **Connected** list
2. On the **Setup** tab, change your details and click **Save**
3. Use **Test connection** under **Check the connection**

### Disconnecting an Integration

To remove an integration:

1. Go to the integration's settings page
2. Open the **More options** menu and choose **Disconnect and remove**
3. Confirm with **Disconnect**
4. The integration stops working for your workspace; its settings are kept in case you connect it again

Admins can delete an integration their organisation uploaded with **Delete plugin**: in the same menu when it is connected, or under **Details** on its page when it is not.

> ⚠️ **Note:** Active conversations on that channel may need to be handled manually after disconnection.

## Integration Marketplace

### Browsing Available Integrations

1. Go to **Integrations** → **Marketplace**
2. Search by name or by what you need (for example "refunds")
3. Narrow the list with a category: Channels, E-commerce, Payments and shipping, CRM and support, Knowledge and bookings, or Security
4. Click any integration to open its page

Each card shows what the integration adds (for example "Agent tools · Catalog sync"). A green dot after the name means it is connected; an amber dot means it needs setup.

Can't find what you use? Use **Request an integration** at the bottom of the page. Admins can also use **Upload plugin** to add a custom integration as a ZIP file, and **Manage custom** to enable, disable or delete the ones they uploaded.

### Connecting an Integration

1. Open the integration's page. Before you connect, it shows what your agent can do with it, how connecting works and the data it works with
2. Click **Connect**
3. Fill in your details on the **Setup** tab, or sign in to the platform when asked to give Hay.chat access
4. Use **Test connection** to check it works
5. Start using it!

## Channel-Specific Features

### Web Chat Features

- ✅ Typing indicators
- ✅ Emoji support
- ✅ Conversation history
- 🔜 Read receipts — planned
- ✅ Photo and PDF uploads (up to 10 MB)
- 🔜 Proactive chat (trigger by page, time on site, etc.) — planned

### WhatsApp Features

- ✅ Plain-text message sending
- ✅ Plain-text message receiving
- ✅ Receiving images, videos, documents and voice notes (voice notes are transcribed)
- 🔜 Quick reply buttons — planned
- 🔜 Message templates — planned
- 🔜 Read receipts — planned

### Email Features

- ✅ Plain-text emails
- ✅ HTML emails
- ✅ Receiving attachments
- 🔜 CC/BCC support — planned
- 🔜 Signature management — planned

## Best Practices

### Start with One Channel

Don't connect everything at once:

1. Start with your primary support channel
2. Train your agent
3. Monitor performance
4. Add more channels gradually

### Test Before Going Live

After connecting an integration:

1. Send test messages
2. Verify responses are correct
3. Check data access (orders, customers, etc.)
4. Ensure escalations work
5. Test on different devices

### Monitor Channel Performance

Different channels may have different needs:

- Email: More detailed, patient responses
- WhatsApp: Quick, concise messages
- Web Chat: Balance of speed and detail

### Customize Per Channel

Hay has one assistant for the whole organization, so it answers the same way on every channel. What you can set per channel:

- Whether the AI replies on its own, only drafts replies for a person to approve, or is off (**Settings** → **Privacy** → **Customer Privacy** → **AI on your channels**)
- Opening hours (**Settings** → **Workspace** → **Business Hours**)
- The label that tells shoppers a reply comes from AI

## Troubleshooting Integrations

### Integration Shows "Disconnected"

**Common causes:**

- Expired authentication token
- Changed password on the connected account
- Permissions revoked
- Service outage

**Solutions:**

1. Try reconnecting
2. Re-authorize permissions
3. Check integration status page
4. Contact support if issue persists

### Messages Not Appearing

**Check:**

- Integration is enabled
- Correct account/channel connected
- No filters blocking messages
- Webhook URL is correct (if applicable)

**Solutions:**

1. Send a test message
2. Check integration logs
3. Verify webhook configuration
4. Re-sync if needed

### Slow Message Delivery

**Possible causes:**

- High message volume
- External service delays
- Network issues

**Solutions:**

1. Check system status
2. Verify API rate limits
3. Contact support for optimization

### Data Not Syncing

For integrations like WooCommerce or Zendesk:

**Check:**

- Permissions are granted
- API credentials are correct
- Sync is enabled
- No data conflicts

**Solutions:**

1. Re-authorize the integration
2. Force a manual sync
3. Check error logs
4. Verify data format compatibility

## Security & Permissions

### What Permissions Do Integrations Need?

Each integration requests only what it needs:

**Web Chat:**

- Access to your domain
- Ability to load chat widget

**WhatsApp:**

- Access to WhatsApp Business account
- Send and receive messages

**WooCommerce:**

- Read orders
- Read products
- Read customer info (name, email only)

**Zendesk:**

- Read and create tickets
- Read customer data
- Update ticket status

### Keeping Integrations Secure

**Best practices:**

- ✅ Use separate credentials for integrations
- ✅ Grant minimum necessary permissions
- ✅ Review connected apps regularly
- ✅ Revoke unused integrations
- ✅ Monitor integration logs
- ✅ Update authentication when passwords change

## Multi-Channel Strategy

### When to Use Multiple Channels

**Good reasons:**

- Reach customers where they prefer
- Different regions prefer different channels
- Some questions better suited to specific channels
- 24/7 coverage across time zones

**Example setup:**

- **Website chat** - Quick questions during business hours
- **WhatsApp** - After-hours and international
- **Email** - Detailed inquiries and formal requests
- **Zendesk** - Internal team collaboration

### Maintaining Consistency

Across all channels:

- One assistant, one set of instructions and one tone
- Same documentation
- Same escalation procedures

But adapt message length:

- **WhatsApp** - Brief, mobile-friendly
- **Email** - More detailed, formatted
- **Web Chat** - Balanced, moderate length

## Common Questions

### Can I connect multiple channels at once?

Yes! Connect as many as you need. Each channel works independently.

### Will customers know they're talking to AI?

Best practice is to be transparent. Most integrations let you customize the greeting to mention AI assistance.

### Can I use a different assistant on each channel?

No. Hay has a single assistant per organization and it answers on every channel. Use [Playbooks](/docs/user-guide/playbooks/) for situation-specific behavior.

### What happens if an integration fails?

A failing channel shows **This channel is failing** on its settings page, and owners and admins are notified by email and push. Hay continues working, but messages on that channel may not come through until it is reconnected.

If a connection was revoked or expired, the email subject starts with **Action needed** and asks you to reconnect. Open the channel's settings page from the email and sign in again.

### Will Hay warn me before a connection expires?

Yes. Hay renews connections such as Instagram automatically, well before they expire. If the provider keeps refusing to renew one, owners and admins get an **Action needed** email and push about 3 days before it expires, and the channel's settings page shows **This channel's connection is about to expire** with the date. Reconnect before then and messages keep flowing. The warning clears by itself if a later renewal succeeds.

### Are there limits on message volume?

Depends on the integration. Hay itself has no limits, but some platforms (like WhatsApp) have their own rate limits.

### Can I build my own integration?

Yes! Hay supports custom integrations via API. See the Developer documentation or contact support.

## Next Steps

- Set up your first integration with [Quick Start](/docs/user-guide/quick-start/)
- Set up your [AI assistant](/docs/user-guide/agents/)
- Create [Playbooks](/docs/user-guide/playbooks/)
- Monitor performance on your [Dashboard](/docs/user-guide/dashboard/)
