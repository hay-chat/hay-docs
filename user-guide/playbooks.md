---
layout: docs.njk
title: Automation Playbooks
description: Playbooks are step-by-step workflows that teach your AI agent how to handle specific situations. Think of them as detailed instruction manuals for common scenarios.
section: user-guide
---

## What is a Playbook?

A playbook tells your agent:

- **When** to activate (the trigger)
- **What** to do (step-by-step instructions)
- **How** to respond (specific guidance)
- **When** to escalate (handoff conditions)

Unlike general agent instructions, playbooks are focused on specific scenarios and provide detailed guidance.

## When to Use Playbooks

### Perfect for Playbooks:

- ✅ Greeting new customers
- ✅ Processing refund requests
- ✅ Looking up order status
- ✅ Gathering information step-by-step
- ✅ Handling specific complaints
- ✅ Qualifying sales leads
- ✅ Scheduling appointments

### Not Needed for:

- ❌ General conversations
- ❌ Simple FAQ answers (use documents instead)
- ❌ One-off situations

## Creating a Playbook

### Step 1: Plan Your Playbook

Before you start, think about:

- What situation are you handling?
- What steps should the agent follow?
- What information needs to be collected?
- When should a human take over?

### Step 2: Create New Playbook

1. Click **Playbooks** in the sidebar
2. Click the **+** button to create a new playbook

> **Tip:** You can also use the AI **Generate Playbook** wizard, a separate flow accessible from the Playbooks list page via the **Generate** button. Describe the scenario in plain language, and the wizard will produce a draft you can review and edit.

### Step 3: Basic Information

**Title**

- Clear and descriptive
- Examples: "Welcome New Customers", "Process Refund Request", "Order Status Lookup"

**Trigger**

- A natural-language description of when this playbook should activate
- Describe the situation or intent in plain English — the AI uses this description to decide when to apply the playbook
- Examples:
  - "Activate when the customer sends a greeting or first message"
  - "Use when the customer mentions wanting a refund, return, or money back"
  - "Apply when the customer asks about the status or location of an order"

**Description** (Optional)

- What this playbook does
- When it should be used

### Step 4: Write Instructions

This is where the magic happens! The **Instructions** field is a rich-text editor (powered by Tiptap) that supports formatting such as bold, bullet lists, and headings. You can also use **@mention** to reference Actions (MCP tools) or Documents inline within the instructions.

**Example 1: Welcome Playbook**

```
Step 1: Greet the customer warmly
- Use their name if available
- Thank them for reaching out
- Let them know you're here to help

Step 2: Ask how you can help
- Keep it simple and open-ended
- Don't overwhelm with options
- Wait for their response

Step 3: Based on their response:
- If it's about orders → Use Order Lookup playbook
- If it's about products → Provide product information
- If it's unclear → Ask clarifying questions
```

**Example 2: Refund Request Playbook**

```
Step 1: Acknowledge the request
- Show empathy: "I understand you'd like a refund"
- Assure them you'll help

Step 2: Collect necessary information
- Order number (required)
- Reason for refund (optional but helpful)
- Check order exists in system

Step 3: Check refund eligibility
- Is order within refund window? (30 days)
- Is item refundable? (some items may be non-refundable)

Step 4: If eligible:
- Confirm refund details
- Explain process and timeline
- Provide refund confirmation

Step 5: If not eligible:
- Explain why politely
- Offer alternatives (store credit, exchange)
- If customer insists → Escalate to human

Step 6: Close
- Ask if they need anything else
- Thank them for their understanding
```

**Example 3: Information Gathering**

```
Collect the following information one question at a time:

1. Full name
   - Ask: "To help you, could I get your full name?"
   - Store response

2. Email address
   - Ask: "What email address do you use for your account?"
   - Validate format

3. Issue description
   - Ask: "Can you briefly describe what you need help with?"
   - Store response

Once all information collected:
- Summarize what they told you
- Ask for confirmation
- Proceed with assistance or escalate if needed
```

### Step 5: Set Status and Save

Status is set when you create the playbook:

**Draft** - Still working on it, not active yet
**Active** - Live and ready to handle conversations
**Archived** - No longer in use, hidden from active lists

Click **Save Changes**

> **Draft/Publish versioning:** Clicking **Save Changes** creates or updates a draft of the playbook. To make a version live, publish it — publishing sets the playbook's status to **Active** and creates an immutable snapshot of the playbook at that point in time. Subsequent edits start a new draft without affecting the published version. You can also set a playbook to **Archived** to remove it from active use.

## Playbook Instructions Best Practices

### Write Like You're Training a Person

Bad:

```
Handle refunds
```

Good:

```
When someone requests a refund:
1. Show empathy first
2. Get their order number
3. Check if the order qualifies
4. If yes, process it; if no, offer alternatives
```

### Be Specific About Decision Points

Bad:

```
Help the customer with their issue
```

Good:

```
If they mention shipping:
  → Look up order status
If they mention broken items:
  → Offer replacement or refund
If they mention size issues:
  → Suggest exchange or return
```

### Include Examples

Bad:

```
Greet the customer
```

Good:

```
Greet the customer warmly. For example:
- "Hi [Name]! Thanks for reaching out. How can I help you today?"
- "Hello! Welcome to [Company]. What brings you here?"
```

### Specify When to Escalate

Always include:

