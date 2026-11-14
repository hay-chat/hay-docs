---
layout: docs.njk
title: AI Support Best Practices
description: Learn how to get the most out of Hay with these proven strategies and expert tips.
section: user-guide
---

## Getting Started Right

### Start Small, Scale Smart

**Week 1: Foundation**

- Fill in the assistant's **Identity** and run the **Set up** board
- Upload 10-20 core FAQs
- Connect 1 primary channel
- Test thoroughly before going live

**Week 2-3: Expansion**

- Monitor and refine based on real conversations
- Add more documentation as gaps appear
- Create 2-3 essential playbooks
- Connect additional channels if needed

**Month 2+: Optimization**

- Analyze performance data
- Expand to edge cases
- Add playbooks for the situations that need their own steps
- Fine-tune based on patterns

**Why this works:**

- Learn the system gradually
- See what works before scaling
- Easier to troubleshoot
- Less overwhelming for team

### Set Realistic Expectations

**What Hay can do amazingly:**

- ✅ Answer FAQs instantly (80-90% resolution)
- ✅ Look up information from documents
- ✅ Follow structured workflows
- ✅ Handle high volumes without fatigue
- ✅ Work 24/7 in multiple languages

**What Hay needs help with:**

- ⚠️ Highly emotional situations
- ⚠️ Complex negotiations
- ⚠️ Unique edge cases not in documentation
- ⚠️ Situations requiring human judgment

## Assistant Configuration

### Writing Great Instructions

**Use the "Training a Person" approach:**

Bad:

```
Help customers with orders.
```

Good:

```
You are a friendly customer support assistant for [Company].

Your main responsibilities:
1. Answer questions about orders, shipping, and products
2. Look up order status when customers provide order numbers
3. Explain our return policy clearly and patiently
4. Escalate to humans when customers are frustrated

Tone: Friendly and professional
Always use the customer's name if you know it
If you're not 100% confident, say so and offer to connect them with a specialist
```

### One assistant, many playbooks

Hay has a single assistant per organization, so specialization happens in playbooks, not in separate agents. Keep the assistant's instructions for standing rules (tone, what to avoid, when to escalate) and put each situation in its own playbook:

- **Support:** order status, returns, refunds, basic troubleshooting
- **Sales:** product questions, recommendations, pricing
- **Technical:** error messages, how-to steps, bug reports

Each playbook has its own trigger, so the right one applies to each conversation. Different tone per department is not possible; pick one tone that fits your brand.

## Document Management

### The Golden Rules of Documentation

**1. Accuracy First**

- One wrong answer destroys trust
- Verify all information before uploading
- Update immediately when things change
- Regular audits (monthly minimum)

**2. Clarity Over Brevity**
Good:

```
Q: What is your return policy?
A: We accept returns within 30 days of purchase. Items must be unused and in original packaging. To start a return, email support@company.com with your order number. We'll send a prepaid return label within 24 hours. Refunds are processed within 5-7 business days after we receive the item.
```

Not great:

```
Returns: 30 days, unused, email us.
```

**3. Use Customer Language**
Write how customers speak:

- ❌ "What is the SKU lookup procedure?"
- ✅ "How do I find my product number?"

**4. Include Context**

- ❌ "Ships in 3-5 days"
- ✅ "Standard shipping delivers in 3-5 business days after your order is processed. Orders placed before 2 PM EST ship same day. Weekend orders ship Monday."

### Document Organization System

**Create a hierarchy:**

```
📁 Policies
  - Return Policy
  - Shipping Policy
  - Privacy Policy

📁 Products
  📁 Product Line A
    - Features and specs
    - Setup guide
    - Common issues
  📁 Product Line B
    - Features and specs
    - Setup guide

📁 FAQs
  📁 Orders
    - Order status
    - Tracking
    - Changes and cancellations
  📁 Shipping
    - Delivery times
    - International shipping
    - Costs and fees

📁 Troubleshooting
  - Common errors
  - Reset procedures
  - When to escalate
```

**Use consistent naming:**

- "How to [Action]" - Process guides
- "[Topic] FAQ" - Question collections
- "[Product] Guide" - Product documentation
- "[Process] Policy" - Official policies

### Document Maintenance Schedule

**Daily:**

