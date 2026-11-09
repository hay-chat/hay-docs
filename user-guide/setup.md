---
layout: docs.njk
title: Set Up Your Assistant
description: The Set up board checks what your assistant can already handle, shows what is missing, and lets you test it before customers see it.
section: user-guide
---

## What is the Set up board?

**Set up** is the first entry in the left sidebar. It opens a board titled **What can your assistant handle?** that lists the requests your customers are likely to make, tests your assistant on each one, and shows what to do next. It is how most new workspaces get started, and it replaces the old checklist-style walkthrough.

Only **Owners and Admins** can open it. Other roles see "Only owners and admins can set up Hay".

**Set up** stays in the sidebar, with a progress badge such as `3/15`, until the board is finished. A **Setup progress** widget at the bottom of the sidebar shows how many requests are ready and links to **Continue setup**. New workspaces that haven't finished or set aside setup land on the board when they sign in.

## Step 1: Answer three questions

The first time, Hay asks a few questions. Use **Back** and **Continue** to move, or **Skip questions** to go straight to the board.

1. **What kind of business is this?** Enter **Your website** and choose a **Business type**: Online store, Software or SaaS, Service provider, or Something else. Hay reads your website to suggest a type ("Looks like you, from your website") and to tailor what it asks next.
2. **What do you already use?** Choose everything that applies, grouped as where customers contact you, where you sell, payments, customers and CRM, support, marketing, docs and knowledge, and bookings. This decides what your assistant can do for customers.
3. **What do customers ask most?** Review the suggested questions, remove the ones that don't fit and add your own with **Add a question customers ask**. Hay tests your assistant on these.

Finish with **See what your assistant can do**. A **Your list so far** panel on the side shows the list as you build it and the progress of reading your website.

## Step 2: Read the board

Each row is a customer request, with a status and the next step:

| Status | What it means | Button |
| --- | --- | --- |
| **Checking…** / **Reading your site** | Hay is testing it, or still reading your website | none |
| **Ready** | The assistant can answer this | **Try it** |
| **Missing answer** | Your website or documents don't say | **Add answer** |
| **Needs connection** | The assistant needs your platform to do this (for example look up orders) | **Connect** or **Choose platform** |
| **Needs a playbook** | No playbook covers this yet | **Write your own** |
| **Playbook off** | A playbook exists but is not turned on | **Review** |
| **Couldn't check** | The check failed | **Retry** |

A banner above the list shows **Up next**, the single step that unlocks the most requests. The header shows how many are ready, for example "0 of 15 ready". Use **Check again** if the checks fail to run, the **x** on a row (**Not relevant**) to remove a question, and **Change answers** to redo the three questions.

- **Add answer** opens a box titled **How should your assistant answer?**. Write the answer and click **Save answer**.
- **Review** on a playbook shows **What the playbook does** and its **Steps**. Click **Turn on** to activate it, or **Open playbook** to edit it. Nothing changes for customers until you turn it on.
- **Connect** starts the integration's connection for you. If it fails, nothing was changed.

While your website is still being read, a request listed as missing may be on a page Hay hasn't reached yet. Large sites finish in the next daily update.

## Step 3: Test your assistant

The **Test your assistant** chat on the right is live from the start. Ask what a customer would ask. Nothing here reaches real customers. If the assistant isn't sure, the chat says it handed off to your team, and a **Source** line shows where an answer came from.

Under the board, **Test on real conversations** lets you import past conversations and replay them to see how your assistant would have answered. It is optional, never sends anything to customers, and is not billed. It counts toward a monthly allowance of test conversations, shown as "(N of M left)". Results are grouped as **Would answer**, **Would hand off**, **Missing knowledge**, **Needed an action** and **Wrong or risky**, with **Open the full report**. See [Tests](/docs/user-guide/tests/).

## Step 4: Add it to your site

Click **Add to your site** (top right of the board, or the button on the **Where customers reach you** card). A window shows the install code: click **Copy code** and paste it into your site's HTML just before the page ends. It also tells you how many requests on your list aren't ready; the assistant hands those to your team instead of guessing. **Appearance and more options** goes to the webchat settings.

The **Where customers reach you** card also lists your channels: web chat, and any other channel with **Connected** or **Not connected**, with a **Connect** button for each.

Click **Finish later** at any time. You can come back from the sidebar.

## Related

- [Quick Start](/docs/user-guide/quick-start/)
- [Documents](/docs/user-guide/documents/) and [Playbooks](/docs/user-guide/playbooks/) cover the fixes the board points to
