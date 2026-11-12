---
layout: docs.njk
title: Your AI Assistant
description: Hay has one AI assistant for your whole organization. Give it a name, a tone, standing rules and a hand-off procedure, and it answers on every channel.
section: user-guide
---

## What is the assistant?

Your organization has **one AI assistant**. It answers customers on every channel (web chat, WhatsApp, Instagram, Messenger, email and the rest) and:

- Has a **name** customers see in the chat widget
- Follows the **instructions** you write
- Speaks in the **tone** you choose
- Uses your [documents](/docs/user-guide/documents/) and [playbooks](/docs/user-guide/playbooks/) to answer questions
- Hands conversations to your team following your **Human escalation** rules

There are no separate agents to create, enable, assign to channels or pick between. Earlier versions of Hay let you create several agents; that is gone. To make the assistant behave differently in one situation, write a [playbook](/docs/user-guide/playbooks/): all active playbooks apply to every conversation.

## Where to find it

Go to **Settings** → **Assistant** in the left sidebar (Admins and Owners). The page has three tabs: **Identity**, **Actions** and **AI Models**.

- **Identity** is described below
- **Actions** decides which store actions need a person's approval (see [Action approvals](/docs/user-guide/action-approvals/))
- **AI Models** chooses the AI provider and models (see [Settings](/docs/user-guide/settings/#ai-models))

## Identity

Fill in the fields, then click **Save Changes** (or **Discard changes** to go back). Changes take effect for new conversations.

**Name**

- Shown to customers in the chat widget. Leave it empty to show the widget title instead.

**Description**

- An internal note. Customers and the assistant never see it.

**Instructions**

The assistant's general behavior and standing rules, written in a rich text editor. Type `@` to insert an action or document, `/` for blocks. Put situation-specific steps in Playbooks, not here.

```
You are a helpful customer support assistant for an online store.

Your job is to:
- Answer questions about orders, shipping, and products
- Be friendly and patient
- Always check our documentation before answering

Important guidelines:
- Never make promises about shipping dates
- Always verify order numbers before looking them up
```

**Initial Greeting Message**

The first message customers see when they start a conversation. If you leave it empty, a default greeting is used. It is translated to the conversation's language automatically.

**Tone**

Pick **Professional**, **Casual** or **Enthusiastic**. Each fills the text box under the cards with a description of that tone, and you can edit it freely or write your own.

| Tone             | When to use                    |
| ---------------- | ------------------------------ |
| **Professional** | B2B, enterprise customers      |
| **Casual**       | Young audience, informal brand |
| **Enthusiastic** | Energetic, upbeat brand voice  |

**Things to Avoid**

Topics or phrases the assistant should never use:

```
- Don't promise specific dates without checking systems
- Avoid technical jargon with non-technical customers
- Never say "that's not possible" without offering alternatives
```

**Language.** The assistant answers in the customer's language and falls back to the organization's default language in **Settings** → **Workspace** → **General**. There is no per-assistant language setting.

## Human escalation

Under **Human escalation**, tell the assistant what to do when a customer needs a person.

**If any human agent is available** - instructions for that case. Leave it empty to simply move the conversation to the queue (it then shows up in the [Inbox](/docs/user-guide/inbox/)).

```
Let the customer know you'll connect them with a teammate.
Give a short summary of what was discussed.
```

**If all human agents are unavailable** - what happens outside business hours or when nobody is online. Choose one:

- **Follow my instructions** - write them yourself, for example "Apologize, collect their email and say someone will reply within 2 business hours."
- **Create a ticket** - the assistant makes sure it has the customer's email, raises a ticket with a summary and the full conversation, and tells the customer the team will get back to them. The conversation is then closed instead of waiting in the queue. This needs a connected helpdesk such as Zendesk: pick it under **Helpdesk**, and use **Prefill every ticket with** to set default ticket fields. Without a helpdesk the page links to **Browse integrations**.

See [Team alerts, pause and business hours](/docs/user-guide/team-alerts-and-hours/) for how opening hours change what shoppers are told.

## Reviewing replies before they send

There is no per-assistant approval switch. To hold the AI's written replies for a person to approve, use **Test Mode Default** under **Settings** → **Workspace** → **General**, or set a channel to **Drafts only** under **Settings** → **Privacy** → **Customer Privacy** → **AI on your channels**. See [Settings](/docs/user-guide/settings/) and [Conversations](/docs/user-guide/conversations/#supervision-mode).

## Writing great instructions

**Do:**

- Be specific and clear
- Use examples
- Include what to do, not just what not to do
- Think like you're training a new employee

**Don't:**

- Be vague ("be helpful")
- Use technical jargon
- Write a novel
- Contradict yourself

Start simple: write basic, clear instructions, [test the assistant](/docs/user-guide/quick-start/#step-5-test-your-assistant), handle one main purpose well, and expand based on real conversations.

## Improving results

Your Dashboard and [Quality](/docs/user-guide/analytics/) pages show where the assistant struggles.

- **Low resolution rate or many hand-offs:** read recent conversations, then add documents or a playbook for the topics it misses. **Insights** (under **Quality**) lists these patterns for you.
- **Wrong or risky answers:** rate the reply in the conversation, then fix the document or playbook it relied on.
- **Wrong tone:** adjust **Tone** and **Things to Avoid**.

## Common questions

### Can I have more than one assistant?

No. Hay has one assistant per organization.

### Can the assistant learn on its own?

It improves based on the documents you add and playbooks you write. It does not change itself without your guidance. Insights can suggest edits, but you review them before anything is saved.

### Can the assistant use a different language per channel?

It answers in the customer's language on every channel.

## Next steps

- Set up [Playbooks](/docs/user-guide/playbooks/) to give the assistant step-by-step workflows
- Upload [Documents](/docs/user-guide/documents/) to train it
- Review [Conversations](/docs/user-guide/conversations/) to see it in action