- Add new FAQs from yesterday's conversations
- Quick updates for urgent changes

**Weekly:**

- Review most-used documents
- Check for outdated information
- Add missing details discovered in conversations

**Monthly:**

- Full content audit
- Remove or archive unused documents
- Consolidate duplicates
- Verify all information is current

**Quarterly:**

- Deep reorganization if needed
- Major content refresh
- Update screenshots/images
- Rewrite unclear sections

## Playbook Design

### Playbook Best Practices

**1. One Purpose Per Playbook**

Good:

- "Welcome New Customers"
- "Process Refund Request"
- "Handle Shipping Delays"

Not ideal:

- "Handle All Customer Issues" (too broad)
- "Welcome and Process Refunds" (too much in one)

**2. Clear Trigger Conditions**

Good triggers:

- "greeting", "hello", "hi" → Welcome
- "refund", "money back" → Refund Request
- "where is my order" → Order Tracking

Vague triggers:

- "help" (too generic)
- "stuff" (unclear)

**3. Step-by-Step Flow**

Good structure:

```
Step 1: Greet and acknowledge
Step 2: Collect order number
Step 3: Verify order exists
Step 4: If found → provide status
Step 5: If not found → escalate with collected info
```

**4. Define Escalation Points**

In every playbook, clearly state:

```
Escalate to human if:
- Customer is angry or frustrated
- Information isn't in our system
- Requires policy exception
- After 3 failed attempts to resolve
```

### Testing Playbooks

**Before activating:**

1. **Test the happy path:**

   - Everything goes right
   - Customer provides all info
   - Issue is resolved

2. **Test edge cases:**

   - Customer doesn't provide required info
   - Information not found in system
   - Customer changes their mind mid-flow

3. **Test escalation:**

   - Verify escalation triggers work
   - Check handoff is smooth
   - Ensure context is preserved

4. **Get team feedback:**
   - Have team members test
   - Collect their observations
   - Refine based on feedback

## Managing Escalated Conversations

Hay doesn't have SLA-based escalation tracking. Conversations that need a person appear in the **Inbox**. (Note: the `/queue` page in the dashboard is unrelated: it is a **Job Queue** for background processing such as document uploads, emails and exports, not customer conversations.)

### Finding Conversations That Need You

Open **Inbox** in the left sidebar. **Waiting for a human** lists conversations longest wait first, **You're handling** lists the ones you took over and **Teammates are handling** the rest. The sidebar badge shows how many are waiting. In **Conversations** the matching statuses are:

| Status                                     | What It Means                    |
| ------------------------------------------- | --------------------------------- |
| **Needs Attention** | Customer needs human help — nobody has picked it up yet |
| **Manual Control** | A team member is actively handling it |

**Set your own response time targets:**

| Urgency                | Target Response |
| ---------------------- | --------------- |
| 🔴 Angry/Frustrated    | 5 minutes       |
| 🟡 Standard Escalation | 30 minutes      |
| 🟢 Low Priority        | 4 hours         |

Since there's no built-in SLA tracking, these targets are something your team agrees on and self-monitors. Turn on **Alerts on this device** so you hear about new hand-offs; Hay alerts again after 10 minutes and then emails available team members.

### Daily Workflow

**Morning routine (5 minutes):**

1. Open the **Inbox**
2. Scan **Waiting for a human** for urgent cases and answer any **Questions for the team**
3. Click **Take over** on one to start handling it
4. Respond within your target time

**Throughout day:**

- Keep the Inbox badge in view and take over new escalations promptly
- Use **Release Conversation → Return to Queue** if you need to hand a conversation to another team member (it goes back to **Waiting for a human** for someone else to pick up)
- Keep customers informed

**End of day:**

- Review what is still under **Waiting for a human**
- Leave conversations you're mid-way through in **You're handling** so teammates know they're owned
- Note follow-ups needed
- Set reminders for tomorrow

### Preventing Escalation Buildup

**Proactive prevention:**

1. **Improve AI training:**

   - Analyze why conversations escalate
   - Add documents for common escalation topics
   - Create playbooks for repetitive escalations

2. **Set better triggers:**

   - Adjust escalation sensitivity
   - Allow AI to handle more edge cases
   - Only escalate what truly needs human help

