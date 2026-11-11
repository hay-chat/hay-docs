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

## Available Integrations

Each integration has its own guide covering what it is, how to connect it, and what your agent can do with it:

**Communication channels**

- [WhatsApp](/docs/user-guide/integrations/whatsapp/) - Answer WhatsApp messages via Twilio
- [Instagram](/docs/user-guide/integrations/instagram/) - Answer Instagram DMs
- [Chatwoot](/docs/user-guide/integrations/chatwoot/) - Run Hay as an agent bot inside Chatwoot inboxes

**E-commerce**

- [Shopify](/docs/user-guide/integrations/shopify/) - Orders, refunds, customers, products, and catalog sync
- [WooCommerce](/docs/user-guide/integrations/woocommerce/) - Full store access including orders, products, and coupons
- [Magento](/docs/user-guide/integrations/magento/) - Products, stock, and sales insights
- [Wix](/docs/user-guide/integrations/wix/) - Orders, refunds, fulfillment, and products

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

**Utilities**

- [Email](/docs/user-guide/integrations/email/) - Send notification emails to your team

### Web Chat

Add a live chat widget to your website. Web Chat is a built-in Hay feature, not a marketplace plugin.

**What it does:**

- Embeds a chat button on your site
- Customers click and start chatting instantly
- Conversations go straight to your Hay dashboard
- Fully customizable appearance

**How to set up:**

1. Go to **Settings** → **Webchat**
2. Copy the embed code
3. Paste it into your website (usually before the closing `</body>` tag)
4. Customize colors, position, and welcome message
5. Save and test!

**Customization options:**

- Button position (bottom-right, bottom-left, etc.)
- Brand colors
- Welcome message
- Chat window size

## Managing Integrations

### Viewing Installed Integrations

Go to **Integrations** → **Marketplace**. The **Connected** list at the top shows every integration you have added, with its status:

- **Connected**: working.
- **Finish setup**: added, but something is still missing (a sign-in, a required setting).

Connected integrations also appear under **Integrations** in the sidebar. An amber dot next to one means it still needs setup.

### Configuring an Integration

To adjust settings:

1. Click **Configure** next to the integration in the **Connected** list, or click it in the sidebar
2. Make your changes
3. Test the connection
4. Save

### Disconnecting an Integration

To remove an integration:

1. Open the integration's page
2. Open the **⋯** menu and choose **Disconnect and remove**
3. Confirm the action

Its settings are kept in case you connect it again. Admins can delete a plugin their organisation uploaded from the same menu, or from the plugin's page when it is not connected.

> ⚠️ **Note:** Active conversations on that channel may need to be handled manually after disconnection.

## Integration Marketplace

### Browsing Available Integrations

1. Go to **Integrations** → **Marketplace**
2. Search by name or by what you need (for example "refunds"), or pick a category: Channels, E-commerce, Payments and shipping, CRM and support, Knowledge and bookings, Security
3. Click any integration to open its page

Each card shows what the integration adds (for example "Agent tools · Catalog sync"). A green dot after the name means it is connected; an amber dot means it needs setup.

Can't find what you use? **Request an integration** at the bottom of the page, or build your own plugin and upload it for your organisation.

### Connecting an Integration

1. Open the integration's page. It shows what your agent can do with it, how connecting works, and the data it works with
2. Click **Connect**
3. Follow the setup steps (sign in, or enter the details it asks for)
4. Test the connection

## Channel-Specific Features

### Web Chat Features

- ✅ Typing indicators
- ✅ Read receipts
- ✅ File uploads
- ✅ Emoji support
- ✅ Conversation history
- ✅ Proactive chat (trigger by page, time on site, etc.)

### WhatsApp Features

- ✅ Plain-text message sending
- ✅ Plain-text message receiving
- 🔜 Rich media (images, videos, documents) — planned
- 🔜 Quick reply buttons — planned
- 🔜 Message templates — planned
- 🔜 Read receipts — planned

### Email Features

- ✅ Plain-text emails
- 🔜 HTML emails — planned
- 🔜 Attachments — planned
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

You can assign different:

- Agents to different channels
- Playbooks based on channel
- Response styles (formal on email, casual on WhatsApp)

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

- Use same agent instructions
- Same tone and style
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

### Can I use the same agent across all channels?

Yes, or you can assign different agents to different channels based on your needs.

### What happens if an integration fails?

You'll be notified immediately. Hay continues working, but new messages on that channel won't come through until reconnected.

### Are there limits on message volume?

Depends on the integration. Hay itself has no limits, but some platforms (like WhatsApp) have their own rate limits.

### Can I build my own integration?

Yes! Hay supports custom integrations via API. See the Developer documentation or contact support.

## Next Steps

- Set up your first integration with [Quick Start](/docs/user-guide/quick-start/)
- Configure [Agents](/docs/user-guide/agents/) for different channels
- Create channel-specific [Playbooks](/docs/user-guide/playbooks/)
- Monitor performance in [Analytics](/docs/user-guide/analytics/)
