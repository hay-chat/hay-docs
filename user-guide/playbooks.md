---
layout: docs.njk
title: Automation Playbooks
description: Playbooks are step-by-step workflows that teach your AI assistant how to handle specific situations. Think of them as detailed instruction manuals for common scenarios.
section: user-guide
---

## What is a Playbook?

A playbook tells your assistant:

- **When** to activate (the trigger)
- **What** to do (step-by-step instructions)
- **How** to respond (specific guidance)
- **When** to escalate (handoff conditions)

Unlike the assistant's general instructions, playbooks are focused on specific scenarios and provide detailed guidance.

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
- What steps should the assistant follow?
- What information needs to be collected?
- When should a human take over?

### Step 2: Create New Playbook

1. Click **Playbooks** in the sidebar (under **Knowledge**)
2. Click the **+** button to create a new playbook

> **Tip:** Two faster ways to start. **From template** (a menu next to the **+** button) creates a draft playbook from a ready-made template for you to review and activate. **Generate Playbook** (with the sparkles icon) opens the AI wizard, which walks you through five steps (Purpose, Actions, Documents, Boundaries, Generate) and produces a draft you can review and edit. The **Set up** board can also write playbooks for you; see [Set Up Your Assistant](/docs/user-guide/setup/).

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
**Archived** - No longer in use

Click **Create Playbook**.

> **What's live vs. what's versioned:** Identity fields — **Title**, **Trigger** and **Description** — save immediately when you click **Save Changes** and take effect right away, no publishing required. Only the **Instructions** content goes through draft/publish versioning: edits to Instructions auto-save to a hidden draft ("All changes saved"), and the assistant keeps using the last published version until you click **Publish**, which sets the playbook's status to **Active** and creates a snapshot of the Instructions at that point in time. You can add an optional note about what changed. Open **Version History** (in the **More actions** menu) to browse previous versions, **Restore** an earlier one, or **Discard** a draft to revert to the live version. **Compare with active** shows what changed against the live version.
>
> Playbooks can also approve actions: see [Approval for one playbook](/docs/user-guide/action-approvals/#approval-for-one-playbook).

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

## One assistant, all playbooks

Playbooks are not assigned to agents: Hay has one assistant, and every active playbook is evaluated for every conversation. The orchestrator scores the active playbooks against the conversation context and applies the best match.

## Managing Playbooks

### Editing a Playbook

1. Click on the playbook
2. Make your changes
3. Click **Save Changes**

**Save Changes** applies immediately to Title, Trigger and Description — no publishing needed. If you also edited the Instructions, those changes auto-save to a draft and won't affect live conversations until you click **Publish**.

### Testing a Playbook

A playbook must be **Active** (published) to be evaluated by the assistant — Draft playbooks are not matched to conversations. Note that this only concerns the Instructions version: changes to the Trigger, Title, and Description take effect immediately on **Save Changes**, so an active playbook can start (or stop) matching conversations right away even without a publish. You can test a draft before it goes live:

1. Edit the playbook so there is a draft, then open the **More actions** menu (the **...** button in the editor header)
2. Choose **Test this draft** to start a test conversation that uses the draft, not the published version, or **Replay past conversations** to see how the draft answers your closed conversations
3. Send messages that match the trigger scenario and see if the assistant follows the steps correctly
4. Adjust the instructions as needed, then **Publish**

If the playbook has saved test cases, they run against the draft when you publish and you are warned about failures. See [Tests](/docs/user-guide/tests/).

**Improve with AI.** The same **More actions** menu has **Improve with AI**: describe what is wrong and the AI proposes a revised version of the instructions, shown as added, modified and removed changes. Click **Apply to draft** to accept it. From a conversation you can use **Improve playbook** for the same thing.

### Viewing Playbook Performance

> **Coming Soon:** Playbook-level analytics (usage count, success rate, completion time, escalation rate) are not yet available. Monitor playbook effectiveness by reviewing conversations manually in the **Conversations** section.

### Archiving, duplicating and deleting

On the **Playbooks** list, each row has a **More actions** menu with **Edit**, **Duplicate** (creates "<name> (copy)"), **Archive** and, for archived playbooks, **Restore as draft**. **Delete Playbook** is in the **More actions** menu in the editor and cannot be undone. An archived playbook is not used by the assistant. The list shows **Total Playbooks**, **Active** and **Total Triggers** cards, and can be filtered by category and status.

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
- Set up your [AI assistant](/docs/user-guide/agents/) to customize behavior
- Explore [Documents](/docs/user-guide/documents/) to give your assistant knowledge
- Monitor results in [Analytics](/docs/user-guide/analytics/)