3. **Empower AI:**

   - Give clearer instructions
   - Provide more examples
   - Build confidence through documentation

4. **Staff appropriately:**
   - Know your peak times
   - Have coverage during busy periods
   - Consider time zones for global support

## Measuring and Optimizing

### Metrics That Matter

**Focus on these key metrics:**

1. **Resolution Rate**

   - Target: 70-85%
   - Higher = AI handling more independently
   - Review: Weekly

2. **Customer feedback**

   - Target: 85%+ positive
   - **Quality** → **Feedback** shows your team's good/bad ratings of AI replies and a **Positive Rate**. If you enable CSAT in the Webchat settings, web chat visitors can also rate a finished conversation
   - Review: Weekly

3. **Escalation Rate**

   - Target: 15-30%
   - Lower = more efficient AI
   - Review: Weekly
   - **Note:** Hay doesn't compute an escalation rate. The Dashboard's **Human escalations** card shows takeovers for today and this week; compare them with **Total Conversations**.

4. **Response Time**
   - Target: Under 5 seconds for AI
   - Should be consistent
   - Review: Weekly
   - **Note:** The Dashboard's **Avg Response Time** card shows the average and the median.

### Weekly Review Ritual

**Every Monday, spend 30 minutes:**

1. **Review last week's metrics**

   - What improved?
   - What declined?
   - Any unusual patterns?

