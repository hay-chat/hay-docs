---
layout: docs.njk
title: AI Support Best Practices
description: Learn how to get the most out of Hay with these proven strategies and expert tips.
section: user-guide
---

## Getting Started Right

### Start Small, Scale Smart

**Week 1: Foundation**

- Create 1-2 agents maximum
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
- Add specialized agents if needed
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

## Agent Configuration

### Writing Great Instructions

**Use the "Training a Person" approach:**

Bad:

```
Help customers with orders.
```

Good:

```
You are a friendly customer support agent for [Company].

Your main responsibilities:
1. Answer questions about orders, shipping, and products
2. Look up order status when customers provide order numbers
3. Explain our return policy clearly and patiently
4. Escalate to humans when customers are frustrated

Tone: Friendly and professional
Always use the customer's name if you know it
If you're not 100% confident, say so and offer to connect them with a specialist
```

### Agent Specialization Strategy

**Option 1: Single Generalist Agent**
Best for:

- Small teams
- Simple product lines
- Under 100 conversations/day
- Similar types of questions

**Option 2: Multiple Specialized Agents**
Best for:

- Different departments (sales vs support)
- Multiple product lines
- High volume (500+ conversations/day)
- Distinct use cases

**Example specialized setup:**

_First-Line Support Agent_

- Handles: FAQs, basic troubleshooting
- Escalates: Technical bugs, billing issues
- Tone: Friendly and helpful

_Technical Support Agent_

- Handles: Complex technical issues, API questions
- Escalates: Bugs, feature requests
- Tone: Professional and detailed

_Sales Agent_

- Handles: Product questions, pricing, demos
- Escalates: Negotiation, custom deals
- Tone: Enthusiastic and consultative

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

Hay doesn't have a separate SLA-based escalation queue. Instead, escalated conversations live in the regular **Conversations** list, distinguished by status. (Note: the `/queue` page in the dashboard is unrelated — it tracks background job processing like document uploads, emails, and exports, not customer conversations.)

### Finding Conversations That Need You

Go to **Conversations** in the left sidebar and filter by **Status**:

| Status                                     | What It Means                    |
| ------------------------------------------- | --------------------------------- |
| **Pending Human** (shown as "Needs Attention") | Customer needs human help — nobody has picked it up yet |
| **Human Took Over** (shown as "Manual Control") | A team member is actively handling it |

Sort by **Last message** (oldest first) to see which conversations have been waiting longest.

**Set your own response time targets:**

| Urgency                | Target Response |
| ---------------------- | --------------- |
| 🔴 Angry/Frustrated    | 5 minutes       |
| 🟡 Standard Escalation | 30 minutes      |
| 🟢 Low Priority        | 4 hours         |

Since there's no built-in SLA tracking, these targets are something your team agrees on and self-monitors — check the "Needs Attention" filter regularly rather than relying on the dashboard to flag overdue items.

### Daily Workflow

**Morning routine (5 minutes):**

1. Find escalated conversations via the dashboard's **Attention Needed** widget, or navigate to `/conversations?status=pending-human`
2. Sort by oldest first and scan for urgent cases
3. Open one and click **Take Over** to start handling it
4. Respond within your target time

**Throughout day:**

- Re-check the "Needs Attention" filter every 30-60 minutes
- Take over new escalations promptly
- Use **Release Conversation → Return to Queue** if you need to hand a conversation to another team member (it goes back to "Needs Attention" for someone else to pick up)
- Keep customers informed

**End of day:**

- Review remaining "Needs Attention" conversations
- Leave conversations you're mid-way through in "Manual Control" so teammates know they're owned
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

## Analytics & Optimization

### Metrics That Matter

**Focus on these key metrics:**

1. **Resolution Rate**

   - Target: 70-85%
   - Higher = AI handling more independently
   - Review: Weekly

2. **Customer Satisfaction**

   - Target: 85%+
   - Review: Daily
   - **Note:** Customer satisfaction analytics are not yet wired up and currently show placeholder data. Track satisfaction manually until in-app tracking ships.

