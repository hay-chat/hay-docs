---
layout: docs.njk
title: Webchat
description: Add Hay's chat to your website, match it to your brand, and see what your visitors see.
section: user-guide
---

## What is Webchat?

Webchat is the chat on your website. Visitors open it from a button in the corner of the page, ask a question, and your AI assistant answers. When a person is needed, your team picks the conversation up in the Hay inbox. Webchat is built into Hay: there is nothing to install from the marketplace.

> **Access:** Webchat settings are available to users with the **Admin** or **Owner** role.

## Opening the Webchat settings

1. Go to **Channels** (in the Settings menu, under **Channels** → **Channel list**)
2. Click the **Webchat** card. It is always the first card in the list, so the list is never empty

The card shows where the chat stands:

- **Live** — the chat has been seen on your website ("The chat on your website, shop.com.")
- **Not installed** — the chat is switched on but has not loaded on any website yet ("Add one line of code to go live.")
- **Off** — the chat is switched off

Until the chat has been seen on a website, the card opens the **Install** tab. After that it opens **Appearance**.

The Webchat page has four tabs: **Appearance**, **Content**, **Install** and **Security**, with a live **Preview** beside them. At the top of the page, the **Show on your website** switch turns the chat on or off.

When you change something, a bar appears at the bottom of the page: "You have unsaved changes", with **Discard** and **Save**. A few things apply straight away without Save: uploading or removing the logo, turning under-attack mode on or off, and showing or replacing the signed-in customer secret.

### The live preview

The preview runs the real chat with your current settings, including changes you have not saved yet. Switch between:

- **Show**: **Home**, **Chat** (a short sample conversation) or **Ended** (the end-of-chat card)
- **Device**: **Desktop** or **Mobile**
- **Theme**: **Light** or **Dark**

The preview never creates conversations and never counts as the chat being installed.

## Appearance

### Brand: logo and colour from your website

Hay brings in your logo and brand colour from your website automatically. The website is the one set in **Settings** → **General** (the **Website** field). If no website is set, the Brand section says "Add your website to bring in your logo and colours automatically." with a link to add it.

When you save the website (and once, the first time the Webchat settings are opened with nothing set yet), Hay:

- Finds your site's icon: an SVG icon first, then the Apple touch icon, then the largest icon in the web app manifest, then `/favicon.ico`. Larger icons (96px and up) are preferred. Icons of 32px or smaller are not used as a logo
- Uses that icon as the chat **Logo** ("Your website icon")
- Picks up to three main colours from the icon (it ignores transparent areas, white, black and greys) and offers them as colour swatches. The first one becomes the **Brand colour**

Anything you change yourself is never overwritten by the import. If you upload your own logo, remove the logo, or choose a brand colour, a later website import leaves it alone. If the import can't reach your site or finds nothing usable, your settings simply stay as they are.

### Logo

- **Upload** or **Replace** — PNG, JPG, WebP, GIF or SVG, up to 2 MB. A new logo applies straight away
- **Remove** — shown for a logo you uploaded. The chat then falls back to your organization logo from **General** settings, and the website import does not bring its icon back
- With no logo at all, the chat shows the first letter of your organization name on your brand colour ("No logo yet. The chat shows your first letter.")

### Brand colour

Pick one of the swatches found in your website icon, use the colour picker, or type a hex code (for example `#14532D`) in the field beside the picker. Any colour works: the chat keeps text on it readable automatically. If white text would be hard to read on your colour, the chat uses dark text or a slightly darker shade of your colour for buttons and your visitors' messages. The chat button itself always keeps your exact colour.

The default brand colour is `#001DF5`.

### Look