2. **Read 10 conversations**

   - 5 high-rated (learn what works)
   - 5 low-rated (learn what doesn't)

3. **Identify one improvement**

   - Add a document
   - Update the assistant's instructions
   - Create a playbook
   - Fix a recurring issue

4. **Track the change**
   - Note what you changed
   - Monitor impact over next week
   - Keep if it works, revert if it doesn't

### Testing changes before you roll them out

There is one assistant, so there is no traffic split between variants. Test a change against real history instead:

1. Edit a playbook: your changes are saved as a **draft** and do not reach customers until you **Publish**
2. In the playbook's **More actions** menu, choose **Test this draft** to try it in a test conversation, or **Replay past conversations** to see how it answers your closed conversations
3. Save good conversations as **test cases**; before you publish, the test cases run against the draft and warn you about failures
4. Publish when the report looks right, then watch **Quality** → **Insights** to see whether the problem shows as **Fixed**

See [Tests](/docs/user-guide/tests/).

### Multi-Channel Rollout

**Phase 1: Primary Channel (Week 1-2)**

- Set up and test thoroughly
- Train AI on common questions
- Monitor performance daily
- Achieve 70%+ resolution rate

**Phase 2: Secondary Channel (Week 3-4)**

- Add next channel
- Test with the same assistant
- Adjust tone if needed for channel
- Monitor cross-channel performance

**Phase 3: Expansion (Month 2+)**

- Add remaining channels
- Adjust playbooks for channel-specific needs
- Optimize for channel-specific needs

### Channel-Specific Optimization

**Web Chat:**

- Quick, concise responses
- Use formatting (bold, lists)
- Proactive engagement
- Business hours focus

**WhatsApp:**

- Very brief messages
- Use emojis appropriately
- Quick replies preferred
- 24/7 availability

**Email:**

- More detailed, formal
- Proper formatting
- Complete information first time
- Professional tone

## Team Training

### Onboarding New Team Members

**Day 1: Introduction**

- Tour of dashboard
- Review the assistant's instructions and playbooks
- Read key documentation
- Watch AI handle conversations

**Week 1: Supervised Practice**

- Turn on **Test Mode Default** (Require approval for AI messages) under **Settings** → **Workspace** → **General**
- Review and approve responses before they reach customers
- Provide feedback
- Share best practices

**Week 2: Independent Work**

- Handle escalations independently
- Regular check-ins
- Review their conversations
- Answer questions

**Ongoing: Continuous Improvement**

- Weekly team reviews
- Share interesting cases
- Discuss improvement ideas
- Celebrate wins

### Creating a Knowledge Culture

**Make documentation everyone's job:**

1. **"See something, document something"**

   - Found a gap in docs? Fill it.
   - Customer asked new question? Add the answer.
   - Process changed? Update immediately.

2. **Weekly knowledge sharing:**

   - Team meeting to discuss learnings
   - Share interesting conversations
   - Update docs together
   - Celebrate knowledge contributions

3. **Reward good documentation:**
   - Recognize team members who improve docs
   - Track documentation contributions
   - Make it part of performance reviews

## Security & Privacy

### Protecting Customer Data

**Do:**

- ✅ Use strong passwords and 2FA
- ✅ Limit access to what each person needs
- ✅ Review user access quarterly
- ✅ Log out when leaving computer
- ✅ Use secure connections (HTTPS)

**Don't:**

- ❌ Share login credentials
- ❌ Access Hay from public computers
- ❌ Leave sensitive data visible on screen
- ❌ Email customer data unencrypted
- ❌ Take screenshots with customer PII

### API Token Management

**Best practices:**

- Create specific tokens for each integration
- Name tokens clearly ("Shopify Prod", "Test API")
- Rotate tokens quarterly
- Delete unused tokens immediately
- Never commit tokens to code
- Store tokens in secure vault

### Compliance Maintenance

**Regular compliance checklist:**

Monthly:

- ☐ Review data retention settings
- ☐ Process any privacy requests
- ☐ Check for unauthorized access
- ☐ Verify team member access is appropriate

Quarterly:

- ☐ Full security audit
- ☐ Update privacy documentation
- ☐ Review compliance with regulations
- ☐ Test data export/deletion procedures

## Scaling Tips

### From 100 to 1,000 Conversations/Day

**What changes:**

- Need better organization
- More playbooks for specific situations
- Stricter quality control
- Team workflows matter more

**Key actions:**

1. **Better segmentation:**

   - Route by topic/complexity
   - Playbooks per topic
   - Clear Inbox ownership

2. **Automation focus:**

   - Every repetitive task → playbook
   - Reduce human involvement
   - Faster escalation resolution

3. **Team structure:**
   - Dedicated Inbox owners
   - Specialists for complex issues
   - Clear escalation paths

### From 1,000 to 10,000+ Conversations/Day

**Enterprise considerations:**

- Multiple teams
- Advanced routing
- Custom integrations
- Dedicated account management

**Contact Hay for:**

- Enterprise pricing
- Dedicated support
- Custom features
- Training and consulting

## Common Mistakes to Avoid

### Mistake 1: Insufficient Documentation

**Problem:** AI has nothing to reference
**Solution:** Start with comprehensive FAQs

### Mistake 2: Over-complicating Too Soon

**Problem:** Too many playbooks, channels and features at once
**Solution:** Start simple, add complexity gradually

### Mistake 3: Set and Forget

**Problem:** No ongoing optimization
**Solution:** Weekly reviews and continuous improvement

### Mistake 4: Ignoring the Data

**Problem:** Flying blind, missing opportunities
**Solution:** Data-driven decision making

### Mistake 5: No Human Backup

**Problem:** Escalations go unanswered
**Solution:** Clear Inbox ownership and alerts turned on

### Mistake 6: Testing in Production

**Problem:** Customers experience issues
**Solution:** Use the Conversation Playground, Tests and Test Mode extensively

### Mistake 7: Generic Assistant Instructions

**Problem:** Vague, unhelpful responses
**Solution:** Specific, detailed, example-rich instructions

## Success Checklist

Ready to excel with Hay? Check these boxes:

**Foundation:**

- ☐ Clear, specific assistant instructions
- ☐ 20+ core FAQ documents
- ☐ At least 2-3 essential playbooks
- ☐ Team trained on the Inbox
- ☐ Regular review schedule established

**Optimization:**

- ☐ 70%+ resolution rate
- ☐ 85%+ positive feedback
- ☐ Sub-30 minute escalation response
- ☐ Weekly Dashboard and Insights reviews
- ☐ Continuous documentation updates

**Excellence:**

- ☐ 85%+ resolution rate
- ☐ 90%+ customer satisfaction
- ☐ Sub-15 minute escalation response
- ☐ Proactive improvement culture
- ☐ Knowledge sharing system

## Next Steps

- Implement these best practices gradually
- Track your improvements
- Share what works with your team
- Join the Hay community to learn from others

Remember: Great customer support is a journey, not a destination. Keep learning, keep improving, and keep delighting your customers!