3. **Escalation Rate**

   - Target: 15-30%
   - Lower = more efficient AI
   - Review: Weekly

4. **Response Time**
   - Target: Under 5 seconds for AI
   - Should be consistent
   - Review: Weekly
   - **Note:** Response time analytics aren't available in the dashboard yet — the backend currently returns placeholder data. Treat this as an aspirational target you track manually until in-app reporting ships.

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
   - Update an agent
   - Create a playbook
   - Fix a recurring issue

4. **Track the change**
   - Note what you changed
   - Monitor impact over next week
   - Keep if it works, revert if it doesn't

### A/B Testing

> **Note:** Hay does not have built-in A/B testing or automatic traffic splitting. The approach below describes a manual testing workflow you can follow using multiple agents.

Test changes before rolling out everywhere:

**Example: Testing agent instructions (manual approach)**

1. **Current state:**

   - Agent A: Current instructions
   - Resolution rate: 75%

2. **Create test variant:**

   - Agent B: New instructions
   - Same documents, same playbooks

3. **Run for 1 week:**

   - Manually assign conversations to each agent to compare
   - Track metrics separately

4. **Compare results:**

   - Which has better resolution?
   - Which has higher satisfaction?
   - Are escalations different?

5. **Roll out winner:**
   - Apply winning approach
   - Measure continued performance

## Integration Strategy

### Start With One Channel

**First integration should be:**

- Your highest volume channel
- Where customers prefer to contact you
- Easiest to set up and test

**Examples:**

- E-commerce → Shopify + Web Chat
- SaaS → In-app chat + Email
- Local business → WhatsApp + Web Chat

### Multi-Channel Rollout

**Phase 1: Primary Channel (Week 1-2)**

- Set up and test thoroughly
- Train AI on common questions
- Monitor performance daily
- Achieve 70%+ resolution rate

**Phase 2: Secondary Channel (Week 3-4)**

- Add next channel
- Test with same agent
- Adjust tone if needed for channel
- Monitor cross-channel performance

**Phase 3: Expansion (Month 2+)**

- Add remaining channels
- Consider specialized agents per channel
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
- Review organization's agents
- Read key documentation
- Watch AI handle conversations

**Week 1: Supervised Practice**

- Use Supervision Mode (planned but not yet fully functional -- for now, review conversations manually via the dashboard)
- Review and approve responses
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
- More specialized agents
- Stricter quality control
- Team workflows matter more

**Key actions:**

1. **Better segmentation:**

   - Route by topic/complexity
   - Specialized agents
   - Priority queues

2. **Automation focus:**

   - Every repetitive task → playbook
   - Reduce human involvement
   - Faster escalation resolution

3. **Team structure:**
   - Dedicated queue managers
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

**Problem:** Too many agents, playbooks, features at once
**Solution:** Start simple, add complexity gradually

### Mistake 3: Set and Forget

**Problem:** No ongoing optimization
**Solution:** Weekly reviews and continuous improvement

### Mistake 4: Ignoring Analytics

**Problem:** Flying blind, missing opportunities
**Solution:** Data-driven decision making

### Mistake 5: No Human Backup

**Problem:** Escalations go unanswered
**Solution:** Clear queue management process

### Mistake 6: Testing in Production

**Problem:** Customers experience issues
**Solution:** Use playground/test mode extensively

### Mistake 7: Generic Agent Instructions

**Problem:** Vague, unhelpful responses
**Solution:** Specific, detailed, example-rich instructions

## Success Checklist

Ready to excel with Hay? Check these boxes:

**Foundation:**

- ☐ Clear, specific agent instructions
- ☐ 20+ core FAQ documents
- ☐ At least 2-3 essential playbooks
- ☐ Team trained on queue management
- ☐ Regular review schedule established

**Optimization:**

- ☐ 70%+ resolution rate
- ☐ 85%+ customer satisfaction
- ☐ Sub-30 minute escalation response
- ☐ Weekly analytics reviews
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