- **Theme** — **Light**, **Dark**, or **Match the visitor** (follows the light or dark setting of the visitor's device). The default is Light
- **Button position** — **Bottom right** or **Bottom left**

### Assistant

The assistant's name and avatar shown in the chat come from **Assistant** → **Identity**. Click **Edit** to change them there.

## Content

### Home

What visitors see when they open the chat.

- **Heading** — the large heading on the home view. Leave it empty to show "How can we help?" in the visitor's language
- **Chat title** — shown at the top of a conversation when the assistant has no name (for example "Chat with us")
- **Status line** — "Typically replies in …", worked out from your [business hours](/docs/user-guide/settings/#business-hours) and how fast your team usually replies. Click **Business Hours** to change the hours

### Greeting

- **Start new chats with a greeting** — when on, a new conversation opens with your **Greeting** message (for example "Hi! How can we help you today?")
- **Message next to the chat button** — a short message shown beside the closed chat button until the visitor opens the chat or closes the message. Leave it empty to show nothing

### Before the chat

Ask for details, such as an email, before the first message. The pill shows whether the form is **On** or **Off**. Click **Set up a form** (or **Edit the form**) to build the **Pre-chat form**: a form title, description, button label and fields. Each field can be required and can be saved to the customer record. Changes to the form are saved with the bar at the bottom of the page.

### End of the chat

- **Ask visitors to rate the conversation** — when on (the default), visitors see a rating question when the conversation ends
- **Question** — leave it empty to show "How did we do?" in the visitor's language

### AI label

The chat shows "· AI" next to the assistant's name. The label is set in **Customer Privacy**.

## Install

### Install status

The top of the Install tab shows whether the chat is running on your website:

- **Working** — "Seen on shop.com 2 minutes ago." with a link to open that site. The chat counts as seen when it loads on a page of one of your **Allowed websites** while it is switched on
- **Not installed yet** — "Add the code below to your website. This changes to Working once a page with the chat loads."
- **Off** — "The chat is switched off. Turn on “Show on your website” and save to go live."

### Add to your website

Copy the code and paste it before the closing `</body>` tag on every page of your site. If someone else looks after your website, click **Email these steps to a developer** to send them the code.

You only add the code once. Everything you change here (logo, colour, title, position, greeting and the rest) updates on your site as soon as you save.

If your site has an older version of the code that also sets the title, position or greeting, those values win over the dashboard. Copy the code again to replace it.

Developers can find every option the code accepts in the [Webchat embed reference](/docs/technical/webchat-embed/).

### Where it can run

**Allowed websites** — the websites where your chat can start conversations, one domain per line:

- `shop.com` allows only that domain
- `*.shop.com` allows every subdomain (`www.shop.com` needs its own line or the wildcard)
- `*` allows any website. Anyone who copies your organization ID could then run your chat, and its AI usage, on their own site. Replace `*` with your domains before going live

### Signed-in customers

To link a chat to a customer who is signed in on your website, your website's server signs their customer ID with the secret shown here and passes `customerExternalId` and `customerExternalIdSignature` to the chat. Without a valid signature, the visitor is treated as a new anonymous customer.

The secret is hidden until an admin clicks **Show secret**. **Replace secret** makes signatures made with the old secret stop working at once, so update your website at the same time.

### Page actions

**Allow page actions** lets the AI use actions your own web pages register in the visitor's browser, such as opening a page. You can limit which sites may offer page actions; leave the list empty to use the allowed websites above.

## Security

- **Spam protection** — Hay screens the first messages of every webchat conversation and holds spam from the AI. Spam never counts toward your bill. Turn on **Double-check suspicious chats with AI** to have a small AI model read chats that look suspicious but not clearly spam, and use **Guidance for the spam check** to say what counts as spam for your store and what doesn't
- **Under-attack mode** — every visitor solves a bot check before their first message, for a set number of hours. It also turns on by itself when an unusual number of new chats arrive at once. Needs the Bot protection plugin. **Turn on** / **Turn off** apply straight away

## What your visitors see

### The chat button and window

The chat button sits in the corner you chose, in your exact brand colour. On a computer the chat opens as a window; on a phone it fills the screen. Visitors can close it with the close button or the Esc key.

### Home

Opening the chat shows the home view: your logo, your heading ("How can we help?" by default), the status line, and a message box. Typing a question there starts a conversation straight away. Under the box, a line explains that answers come from your AI assistant: "Answered by our AI assistant. The team steps in when it's needed."

If the visitor already has a conversation in progress, opening the chat goes straight to that conversation instead.

### Your conversations

Returning visitors see **Your conversations** on the home view: up to three of their conversations, one still in progress first (marked **Open**, with its last message), then ended ones with their date. A visitor can open one to read it again. The conversation open in the current browser tab is always listed; earlier ones appear once the visitor has agreed to the chat storing data on their device, so a first visit shows none. A signed-in customer also sees conversations they started on another device; those are listed but can only be opened on the device where they started, and the chat says so.

### In a conversation

- The header shows your brand colour, your logo (or the photo of the team member who took over), the assistant's name (or the chat title when the assistant has no name) and the status line. When a person can take over, up to two team members' avatars appear in the header
- Messages are grouped by sender. AI replies carry the assistant's name with "· AI"; replies from your team show the person's first name
- While the AI looks something up with one of your integrations, a progress line replaces the typing dots ("Looking up your order"). When it finishes, a short line such as "✓ Checked your order" stays above the answer. Integrations name these steps themselves; otherwise the chat says "Working on it…" and "Done"
- When an answer used documents that have a public web address, such as your help centre pages, a **sources** pill under it (for example "2 sources") opens a list of links to those pages. Internal documents are never listed
- The latest AI answer has thumbs up and thumbs down buttons. A visitor's rating is recorded as feedback on that message and appears in [Feedback Insights](/docs/user-guide/analytics/#feedback-insights). Rating again replaces the earlier rating
- The visitor's last message shows one tick once Hay has received it and two ticks once it has been delivered to whoever answers. When someone on your team opens the conversation in the inbox, it shows **Seen**. The AI never triggers Seen
- When a conversation is waiting for a person, a "Waiting for a person" card shows the usual wait. The visitor can leave an email and click **Notify me** to get the reply by email if they have left

### When the conversation ends

The message box is replaced by a "Conversation ended" card. If rating is turned on, it asks your question ("How did we do?" by default) with five faces: Very bad, Bad, Okay, Good and Great. After a rating it says "Thanks for your feedback". The visitor can click **Start a new conversation** to begin again.

## Common Webchat Questions

### Where did the Webchat settings go?

They used to be under **Settings** → **Webchat**. They now have their own page, opened from the **Webchat** card in **Channels**. Old links to the settings page take you there.

### Where are business hours now?

**Business Hours** moved to the **Workspace** section of the Settings menu, next to **General** and **Users**. They still control the chat's status line and what visitors are told when they ask for a person.

### Can I use any colour?

Yes. Pick any colour and the chat keeps its text readable. The four fixed colour themes and the widget subtitle are gone; existing colour themes were carried over as brand colours.

### Why hasn't my logo or colour changed after I updated my website?

The website import never overwrites a logo or colour you set yourself. Upload a new logo or pick a new colour on the **Appearance** tab.

## Next Steps

- Set your [business hours](/docs/user-guide/settings/#business-hours) so the status line is accurate
- Add public [Documents](/docs/user-guide/documents/) so answers can show their sources
- Review visitor ratings in [Feedback Insights](/docs/user-guide/analytics/#feedback-insights)
- If the chat doesn't show on your site, see [Troubleshooting](/docs/user-guide/troubleshooting/#web-chat-widget-not-appearing)
