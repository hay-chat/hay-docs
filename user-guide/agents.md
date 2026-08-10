---
layout: docs.njk
title: AI Agents Configuration
description: Agents are your AI assistants. Think of each agent as a team member with a specific personality, expertise, and way of handling customer conversations.
section: user-guide
---

## What is an Agent?

An agent is a configured AI assistant that:

- Has a specific **name** and **purpose**
- Follows **instructions** you provide
- Speaks in a particular **tone** or **style**
- Handles conversations for specific **channels** or **topics**
- Uses your **documents** and **playbooks** to answer questions

You can have multiple agents, each specialized for different needs.

## Creating an Agent

### Step 1: Navigate to Agents

1. Expand **Settings** in the left sidebar, then click **Agents**
2. Click **Create Agent** button

### Step 2: Basic Information

Fill in the essentials:

**Name**

- Something descriptive: "Customer Support Agent", "Sales Assistant", "Technical Support Bot"
- Visible to your team and also shown to customers in the web chat widget

**Description** (Optional but recommended)

- What does this agent handle?
- Example: "Handles order questions, shipping inquiries, and basic product information"

### Step 3: Configure Behavior

**Instructions**

Tell your agent how to behave. Write in plain English:

```
You are a helpful customer support agent for an online store.

Your job is to:
- Answer questions about orders, shipping, and products
- Be friendly and patient
- Always check our documentation before answering
- Escalate to a human if you're not confident

Important guidelines:
- Never make promises about shipping dates
- Always verify order numbers before looking them up
- If someone is frustrated, offer to connect them with a manager
```

**Tone**

Choose how your agent should communicate:

| Tone               | When to Use                    | Example                                                                      |
| ------------------ | ------------------------------ | ---------------------------------------------------------------------------- |
| **Professional**   | B2B, enterprise customers      | "Thank you for contacting us. I'd be happy to assist you with that inquiry." |
| **Casual**         | Young audience, informal brand | "Hey! No worries, I got you. Let's figure this out together."                |
| **Enthusiastic**   | Energetic, upbeat brand voice  | "Awesome, let's get this sorted for you right away!"                         |

**Language**

Select the language your agent should respond in. Hay supports 33 languages, allowing you to serve customers in their preferred language.

**Things to Avoid** (Optional)

List topics or phrases your agent should never use:

```
- Don't promise specific dates without checking systems
- Avoid technical jargon with non-technical customers
- Never say "that's not possible" without offering alternatives
- Don't make pricing decisions without approval
```

### Step 4: Initial Greeting Message

Set a welcoming first message customers see:

**Examples:**

_E-commerce:_

```
Hi! Welcome to [Store Name]! I'm here to help with orders,
products, and shipping. What can I help you with today?
```

_SaaS Support:_

```
Hello! I'm your [Product Name] assistant. I can help with
account questions, troubleshooting, or feature guidance.
How can I assist you?
```

_General Support:_

```
Hi there! Thanks for reaching out. I'm here to help!
What brings you in today?
```

### Step 5: Enable and Save

- Toggle **Enabled** to activate your agent
- Click **Create Agent**

Your agent is now ready to start handling conversations!

## Editing an Existing Agent

Need to make changes? Easy!

1. Go to **Agents** in the sidebar
2. Click on the agent you want to edit
3. Make your changes
4. Click **Save Changes**

**Changes take effect immediately** for new conversations.

## Agent Settings

### Message Approval

Control whether the agent's responses are sent automatically or require human review before delivery.

**Options:**

- **Inherit from Organization** - Uses the organization-level default setting
- **Require Approval** - Agent generates responses but a team member must approve each one before it is sent to the customer; ideal for quality assurance and training
- **Auto-Send** - Agent sends responses immediately without manual review

**How to configure:**

1. Open your agent settings
2. Under **Message Approval**, select the desired option
3. Changes take effect for all new conversations with this agent

### Human Escalation

Tell your agent exactly how to handle escalations:

> **Note:** The human handoff instructions field uses a rich text editor, so you can format your instructions with headings, lists, bold/italic text, and other formatting options rather than plain text.

**When Human is Available:**

```
Let the customer know that you'll connect them with a specialist
who can help. Transfer the conversation and provide a summary of
what was discussed.
```

**When Human is NOT Available:**

```
Apologize and let the customer know you'll have someone reach out
within 2 business hours. Collect their email if you don't have it.
Ask if there's anything else you can help with while they wait.
```

### Trigger Conditions

The **Trigger** field is a free-text description of when this agent should be selected for a conversation. Rather than matching exact keywords, Hay uses an LLM to semantically compare the incoming message against each enabled agent's trigger (along with its name and description) and picks the best match.

