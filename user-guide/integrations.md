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

Add a live chat widget to your website. Web Chat is a built-in Hay feature, not a marketplace plugin.

**What it does:**

- Embeds a chat button on your site
- Customers click and start chatting instantly
- Conversations go straight to your Hay dashboard
- Fully customizable appearance
- Shoppers can attach photos (JPG, PNG, GIF, WebP) or PDFs up to 10 MB, and the AI reads them
- Optional bot protection with Cloudflare Turnstile (see below)

**How to set up:**

1. Go to **Settings** → **Channels** → **Webchat**
2. Copy the **Installation Code**
3. Paste it into your website (usually before the closing `</body>` tag)
4. Customize colors, position, and welcome message
5. Save and test!

**Bot protection:** install the **Turnstile** plugin to stop bots from opening conversations. **Invisible** (recommended) checks visitors in the background and asks for a click only when Cloudflare is unsure; **Challenge everyone** shows the check before a visitor's first message; **Off** disables it. If one workspace suddenly gets a flood of new chats, Hay.chat can challenge every visitor for a few hours automatically.

**Customization options:**

- Widget position (right or left)
- Theme colors (blue, green, purple, black)
- Greeting message
- Teaser message next to the chat button, set in the dashboard or in code (see [Teaser message](/docs/user-guide/settings/#teaser-message))

## Managing Integrations

### Viewing Installed Integrations

**Integrations** in the sidebar is not itself a page — it's a parent item that expands to show a **Marketplace** link plus a link for each integration you've enabled.

- The **Marketplace** page shows aggregate stats: **Total Integrations**, **Installed**, **Channels** and **MCP Connectors**
- Each integration's own settings page shows its status (**Connected**, **Needs action** or **Not set up**) and has three tabs: **Setup**, **What it can do** and, when the integration has technical values, **Advanced**
- **Settings** → **Channels** → **Channel list** lists your active communication channels, each with a **Configure** button

### Configuring an Integration

To adjust settings:

1. Open the integration from the **Integrations** sidebar entry, or click **Configure** on its Marketplace card
2. On the **Setup** tab, change your details and click **Save**
3. Use **Test connection** under **Check the connection**

### Disconnecting an Integration

To remove an integration:

1. Go to the integration's settings page
2. Open the **More options** menu and choose **Disconnect and remove** (or click **Remove** on its Marketplace card)
3. Confirm with **Disconnect**
4. The integration stops working for your workspace; its settings are kept in case you connect it again

> ⚠️ **Note:** Active conversations on that channel may need to be handled manually after disconnection.

## Integration Marketplace

### Browsing Available Integrations

1. Go to **Integrations** → **Marketplace**
2. Browse the available integrations, or use **Search integrations...**
3. Narrow the list with the **Categories** filter: All Integrations, Channels, MCP Connectors, Document Importers, Retrievers or Playbooks

Admins can also use **Upload Integration** to add a custom integration as a ZIP file, and **Manage Custom** to enable, disable or delete the ones they uploaded.

### Installing from Marketplace

1. Find the integration you want
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
- Opening hours (**Settings** → **Channels** → **Business Hours**)
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

### Are there limits on message volume?

Depends on the integration. Hay itself has no limits, but some platforms (like WhatsApp) have their own rate limits.

### Can I build my own integration?

Yes! Hay supports custom integrations via API. See the Developer documentation or contact support.

## Next Steps

- Set up your first integration with [Quick Start](/docs/user-guide/quick-start/)
- Set up your [AI assistant](/docs/user-guide/agents/)
- Create [Playbooks](/docs/user-guide/playbooks/)
- Monitor performance on your [Dashboard](/docs/user-guide/dashboard/)
