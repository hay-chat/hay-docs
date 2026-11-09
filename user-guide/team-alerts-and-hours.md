---
layout: docs.njk
title: Team Alerts, Pause and Business Hours
description: Get alerted when a conversation needs a person, pause the AI everywhere with one switch, and set business hours so shoppers are told the truth about when your team is back.
section: user-guide
---

## Alerts when a conversation needs a person

When Hay.chat hands a conversation to your team, it alerts the people who are set to **Available**.

1. **Right away:** a push notification to every device those people turned alerts on for.
2. **After 10 minutes with no answer** (during business hours): a second push.
3. **10 minutes after that:** an email to the available team members. If the second push reached no device, the email is sent straight away.

The alerts stop as soon as someone replies, takes over or is assigned to the conversation, or the conversation closes. When a shopper writes again in a conversation your team already holds, the person handling it is alerted too.

### Turn alerts on

Alerts are set **per device**, so turn them on from each phone or computer you use. Open the **Inbox** (or **Settings** → **My Profile**), click **Alerts on this device**, and allow notifications when your browser asks. Use **Send a test alert** to check it works.

- **iPhone:** iOS only shows web alerts when Hay.chat is on your Home Screen. In Safari, add Hay.chat to your Home Screen, open it from the new icon, and then turn alerts on. The Inbox shows these steps.
- **Hide customer details on the lock screen:** alerts then say only "A customer needs a human". Open Hay.chat to see who. This applies to all your devices.
- If the browser says notifications are blocked, allow them for the site in your browser settings and reload.

**Self-hosting:** push alerts need VAPID keys on the server (`VAPID_PUBLIC_KEY`, `VAPID_PRIVATE_KEY`, `VAPID_SUBJECT`). Without them the Inbox says alerts are not set up. Email alerts need email sending to be configured. See [Self-hosting](/docs/technical/self-hosting/).

## Pause AI everywhere

On the **Inbox** page, **Pause AI everywhere** stops every AI reply on every channel right away, including follow-ups and inactivity closes. Use it when something is wrong (a wrong price, a promotion that ended, an outage in your store).

- Shoppers who write while the AI is paused get one short message saying your team will reply, and their conversations wait in the Inbox for you.
- A banner shows that the AI is paused everywhere, who paused it and when.
- You can also be more precise: turn the AI off on **one channel only** (it then drafts replies for you to send).

Switch it back on from the same place when you are ready.

## Business hours

Go to **Settings** → **Channels** → **Business Hours** (Admins and Owners).

- Add a **schedule** with weekly opening hours in your store's time zone. A closing time earlier than the opening time runs past midnight.
- Add **holidays and closures**: whole days your team is closed.
- A schedule can cover specific channels. A schedule with no channels ticked is the default for every channel without its own.
- A **Right now** line shows what a shopper who asks for a person would be told on each channel.

With no schedule at all, every channel counts as always open.

### What shoppers are told

When a shopper asks for a person, the answer matches reality instead of a made-up excuse:

- **Open, a teammate is reachable:** the conversation goes to your team.
- **Open, but nobody is reachable:** the team gets an alert, and the shopper is told the team has been notified.
- **Closed:** the shopper is told your team is closed and when it is back, in your time zone.

Outside business hours the AI keeps answering questions it can handle. Conversations that need a person wait in the queue until you open.

In web chat, a shopper who asks for a person while you are closed can leave an email address (with their consent). If a teammate replies after they have closed the chat, Hay.chat emails the reply with a link back to the conversation. The link reopens the chat in the same browser.

### Quick check with the team

When the AI does not know a fact ("does this vase come gift-boxed?"), it can ask your team instead of handing over the whole conversation, and carry on with the shopper's other questions. Everyone on the team gets a notification and an email with a one-tap answer link. Under **Quick check with the team** you set how long until a "still checking" update (default and limits are on the page) and when to pass the question to the queue. Outside business hours, or when nobody is reachable, the question goes straight to the queue.

A conversation where a teammate supplied the answer is not billed as an AI resolution. See [Billing and Usage](/docs/user-guide/billing-and-usage/).