Write it like you're briefing a teammate on when to step in:

```
Handles order questions, shipping inquiries, and delivery tracking.
Also covers returns and refund requests.
```

```
Handles bug reports, error messages, and anything related to the
product not working as expected.
```

```
Handles pricing questions, plan comparisons, and upgrade/downgrade requests.
```

Since matching is semantic, you don't need to enumerate every possible keyword — a clear, descriptive sentence works better than a long list of exact phrases.

> **Note:** Trigger conditions currently don't support scheduling (e.g., business hours vs. after hours). This is a potential future feature.

Assigning an agent to specific channels (WhatsApp, website chat, email, etc.) is configured separately in the **Channels** section of the agent form, not through the trigger field.

### Default Agent

Every organization needs a fallback for conversations that don't clearly match any agent's trigger. Mark one agent as the **Default Agent** and Hay will assign it to those conversations automatically.

**To set an agent as default:**

1. Open the agent you want to use as the fallback
2. Click **Set as Default**

Only one agent can be the default at a time — setting a new one automatically replaces the previous default.

## Multiple Agents Strategy

### When to Use Multiple Agents

**Good reasons:**

- Different expertise areas (sales vs support)
- Different channels (WhatsApp vs email)
- Different languages or regions
- Different customer tiers (free vs premium)

**Not necessary for:**

- Slight tone differences
- Similar topics
- Same channel

### Agent Specialization Examples

**Example 1: E-commerce Store**

_Support Agent_

- Handles: Order questions, shipping, returns
- Tone: Friendly and helpful
- Channels: All

_Sales Agent_

- Handles: Product questions, recommendations
- Tone: Enthusiastic and knowledgeable
- Channels: Website chat only

**Example 2: SaaS Company**

_First-line Support_

- Handles: Account questions, basic troubleshooting
- Escalates: Technical bugs, billing issues
- Channels: In-app chat, email

_Technical Support_

- Handles: Complex technical issues, API questions
- Tone: Professional and detailed
- Channels: Email, priority queue

## Agent Performance

### Monitoring Your Agents

Agent performance metrics are shown on the main **Analytics** page, which provides an overview of conversation volume, resolution rates, and agent activity.

> **Note:** Agent performance analytics currently display sample data while this feature is being developed. Live per-agent metrics are coming soon.

### Improving Agent Performance

**If resolution rate is low:**

1. Review conversations the agent struggled with
2. Add more training documents on those topics
3. Update agent instructions with examples
4. Create specific playbooks for common issues

**If escalation rate is high:**

1. Check why conversations are escalating
2. Give agent more confidence in instructions
3. Add "decision trees" in playbooks
4. Expand knowledge base coverage

**If responses are too slow:**

1. Simplify agent instructions
2. Organize documents better
3. Reduce unnecessary playbook steps

**If customer ratings are low:**

1. Review exact conversations with poor ratings
2. Adjust tone settings
3. Add more empathy to instructions
4. Train on better response examples

## Best Practices

### Writing Great Instructions

**Do:**

- ✅ Be specific and clear
- ✅ Use examples
- ✅ Include what TO do, not just what NOT to do
- ✅ Think like you're training a new employee

**Don't:**

- ❌ Be vague ("be helpful")
- ❌ Use technical jargon
- ❌ Write a novel (keep it under 500 words)
- ❌ Contradict yourself

### Starting Simple

For your first agent:

1. Start with basic, clear instructions
2. Test in playground mode
3. Handle one main purpose well
4. Expand gradually based on real conversations

### Regular Maintenance

Set a reminder to review your agents:

**Weekly:**

- Check performance metrics
- Review a few conversations
- Update instructions if needed

**Monthly:**

- Deep dive into problem areas
- Add new capabilities
- Retire unused agents

## Common Questions

### Can I have multiple agents responding to the same customer?

No, Hay assigns one agent per conversation to maintain consistency. You can manually reassign if needed.

### What happens if I disable an agent?

New conversations won't be assigned to this agent. Existing conversations continue with the same agent.

### Can agents learn on their own?

Agents improve based on the documents you add and playbooks you create. They don't automatically "learn" without your guidance.

### How many agents should I have?

Most businesses start with 1-2 agents. You only need more if you have truly different use cases or channels.

### Can I clone an agent?

Agent cloning is not currently implemented. To reuse a configuration, create a new agent and manually copy the instructions and settings from an existing one.

## Next Steps

- Set up [Playbooks](/docs/user-guide/playbooks/) to give your agent step-by-step workflows
- Upload [Documents](/docs/user-guide/documents/) to train your agent
- Review [Conversations](/docs/user-guide/conversations/) to see your agent in action
