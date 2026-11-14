---
layout: docs.njk
title: Managing Conversations
description: Every interaction with your customers becomes a conversation in Hay. Learn how to view, manage, and take control of customer conversations.
section: user-guide
---

## What is a Conversation?

A conversation is a complete interaction between a customer and your Hay assistant. It includes:

- All messages exchanged
- Customer information
- Whether the AI or a team member handled it
- How it was resolved
- Any feedback or ratings

## Viewing Conversations

### Conversations List

Go to **Conversations** in the left sidebar to see all your interactions. Three cards at the top show **Total Conversations** (with how many started today), **Active Now** and **Avg Response Time** (last 7 days). **Conversation Playground** and **Refresh** are in the header; **Select** lets you pick several conversations and **Archive** them.

To see only conversations that need a person, use the [Inbox](/docs/user-guide/inbox/).

**What you'll see:**

- **Conversation** - Title and truncated ID
- **Channel** - Where the conversation came from (shown when multiple channels are active)
- **Status** - Current state (Open, Processing, Awaiting Approval, Needs Attention, Manual Control, Resolved, Closed)
- **Assigned To** - Human agent, if the conversation has been taken over
- **Duration** - How long the conversation has been active
- **Satisfaction** - Customer satisfaction rating ("Not rated" when the customer did not give one)
- **Updated** - When the last activity happened

### Searching and Filtering

Find specific conversations quickly:

**Search by:**

- Conversation title
- Conversation ID

> **Note:** Search currently matches conversation title and ID only. Searching by customer name/email or message content is not yet implemented.

**Filter by:**

- **Status** - All Status, Active, Resolved, Unresolved, Escalated, Closed
- **Timeframe** - Today, This Week, This Month, All Time (preset options)

> **Note:** The timeframe filter is not yet functional — selecting a timeframe currently has no effect. A channel filter is planned but not yet available.

> **Note:** The "Active", "Unresolved" and "Escalated" status filters don't currently match any real conversation status. The filter does a strict equality check against actual status values (`open`, `processing`, `pending-human`, `human-took-over`, `resolved`, `closed`), so selecting "Active" or "Escalated" may return no results.

## Conversation Statuses

Understanding what each status means:

| Status              | What It Means                            | What Happens                 |
| ------------------- | ---------------------------------------- | ---------------------------- |
| **Open**            | AI is actively handling the conversation | Assistant responds automatically |
| **Processing**      | AI is thinking and preparing a response  | Wait a few seconds           |
| **Awaiting Approval** | The AI asked to run an action that needs a person's yes | See [Action approvals](/docs/user-guide/action-approvals/) |
| **Needs Attention** | Customer needs human help | Appears in your [Inbox](/docs/user-guide/inbox/) under **Waiting for a human** |
| **Manual Control** | A team member is now handling it | AI stops responding          |
| **Resolved**        | Issue was solved successfully            | Conversation is complete     |
| **Closed**          | Conversation ended                       | No further action needed     |

## Opening a Conversation

Click any conversation to see the full details:

### Conversation Thread

The main area shows all messages in chronological order:

- **Customer messages** appear on one side
- **Assistant responses** appear on the other
- **System messages** (like "Agent joined") in the middle
- **Timestamps** on each message

### Message Features

For each AI response, you can see:

- 👍 👎 **Feedback** - Rate the quality of the response

> **Webchat visitors see when you've read their message.** Opening a webchat conversation in the inbox shows the visitor **Seen** under their last message. Only a person opening the conversation does this; the AI never does.

### Conversation Info Panel

On the right side, view:

- **Customer details** - Name, email, history
- **Conversation metadata** - Channel, start time, duration
- **Related documents** and **Previous conversations** with this customer

## Taking Over a Conversation

Sometimes you need to step in and handle a conversation yourself.

### When to Take Over

- Customer specifically requests a human
- Issue is too complex for AI
- Situation requires empathy or judgment
- The assistant isn't providing good answers

### How to Take Over

From the [Inbox](/docs/user-guide/inbox/), click **Take over** on any conversation in the **Waiting for a human** list. The conversation opens with you in control.

From a conversation:

1. Open the conversation
2. Click the **Take Over** button at the top
3. Start typing your messages
4. The AI will stop responding automatically

### Getting Alerted

When the AI hands a conversation off, everyone in the organization with the dashboard open hears a sound and sees a toast. To be alerted with the dashboard closed, turn on **Alerts on this device** in the Inbox or under **Settings** → **My Profile**: your browser or phone then shows a notification that opens the conversation. Each device is enabled separately. On iPhone and iPad, add Hay to the Home Screen first and open it from there, otherwise iOS does not deliver notifications.

> **What happens when you take over:**
>
> - Status changes to "Manual Control"
> - AI stops generating responses
> - You can chat directly with the customer
> - All your messages are logged

### Exiting Takeover

When you're done:

- Click **Release Conversation** to hand the conversation off. A dialog appears with two options:
  - **Return to AI** - The assistant resumes handling messages automatically
  - **Return to Queue** - The conversation is placed back in the queue for another team member to pick up
- Click **Close Conversation** (a separate button) to mark the conversation as closed

## Supervision Mode

Want to review AI responses before they reach customers? Use **Test Mode** (also called Require approval for AI messages).

