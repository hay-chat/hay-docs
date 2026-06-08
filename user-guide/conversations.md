---
layout: docs.njk
title: Managing Conversations
description: Every interaction with your customers becomes a conversation in Hay. Learn how to view, manage, and take control of customer conversations.
section: user-guide
---

## What is a Conversation?

A conversation is a complete interaction between a customer and your Hay agent. It includes:

- All messages exchanged
- Customer information
- Which agent handled it
- How it was resolved
- Any feedback or ratings

## Viewing Conversations

### Conversations List

Go to **Conversations** in the left sidebar to see all your interactions.

**What you'll see:**

- **Conversation** - Title and conversation ID
- **Status** - Current state (Open, Resolved, Closed, etc.)
- **Assigned To** - Which agent or team member is handling it
- **Duration** - How long the conversation has been active
- **Satisfaction** - Customer satisfaction rating (if provided)
- **Updated** - When the last activity happened

### Searching and Filtering

Find specific conversations quickly:

**Search by:**

- Conversation title
- Conversation ID

**Filter by:**

- **Status** - Active, Resolved, Escalated, Closed
- **Timeframe** - Today, This Week, This Month, All Time

## Conversation Statuses

Understanding what each status means:

| Status              | What It Means                            | What Happens                 |
| ------------------- | ---------------------------------------- | ---------------------------- |
| **Open**            | AI is actively handling the conversation | Agent responds automatically |
| **Processing**      | AI is thinking and preparing a response  | Wait a few seconds           |
| **Pending Human**   | Customer needs human help                | Appears in your Queue        |
| **Human Took Over** | A team member is now handling it         | AI stops responding          |
| **Resolved**        | Issue was solved successfully            | Conversation is complete     |
| **Closed**          | Conversation ended                       | No further action needed     |

## Opening a Conversation

Click any conversation to see the full details:

### Conversation Thread

The main area shows all messages in chronological order:

- **Customer messages** appear on one side
- **Agent responses** appear on the other
- **System messages** (like "Agent joined") in the middle
- **Timestamps** on each message

### Message Features

For each AI response, you can see:

- ✅ **Confidence score** - How sure the AI was about its answer
- 📚 **Sources** - Which documents were used to generate the response
- ⏱️ **Response time** - How long it took to generate
- 👍 👎 **Feedback** - Rate the quality of the response

### Conversation Info Panel

On the right side, view:

- **Customer details** - Name, email, history
- **Conversation metadata** - Channel, start time, duration
- **Agent info** - Which agent handled this
- **Playbook used** - If any workflow was triggered
- **Tags** - Categorize conversations for later analysis

## Taking Over a Conversation

Sometimes you need to step in and handle a conversation yourself.

### When to Take Over

- Customer specifically requests a human
- Issue is too complex for AI
- Situation requires empathy or judgment
- Agent isn't providing good answers

### How to Take Over

1. Open the conversation
2. Click **Take Over** button at the top
3. Start typing your messages
4. The AI will stop responding automatically

> **What happens when you take over:**
>
> - Status changes to "Human Took Over"
> - AI stops generating responses
> - You can chat directly with the customer
> - All your messages are logged

### Exiting Takeover

When you're done:

1. Click **Release Conversation**
2. Choose **Return to AI** to let the agent resume handling messages
3. Or choose **Return to Queue** to make the conversation available for other team members

## Supervision Mode (Coming Soon)

> **Coming Soon:** Supervision Mode is planned but not yet available. This section describes the intended functionality.

### What is Supervision Mode?

- Watch the conversation in real-time
- See what the AI is about to say BEFORE it sends
- Approve or edit responses
- Step in only if needed

### How It Will Work

1. Open an active conversation
2. Click **Supervise** at the top
3. When AI generates a response, you'll see it first
4. **Approve** to send it, **Edit** to change it, or **Take Over** to respond yourself

**Perfect for:**

- Training new agents
- High-value customers
- Sensitive topics
- Testing and quality control

## Test Conversations (Playground)

Practice and test your agent without affecting real customers.

### Starting a Test

1. Go to **Conversations**
2. Click **New Test Conversation** or enter **Playground mode**
3. Chat with your agent like a customer would

### What's Different in Test Mode

- ⚠️ Marked clearly as "Test"
- Messages send automatically (no waiting)
- Doesn't count in your analytics
- Perfect for trying different questions

**Use playground to:**

- Test new playbooks
- Try edge cases
- Train team members
- Show demos to stakeholders

## Conversation Actions

### Export Conversation

Save a complete record:

1. Click **Export** button
2. Choose format (PDF or CSV)
3. Download includes all messages, timestamps, and metadata

**Perfect for:**

- Sharing with team members
- Record keeping
- Training materials
- Compliance documentation

## Managing Customer Messages

### Messages You'll See

**Customer Types:**

- 💬 Regular message
- 📎 File or image attachment
- ⭐ Feedback or rating
- 🔄 Follow-up question

**Agent Types:**

- 🤖 AI response
- 👤 Human agent message
- ℹ️ System notification
- ⚠️ Error or escalation notice

### Handling Attachments

When customers send files:

1. Click the attachment to view
2. Download if needed
3. AI can read text in images and PDFs
4. Use context to provide better answers

## Best Practices

### Reviewing Conversations

Make it a habit to review conversations to improve your agent:

**Look for:**

- ✅ Great responses (what worked well?)
- ❌ Poor responses (what needs improvement?)
- 🔁 Repeated questions (add to your docs!)
- 😟 Frustrated customers (why did it escalate?)

### Responding to Feedback

When customers rate messages with 👍 or 👎:

- Read those conversations carefully
- Update documents if information was wrong
- Adjust playbooks if flow was confusing
- Add more examples for unclear topics

### Following Up

Even after AI resolves an issue:

- Check resolved conversations periodically
- Reach out to customers who seemed unsatisfied
- Turn common questions into documentation
- Celebrate wins when AI nails it!

## Common Questions

### Can I delete conversations?

Conversations are archived rather than deleted for compliance and learning purposes. Contact your administrator if you need to remove sensitive data.

### How long are conversations stored?

Based on your organization's settings. Typically conversations are kept for analysis and improvement.

### Can customers reopen closed conversations?

Yes! If a customer messages again on the same channel, Hay can either continue the previous conversation or start a new one.

### What if two agents respond to the same conversation?

Hay prevents this with a "locking" system. Only one agent (AI or human) can respond at a time.

## Next Steps

- Learn about [Agents](/docs/user-guide/agents/) to handle escalations
- Explore [Agents](/docs/user-guide/agents/) to customize your AI assistant
- Check out [Playbooks](/docs/user-guide/playbooks/) to automate common workflows
