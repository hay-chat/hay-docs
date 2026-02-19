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

## Available Integrations

### Web Chat (Webchat Plugin)

Add a live chat widget to your website.

**What it does:**

- Embeds a chat button on your site
- Customers click and start chatting instantly
- Conversations go straight to your Hay dashboard
- Fully customizable appearance

**How to set up:**

1. Go to **Integrations** → **Marketplace**
2. Find **Web Chat**
3. Click **Install**
4. Copy the embed code
5. Paste it into your website (usually before closing `</body>` tag)
6. Customize colors and position
7. Save and test!

**Customization options:**

- Button position (bottom-right, bottom-left, etc.)
- Brand colors
- Welcome message
- Chat window size

### WhatsApp Business

Handle WhatsApp messages with your AI agent.

**What it does:**

- Customers message your WhatsApp Business number
- Hay responds automatically
- Full conversation history in dashboard
- Works 24/7

**How to set up:**

1. Go to **Integrations** → **Marketplace**
2. Find **WhatsApp**
3. Click **Connect**
4. Sign in with Facebook Business account
5. Select your WhatsApp Business number
6. Authorize Hay
7. Done!

**Requirements:**

- WhatsApp Business account
- Facebook Business Manager account
- Phone number verified with WhatsApp

**Perfect for:**

- International customers
- Mobile-first audiences
- Markets where WhatsApp is popular
- Quick, informal support

### Shopify

Connect your Shopify store for e-commerce support.

**What it does:**

- Look up order information
- Check product availability
- Access customer purchase history
- Provide tracking numbers

**How to set up:**

1. Go to **Integrations** → **Marketplace**
2. Find **Shopify**
3. Click **Install**
4. You'll be redirected to Shopify
5. Click **Install app** in your Shopify admin
6. Authorize permissions
7. Return to Hay - connection confirmed!

**What your agent can do:**

- "Where is my order?" → Look up order status
- "When will it arrive?" → Provide tracking info
- "I want to return this" → Check return eligibility
- "Is this in stock?" → Check inventory

**Permissions needed:**

- Read orders
- Read products
- Read customers

### Zendesk

Sync with your Zendesk help desk.

**What it does:**

- Import existing tickets into Hay
- Create tickets from Hay conversations
- Sync conversation history
- Unified customer view

**How to set up:**

1. Go to **Integrations** → **Marketplace**
2. Find **Zendesk**
3. Click **Connect**
4. Enter your Zendesk subdomain
5. Enter API token (generate in Zendesk settings)
6. Test connection
7. Choose sync preferences

**Sync options:**

- Import all tickets → Hay learns from ticket history
- Two-way sync → Updates flow both ways
- One-way sync → Hay creates tickets in Zendesk

### Email Integration

Handle email support with AI.

**What it does:**

- Monitor support email inbox
- AI responds to emails automatically
- Escalate when needed
- Maintain email thread history

**How to set up:**

1. Go to **Integrations** → **Email**
2. Choose method:
   - **Forwarding** - Forward emails to a Hay address
   - **IMAP** - Connect directly to your mailbox
3. Configure settings
4. Test with a sample email

**Best for:**

- Existing support email addresses
- Customers who prefer email
- Detailed, non-urgent questions

## Managing Integrations

### Viewing Installed Integrations

Go to **Integrations** in the sidebar to see:

- All installed integrations
- Connection status (✅ Connected or ⚠️ Issue)
- Last sync time
- Usage statistics

### Configuring an Integration

To adjust settings:

1. Click on the integration
2. Click **Settings** or **Configure**
3. Make your changes
4. Test the connection
5. Save

### Disconnecting an Integration

To remove an integration:

1. Go to the integration
2. Click **Disconnect** or **Uninstall**
3. Confirm the action
4. Integration is removed

> ⚠️ **Note:** Active conversations on that channel may need to be handled manually after disconnection.

## Integration Marketplace

### Browsing Available Integrations

1. Go to **Integrations** → **Marketplace**
2. Browse categories:
   - Communication Channels
   - E-commerce
   - CRM & Support
   - Productivity Tools
3. Click any integration to learn more

### Installing from Marketplace

1. Find the integration you want
2. Click **Install** or **Connect**
3. Follow the setup wizard
4. Authorize necessary permissions
5. Test the integration
6. Start using it!

### Suggesting New Integrations

Don't see what you need?

1. Click **Suggest an Integration**
2. Tell us what tool you want to connect
3. Explain your use case
4. We'll consider it for future development

## Channel-Specific Features

### Web Chat Features

- ✅ Typing indicators
- ✅ Read receipts
- ✅ File uploads
- ✅ Emoji support
- ✅ Conversation history
- ✅ Proactive chat (trigger by page, time on site, etc.)

### WhatsApp Features

- ✅ Rich media (images, videos, documents)
- ✅ Quick reply buttons
- ✅ Message templates (for notifications)
- ✅ Read receipts
- ✅ Delivered status

### Email Features

- ✅ HTML emails
- ✅ Attachments
- ✅ CC/BCC support
- ✅ Thread continuity
- ✅ Signature management

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

For integrations like Shopify or Zendesk:

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

**Shopify:**

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