### What is Test Mode?

- AI responses are held in a queue before sending; the customer sees nothing until a team member acts
- A team member reviews and approves each message
- Messages can be edited before approval or blocked entirely
- Approving sends the reply on the conversation's own channel: web widget, WhatsApp, Instagram or Chatwoot

> **Note:** A "Supervise" toggle also appears on conversation pages, but it is not yet functional — it only changes the button label without affecting behavior. For actual review-before-send capability, use Test Mode.

> **WhatsApp and Instagram:** Meta only accepts replies within 24 hours of the customer's last message. A reply approved after that window is not delivered; the message is marked as failed in the conversation and the customer has to write again before you can answer.

### How to Enable Test Mode

1. Go to **Settings** → **Workspace** → **General**
2. Under **Test Mode Default**, tick **Require approval for AI messages by default**
3. Click **Save Changes**

To hold replies on a single channel only, go to **Settings** → **Privacy** → **Customer Privacy** → **AI on your channels** and set that channel's **AI replies** to **Drafts only (a person approves)**. Choose **Off (your team replies)** to stop the AI on a channel, or **Send automatically** for the normal behavior.

When enabled, bot messages appear with a "Waiting for approval" badge and must be approved by a team member before being delivered to the customer. Click **Approve & Send** to open **Review AI reply**, edit the reply if needed (**Approve & Send edited**), or click **Block** to discard it. An edited or blocked reply can become a test case: see [Tests](/docs/user-guide/tests/).

**Perfect for:**

- Onboarding a new setup
- High-value customers
- Sensitive topics
- Quality assurance

## Test Conversations (Playground)

Practice and test your assistant without affecting real customers.

### Starting a Test

1. Go to **Conversations**
2. Click **Conversation Playground**
3. Chat with your assistant like a customer would. **Test Scenarios** buttons (Ask About Hours, Request Support, Cancel Subscription, Request Human) fill in common messages, and **New Test** starts fresh.

You can also chat with the assistant from the **Test your assistant** box on the [Set up board](/docs/user-guide/setup/).

### What's Different in the Playground

- Marked clearly as **Conversation Playground**, with an **Auto-sends** badge
- Messages send automatically (no waiting)
- Never billed and not counted as a real conversation
- **Actions are simulated** by default, so a refund or cancellation in a test does not touch your store. A badge shows the mode. Click **Use live store** (and confirm) to send actions in the test conversation to your real store, and **Simulate actions** to switch back.

**Use playground to:**

- Test new playbooks (from the playbook editor, **More actions** → **Test this draft**)
- Try edge cases
- Train team members
- Show demos to stakeholders

## Conversation Actions

### Improve from a conversation

From a conversation's actions you can ask Hay to propose a fix: **Improve playbook** (a suggested edit you review and apply to the playbook's draft) or **Improve knowledge** (a suggested document edit or a new Q&A). Nothing is saved until you confirm. You can also rate a reply, or save it as a test case.

### Reply window and delivery problems

On channels with a reply window (WhatsApp, Instagram), the conversation shows when the window closes. After it closes, only an approved template reaches the customer: use **Send a template**, pick a template and click **Send template**. If a reply could not be delivered, the message shows **Delivery failed** and an alert tells you Hay.chat retried 3 times; open the conversation to follow up.


### Export Conversation

Save a complete record:

1. Click the **Export** button
2. Choose **Export as PDF** or **Export as CSV** (or **Evidence pack for a dispute**, see [Exports and evidence packs](/docs/user-guide/exports-and-evidence/))
3. Download includes all messages, timestamps, and metadata

**Perfect for:**

- Sharing with team members
- Record keeping
- Training materials
- Compliance documentation

## Managing Customer Messages

### Messages You'll See

**Message Types:**

- 💬 **Customer** - Message from the customer
- 🤖 **BotAgent** - AI response
- 👤 **HumanAgent** - Human agent message
- ℹ️ **System** - System notification
- 🛠️ **Tool** - Tool call/result
- 📄 **Document** - Document reference
- 📋 **Playbook** - Playbook step
- 🛒 **ProductRecommendation** - Suggested product(s)

> **Note:** Feedback (👍/👎) is an action on a BotAgent message, not a separate message type. File and image attachments are shown via a 📎 paperclip icon on the message that carries them, not as their own message type.

### Handling Attachments

When customers send files:

1. Click the attachment to view
2. Download if needed
3. AI can read text in images and PDFs
4. Use context to provide better answers

## Best Practices

### Reviewing Conversations

Make it a habit to review conversations to improve your assistant:

**Look for:**

- ✅ Great responses (what worked well?)
- ❌ Poor responses (what needs improvement?)
- 🔁 Repeated questions (add to your docs!)
- 😟 Frustrated customers (why did it escalate?)

### Responding to Feedback

Webchat visitors can rate the latest AI answer with 👍 or 👎 (their ratings show up in [Feedback Insights](/docs/user-guide/analytics/#feedback-insights)). When customers rate messages with 👍 or 👎:

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

- Use the [Inbox](/docs/user-guide/inbox/) to handle escalations
- Set up your [AI assistant](/docs/user-guide/agents/)
- Check out [Playbooks](/docs/user-guide/playbooks/) to automate common workflows
