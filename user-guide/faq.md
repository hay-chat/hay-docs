---
layout: docs.njk
title: Frequently Asked Questions
description: Quick answers to common questions about Hay, from setup to billing and troubleshooting.
section: user-guide
---

## General Questions

### What is Hay?

Hay is an AI-powered customer support platform that handles conversations automatically using artificial intelligence. It connects to your existing channels (website, WhatsApp, email, etc.) and responds to customer questions 24/7.

### How is Hay different from traditional chatbots?

Traditional chatbots follow rigid scripts and break easily. Hay uses AI to understand context, learn from your documentation, and respond naturally. It knows when it needs help and escalates appropriately.

### Do I need technical skills to use Hay?

No! Hay is designed for non-technical users. You can set up and manage everything through a simple web interface. No coding required.

### How long does it take to set up?

You can be up and running in about 10 minutes with basic setup. Full optimization typically takes 2-4 weeks as you add more documentation and refine your agents.

### What languages does Hay support?

Hay can understand and respond in over 50 languages. Your documents can be in any language, and the AI will match the customer's language.

## Pricing & Plans

### How much does Hay cost?

Hay has four plans (Solo, Starter, Growth and Enterprise), billed monthly or annually. Every plan includes a number of **resolutions** per billing period; a resolution is a conversation the AI closes without a human. Current prices are on [hay.chat/pricing](https://hay.chat/pricing/).

### Which currencies can I pay in?

Euro, US dollar, British pound and Brazilian real. You pick the currency when you first check out (the pricing page pre-selects it from your region). Stripe fixes a customer's currency with that first subscription, so later plans, including a new subscription after a cancellation, stay in it. To move to another currency, contact support and we set up a new billing customer for you.

### Is there a free trial?

Yes. Every new workspace starts on a free trial; the dashboard shows how many days remain. When the trial ends you pick a plan on [hay.chat/billing](https://hay.chat/billing/).

### What's included in my plan?

- A number of AI resolutions per billing period (annual plans get twelve months' worth up front)
- Every channel and integration
- The support level listed for your plan on [hay.chat/pricing](https://hay.chat/pricing/)

### What happens if I exceed my resolution limit?

You are warned by email at 50 %, 80 % and 100 % of your included resolutions, and the dashboard shows a banner from 80 %. What happens at 100 % depends on the **Allow extra resolutions** switch on [hay.chat/billing](https://hay.chat/billing/):

- **On (default):** the AI keeps working and each extra resolution is billed at your plan's per-resolution rate on the next invoice.
- **Off:** the AI pauses. New customer messages wait in **Needs Attention** for your team, your dashboard keeps working, and nothing extra is billed. The AI resumes on its own when the next period starts, or immediately when you switch extra resolutions back on or upgrade. Conversations that were parked while the AI was paused stay in Needs Attention until your team replies or hands them back with **Return to AI**.

### Can I cancel anytime?

Yes. Monthly plans can be cancelled at any time and run until the end of the paid period. Annual plans are cancelled the same way; see the refund policy for the early-exit terms.

## Getting Started

### What do I need to get started?

To start using Hay effectively, you need:

1. A Hay account
2. Basic customer support documentation (FAQs, policies)
3. At least one communication channel to connect
4. 10-15 minutes for initial setup

### Should I start with one agent or multiple?

Start with one general agent. Add specialized agents later only if you have distinctly different use cases (e.g., sales vs. support).

### How many documents should I upload initially?

Start with 15-25 essential documents covering your most common customer questions. You can always add more as you discover gaps.

### Can I test before going live with customers?

Absolutely! Use the Playground mode to test your agent with sample questions before connecting it to real customer channels.

## Agents & AI

### How does the AI "learn"?

The AI doesn't learn automatically. It gets "smarter" when you:

- Upload more documentation
- Create better playbooks
- Refine agent instructions
- Add examples and context

### Will the AI make mistakes?

Occasionally, yes. That's why Hay has confidence thresholds and escalates when unsure. You can minimize mistakes by providing clear, comprehensive documentation.

### Can the AI access external systems?

Yes, through integrations. For example, it can look up orders in Shopify or check tickets in Zendesk, if you've connected those integrations.

### Can I control what the AI says?

Yes! You control the AI through:

- Agent instructions (tone, style, what to say/not say)
- Documents (what information to use)
- Playbooks (specific workflows)
- Test mode (approve each response)

### Will customers know they're talking to AI?

That's up to you. Best practice is transparency - let customers know they're chatting with an AI assistant that can connect them to a human if needed.

## Conversations

### Can customers talk to a human anytime?

Yes! Customers can request a human anytime by saying things like "I need to speak to a person." The AI also escalates automatically when appropriate.

### What happens to conversations when someone takes over?

When a human takes over, the AI stops responding. The full conversation history is preserved, so the human has complete context.

### How long are conversations stored?

By default, conversations are kept indefinitely. If a retention period is configured, conversations are anonymized (messages and personal data removed) after the specified number of days. Check **Settings** → **Customer Privacy** for your specific retention policy.

### Can I delete conversations?

Conversations can be deleted from the conversation view. If data retention is configured, conversations are automatically anonymized after the retention period. For GDPR deletion requests on behalf of customers, use the data deletion tool in **Settings** → **Customer Privacy**.

## Integrations

### Which integrations are available?

Popular integrations include:

- Web Chat (embed on your website)
- WhatsApp Business
- Email
- Shopify
- Zendesk

Check **Integrations** → **Marketplace** for the complete list.

### Can I connect multiple channels?

Yes! You can connect as many channels as you need. The same agent can work across all channels, or you can assign different agents to different channels.

### Do integrations cost extra?

Most integrations are included in your plan. Some third-party services (like WhatsApp) may have their own fees.

### Can I build custom integrations?

Yes! Hay has an API for custom integrations. Contact support for developer documentation.

### Will my Shopify/Zendesk data be secure?

Yes. Hay uses secure API connections and only requests the minimum permissions needed. Your data is encrypted and protected.

## Performance & Quality

### What's a good resolution rate?

Resolution rate varies by industry:

- Simple FAQ support: 80-90%
- E-commerce: 70-85%
- Technical support: 60-75%
- Complex B2B: 50-70%

### Why is my resolution rate low?

Common causes:

- Not enough documentation
- Unclear agent instructions
- Complex questions outside AI's scope
- Confidence threshold set too high

See the [Troubleshooting](/docs/user-guide/troubleshooting/) guide for solutions.

### How can I improve customer satisfaction?

1. Upload comprehensive documentation
2. Write clear, empathetic agent instructions
3. Respond quickly to escalations
4. Regularly review and optimize
5. Learn from negative feedback

### How fast should the AI respond?

AI typically responds in:

- 1-3 seconds for simple questions
- 3-8 seconds for document lookups
- 8-15 seconds for complex queries

If slower, check your document organization and agent complexity.

## Documents & Training

### What types of documents work best?

Most effective:

- FAQs (question and answer format)
- Policy documents (returns, shipping, privacy)
- Product information and specs
- Troubleshooting guides
- Step-by-step instructions

### How often should I update documents?

Update documents:

- Immediately when policies change
- Weekly to add new FAQs from recent conversations
- Monthly for full content review
- Quarterly for major refresh

### Can I upload documents in different languages?

Yes! Upload documents in any language. The AI can work with multilingual content.

### What if my document has wrong information?

Simply edit or re-upload the correct version. Changes take effect immediately.

### Is there a limit to how many documents I can upload?

Depends on your plan. Contact your account administrator for storage limit details.

## Queue & Escalations

### Why do conversations escalate?

Common reasons:

- Customer explicitly asks for human
- AI isn't confident in its answer (below threshold)
- Negative sentiment detected (frustrated customer)
- Complex issue beyond AI's capabilities
- Policy exception or special handling needed

### How quickly should I respond to escalations?

Recommended targets:

- Urgent/frustrated customers: 5-15 minutes
- Standard escalations: 30-60 minutes
- Low priority: 4 hours

Set your own SLAs based on your business needs.

### Can I automatically assign escalations to specific team members?

Today, assignment is manual: a team member can take over a conversation from the queue and it's assigned to them. Automatic routing rules (by topic, time of day, availability, or customer tier) are not yet available — coming soon.

### What if the queue gets too long?

Short term:

- Prioritize urgent items
- Add temporary support coverage
- Set customer expectations

Long term:

- Train AI to handle more cases
- Add better documentation
- Adjust escalation triggers
- Hire more support staff

## Security & Privacy

### Is my data secure?

Yes. Hay uses:

- Encryption in transit and at rest
- Secure password-based authentication (2FA policy toggle available in Settings → Security, backend enforcement in progress)
- Regular security audits
- Compliance with data protection regulations

### Is Hay GDPR compliant?

Yes. Hay provides tools for:

- Data access requests
- Data deletion (right to be forgotten)
- Data portability (export)
- Consent management

### Can I control who accesses what?

Yes! Use role-based access control:

- Owner: Full access
- Admin: Settings and management
- Member: Standard team access
- Contributor: Contribute content (documents, playbooks)
- Agent: Conversations only
- Viewer: Read-only access

### Where is my data stored?

Data is stored in secure, encrypted databases. Contact your account manager for specific data center locations relevant to your region.

### Can I export my data?

Yes! Go to **Settings** → **Privacy** to export your own account data (profile, audit logs, documents you authored, and uploads). For customer data exports, use **Settings** → **Customer Privacy**.

> **Note:** Analytics export is not yet available.

## Technical Questions

### What browsers are supported?

Hay works best on:

- Chrome (recommended)
- Firefox
- Safari
- Edge (Chromium-based)

Always use the latest version for best performance.

### Can I use Hay on mobile?

Yes, through your phone's browser. There is no app to install. The dashboard adapts to small screens, and the **Home** screen is built for the phone: it lists the conversations waiting for a human, longest wait first, with a **Take over** button on each row.

To be alerted when a customer needs a human while the dashboard is closed, turn on **Alerts on this device** (on Home, or under **Settings → Profile → Notifications**). On iPhone and iPad, first add Hay to your Home Screen (Share → **Add to Home Screen**) and open it from there; iOS only allows alerts for web apps installed that way.

### Do I need to install anything?

No installation needed! Hay is fully web-based. Just log in through your browser.

### What if Hay is down?

Check status at [status.hay.ai](https://status.hay.ai) (example URL). For urgent issues during outages, check for status updates and estimated resolution times.

### Can I use API to automate things?

Yes! Hay has a full API for:

- Creating conversations
- Sending messages
- Accessing analytics
- Managing agents and documents

Contact support for API documentation.

## Billing & Account

### How do I change my plan?

Go to [hay.chat/billing](https://hay.chat/billing/), open **Plans** and choose the new plan or billing interval. The change applies to your existing subscription immediately and Stripe prorates the difference: between two monthly plans it lands on your next invoice; switching between monthly and yearly starts a new billing cycle and is invoiced right away. Upgrades and downgrades both work this way; nothing creates a second subscription. Payment details and invoices are managed in the Stripe customer portal, reachable from the same page.

### Where do I see my usage?

The overview on [hay.chat/billing](https://hay.chat/billing/) shows resolutions used against your allowance for the current period, the percentage, when the period renews and whether extra resolutions are allowed.

### Can I get a refund?

Refund terms are in the [refund policy](https://hay.chat/refund-policy/). Contact support with your workspace name and invoice number.

## Support

### How do I contact support?

- **Email:** support@hay.ai (example)
- **In-app:** Click the Help icon in the dashboard
- **Community:** Join our community forum

### What are support hours?

Typically Monday-Friday, business hours in your region. Check your plan for specific support hours.

### How fast will I get a response?

Response times vary by plan and urgency:

- Critical issues: 1-4 hours
- Normal issues: 24 hours
- Questions: 48 hours

### Can I get phone support?

Phone support is available for enterprise plans. Contact your account manager to add phone support.

### Where can I find training resources?

- This documentation
- Video tutorials (check dashboard)
- Webinars and live training
- Community forum

## More Questions?

Can't find your answer here?

- Search this documentation
- Check the [Troubleshooting](/docs/user-guide/troubleshooting/) guide
- Review [Best Practices](/docs/user-guide/best-practices/)
- Contact support for help

We're here to help you succeed with Hay!