```
Escalate to human if:
- Customer is angry or frustrated
- Situation is outside refund policy
- Technical issue you can't solve
- Customer explicitly asks for human help
- You're not confident in the answer
```

## Assigning Playbooks to Agents

You can assign playbooks to specific agents from the playbook editor:

1. Open the playbook
2. In the right sidebar, find the **Assigned Agents** card
3. Use the checkboxes to select which agents should use this playbook
4. Save

**If no agents assigned:** The playbook is available to all agents.

## Managing Playbooks

### Editing a Playbook

1. Click on the playbook
2. Make your changes
3. Click **Save Changes**

Saving creates a new draft. Publish to make the updated version live.

### Testing a Playbook

Before activating:

1. Set playbook to **Draft**
2. Create a test conversation
3. Send messages that match the trigger scenario
4. See if agent follows the steps correctly
5. Adjust instructions as needed
6. Publish when ready

### Viewing Playbook Performance

> **Coming Soon:** Playbook-level analytics (usage count, success rate, completion time, escalation rate) are not yet available. Monitor playbook effectiveness by reviewing conversations manually in the **Conversations** section.

### Deactivating a Playbook

To stop a playbook from being used, set its status to **Archived**. Archived playbooks are hidden from active lists and will not be triggered by the agent.

## Common Playbook Templates

### 1. Welcome & Greeting

**Trigger:** Activate when the customer sends a greeting or initial message

**Instructions:**

```
1. Greet warmly
2. Introduce yourself
3. Ask how you can help
4. Wait for customer response before proceeding
```

### 2. Order Status Lookup

**Trigger:** Use when the customer asks about the location, status, or tracking of an order

**Instructions:**

```
1. Ask for order number
2. Look up order in system
3. Provide current status:
   - Processing: "Your order is being prepared"
   - Shipped: "Your order is on the way! Tracking: [number]"
   - Delivered: "Your order was delivered on [date]"
4. Ask if they need anything else
```

### 3. Product Recommendation

**Trigger:** Use when the customer is looking for a product recommendation or asks what to buy

**Instructions:**

```
1. Ask what they're looking for
2. Ask key questions:
   - What's their use case?
   - Any preferences (color, size, etc)?
   - Budget range?
3. Search our product catalog
4. Suggest 2-3 options with brief descriptions
5. Offer to answer questions about any product
```

### 4. Technical Troubleshooting

**Trigger:** Apply when the customer reports something not working, a broken feature, or an error

**Instructions:**

```
1. Express empathy for the frustration
2. Ask for specific details:
   - What exactly isn't working?
   - What happens when they try?
   - Any error messages?
3. Search knowledge base for solutions
4. Provide step-by-step fix if found
5. If no solution found → Escalate to technical support
```

### 5. Account Issues

**Trigger:** Use when the customer mentions login problems, a forgotten password, or a locked account

**Instructions:**

```
1. Identify the specific issue:
   - Forgot password → Send reset link
   - Account locked → Explain unlock process
   - Can't receive email → Check spam, update email
2. Guide through solution
3. Verify issue is resolved
4. If still not working → Escalate to account team
```

## Advanced Playbook Features

### Conditional Logic

Use IF/THEN statements:

```
IF customer says they're a business account:
  → Offer volume discount information
  → Suggest business account features
ELSE:
  → Provide standard pricing
```

### Required Fields

Specify information that MUST be collected:

```
Required fields:
- Order number (validate format: #12345)
- Email address (verify it's valid)
- Issue description (at least 10 characters)

Do not proceed until all required fields are collected.
```

### Multi-Step Workflows

Break complex processes into clear phases:

```
Phase 1: Information Gathering
- Collect customer details
- Understand the issue

Phase 2: Problem Analysis
- Search knowledge base
- Check for known issues

Phase 3: Solution Delivery
- Provide solution or workaround
- Verify customer understanding

Phase 4: Follow-up
- Ask if issue is resolved
- Offer additional help
- Close conversation gracefully
```

## Playbook Maintenance

### Review Regularly

Every 2-4 weeks:

1. Check which playbooks are used most
2. Read conversations using each playbook
3. Look for:
   - Confusing steps
   - Missing information
   - Unnecessary escalations
4. Update instructions

### Retire Unused Playbooks

If a playbook hasn't been used in 90 days:

- Consider if it's still needed
- Delete if obsolete
- Update triggers if it should be used more

### Version Control

Keep notes on changes:

- What you changed
- Why you changed it
- Date of change

This helps track what works and what doesn't.

## Common Questions

### How many playbooks should I have?

Start with 3-5 for your most common scenarios. Add more as needed, but don't overcomplicate.

### Can playbooks call other playbooks?

Yes! You can reference other playbooks in your instructions:

```
If customer needs refund → Use "Refund Request" playbook
```

### What happens if multiple playbooks match?

Hay chooses the best match based on trigger relevance and context.

### Can I use the same playbook across multiple channels?

Yes! Playbooks work across all channels unless you specify otherwise.

## Next Steps

- Create your first playbook with the [Quick Start](/docs/user-guide/quick-start/) guide
- Learn about [Agents](/docs/user-guide/agents/) to customize behavior
- Explore [Documents](/docs/user-guide/documents/) to give your agent knowledge
- Monitor results in [Analytics](/docs/user-guide/analytics/)
