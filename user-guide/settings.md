---
layout: docs.njk
title: Account & Workspace Settings
description: Configure your Hay organization, team, business hours, privacy, API access, Git connections, and AI models.
section: user-guide
---

## Accessing Settings

Click **Settings** in the left sidebar to expand the settings menu. Each entry is a section; sections with several pages show them as tabs along the top. What you see depends on your role.

**Admins and Owners** see:

- **Assistant** - tabs **Identity**, **Actions** and **AI Models** (see the [AI assistant guide](/docs/user-guide/agents/) and [Action approvals](/docs/user-guide/action-approvals/))
- **Channels** - **Channel list**: where customers reach you. **Webchat** is always the first card and opens the [Webchat](/docs/user-guide/webchat/) settings page
- **Workspace** - tabs **General**, **Users**, **Business Hours** and **Single sign-on** (Owners only)
- **Privacy** - tabs **Workspace data** and **Customer Privacy**
- **Developers** - tabs **API Tokens** and **Git Connections**
- **Billing & Usage** - **Usage** (see [Billing and usage](/docs/user-guide/billing-and-usage/))
- **My Profile**

**Other roles** see **My Profile** only.

Installed integrations can add their own entries to the Settings menu (shown to Admins and Owners only).

## General Settings

**Settings** → **Workspace** → **General** ("Manage your platform preferences and configuration"). Use the **Save Changes** button in the header to persist changes, or **Reset to Defaults** to start over.

### Organization

- **Organization Name** — the name of your organization
- **Website** — your store's website (for example `shop.com`). Hay reads its sitemap so the AI links only to pages that exist, and brings in the Webchat logo and brand colour from it (see [Webchat](/docs/user-guide/webchat/#brand-logo-and-colour-from-your-website))
- **About Organization** — a free-text description of what your organization does (max 10,000 characters). This is not just cosmetic: it's given to the AI assistant as business context, so a good description improves response quality.
- **Logo** — upload a square image, max 2MB (JPG, PNG, WebP, or GIF). You can remove it at any time. The Webchat uses this logo when it has no logo of its own.

### Platform Settings

- **Default Language** — default language for new conversations and system messages (English and Portuguese available in the UI; the backend supports additional languages for conversation handling)
- **Timezone** — used for displaying timestamps
- **Date Format** — MM/DD/YYYY (US), DD/MM/YYYY (EU), YYYY-MM-DD (ISO), or DD MMM YYYY, with a live preview
- **Time Format** — 12-hour (AM/PM) or 24-hour, with a live preview
- **Test Mode Default** — when the "Require approval for AI messages by default" checkbox is enabled, AI messages require human approval before being sent to customers. Playground conversations always auto-send regardless. You can also hold replies per channel under **Customer Privacy** → **AI on your channels**. This is separate from approving store actions such as refunds; see [Action approvals](/docs/user-guide/action-approvals/).

### AI Confidence Guardrails

Configure how AI responses are evaluated for confidence and accuracy:

- **High Confidence Threshold** / **Medium Confidence Threshold** — minimum scores (0.0–1.0) for each quality tier (defaults: 0.8 high, 0.5 medium)
- **Enable Automatic Recheck** — when confidence is medium, automatically retrieve more documents and regenerate the response. The recheck uses two tunable parameters:
  - **Max Documents** — number of documents to retrieve (default: 10)
  - **Similarity Threshold** — lower threshold for broader search (default: 0.3)
- **Enable Human Escalation** — when confidence is low, escalate to a human agent instead of sending the AI response
- **Fallback Message** — message shown when confidence is low and escalation is disabled. It is automatically translated to match the conversation language.

### Two-factor for the team

On the same page, **Two-factor authentication for your team** has a **Require two-factor authentication** switch. Members who haven't set it up are asked to at their next sign-in.

### Danger Zone

Visible to **Owners only**.

**Delete organization** permanently deletes the organization and all associated data. The confirmation dialog warns that this will immediately:

- Stop all active conversations
- Delete the assistant, playbooks, and documents
- Remove all team members from the organization
- Delete all customer data and conversation history
- Invalidate all API keys

You must type **DELETE** to confirm. This action is irreversible — export anything you need first.

## Users (Team Members)

**Settings** → **Workspace** → **Users** ("Manage who has access to your organization").

### Roles

Hay has six roles: **Owner**, **Admin**, **Contributor**, **Member**, **Viewer**, and **Agent**. Owners and Admins have full access to the settings menu; only Owners can delete the organization. The **Agent** role cannot open **Insights** or **Feedback**.

### Inviting a team member

1. Click **Invite Member**
2. Enter the **Email Address**
3. Select a **Role**
4. Optionally add a personal **Message** to the invitation
5. Click **Send Invitation**

### Managing members and invitations

- **Search** members by name or email, and filter by role
- **Change Role** — update a member's role (Owners only — Admins can invite members but cannot change roles or remove existing members)
- **Remove Member** — remove someone from the organization (Owners only; cannot be undone)
- **Pending Invitations** — see invitations that haven't been accepted yet, with options to **resend** or **cancel** each one

## Privacy & Data (Your Own Data)

**Settings** → **Privacy** → **Workspace data** covers _your_ personal data as a Hay user, under GDPR. (For your customers' data, see [Customer Privacy](#customer-privacy) below.)

### Export Your Data

Request a copy of all your personal data in JSON format. The export includes:

- Profile information (email, name, account settings)
- Organization memberships and roles
- Audit logs (last 1000 events)
- Documents and content you've created

Exports typically take less than 5 minutes; you'll receive an email with a download link when ready.

### Delete Your Account

Permanently delete your account and associated data:

- Your account is permanently deactivated
- Your access to the organization's API keys is removed (API keys are organization-scoped, not user-scoped)
- Personal information is removed or anonymized
- Some audit logs may be retained (anonymized) for compliance

You must type **DELETE** to confirm, and then click a verification link sent to your email to complete the process. This cannot be undone.

### Active Privacy Requests

Track the status of your export and deletion requests on the same page, with a **Download** action when an export is ready.

## API Tokens

**Settings** → **Developers** → **API Tokens** ("Manage API tokens for organization-level access").

### Connection details

At the top of the page you'll find the values needed to connect Hay to another platform (such as the Shopify chat widget):

- **Hay.chat Server URL**
- **Organization ID**

### Creating a token

1. Click **Create Token**
2. Enter a **Token Name** (e.g., "Production API Token")
3. Optionally set an **Expiration Date** (leave empty for no expiration)
4. Select **Scopes** — which API operations the token can perform. Note: as the UI states, scopes are **not enforced yet**; all tokens currently have organization-level access.
5. Click **Generate Token**
6. **Copy the token immediately** — it is shown only once and cannot be retrieved later

### Managing tokens

For each token you can see its status (Active / Inactive / Expired), creation date, last-used date, and expiration. You can edit a token's name and scopes, **revoke** it (stops working immediately), or **delete** it permanently.

> 🔒 **Security tip:** Treat API tokens like passwords. Never share them publicly or commit them to code repositories.

## Business Hours

**Settings** → **Workspace** → **Business Hours** ("When your team answers each channel. Outside these hours the AI keeps answering, and conversations that need a person wait in the queue until you open."). Business Hours used to sit under **Channels**; it is now part of the **Workspace** section.

- **Add schedule** — give it a **Name**, a **Timezone**, the **Channels** it covers, **Weekly hours**, and **Holidays and closures**. Leave every channel unticked to make a schedule the default for channels without their own schedule
- **Right now** — what a shopper who asks for a person would be told on each channel at this moment
- **Quick check with the team** — when the AI doesn't know a fact, it asks your team instead of handing the whole chat over

With no schedule, every channel counts as open. The Webchat uses your business hours for its status line ("Typically replies in …", "Back Mon 09:00").

## Webchat

Webchat settings are no longer under **Settings** → **Webchat**. They have their own page, opened from the **Webchat** card in **Channels**. See the [Webchat](/docs/user-guide/webchat/) guide.

## Git Connections

**Settings** → **Developers** → **Git Connections** ("Connect your Git repositories to install and sync plugins automatically").

- **Connect GitHub** — connects a GitHub account via a GitHub App. (If the server isn't configured with GitHub App credentials, the page tells you which environment variables are required.)
- **Connected Accounts** — GitHub accounts connected to your organization, with **Browse Repos**, **Manage** (opens GitHub installation settings), and a disconnect button
- **Select a repository** — pick a repository and click **Install Plugin** to install it as a Hay plugin
- **Git Plugins** — plugins installed from Git, each showing sync status (Up to date / Update available), last sync time, commit, and a **Sync Now** button

## AI Models

**Settings** → **Assistant** → **AI Models** ("Choose which AI provider and models power your assistant").

This page is gated behind an advanced-settings warning — "**Advanced — changes here affect the assistant**" — because changing the defaults can radically change response quality, cost, latency, and reliability. Click "I understand — let me edit" to unlock editing.

### Chat Provider

- **Provider** — the provider used to generate the assistant's responses. Choose from **OpenAI**, **Anthropic (Claude)**, **Google Gemini**, **Mistral**, **xAI Grok**, **Custom (OpenAI-compatible)**, or **Custom — supports tool calling (OpenAI-compatible)** (seven options total).
- **Base URL** — the provider's OpenAI-compatible API base URL. Shown only for Mistral, Grok, and custom endpoints.
- **Use my own API key** — when off, Hay's managed AI is used (billed with your plan). When on, enter your own provider **API Key** — it is stored encrypted and never shown again after saving.

### Model Tiers

The assistant picks a tier by task complexity; map each tier to a model for your provider:

- **Hard (most capable)** — orchestration, planning, and guardrails
- **Medium (balanced)** — audits and translations
- **Easy (cheapest)** — bulk document summaries

Each tier offers a model list plus a **Custom…** option to enter any model id. Switching to a different provider resets the tier models to that provider's defaults.

### Embeddings (Managed)

Embeddings are always managed by Hay and pinned to 1536 dimensions. You can pick the **Embedding Model** from Hay's list (it must produce 1536-dimension vectors, e.g., `text-embedding-3-small`), but you cannot bring your own embedding provider.

## Customer Privacy

**Settings** → **Privacy** → **Customer Privacy** ("Manage GDPR data requests for your customers"). Admins and Owners.

### Data Retention Policy

Configure automatic anonymization of closed conversations:

- **Conversation Retention Period** — Disabled (keep forever), 30, 60, 90, 180, or 365 days
- When enabled, closed conversations older than the retention period are automatically anonymized daily: messages are permanently deleted, while conversation metadata is preserved for analytics
- Conversations marked with **Legal Hold** are exempt from anonymization

> For full details on anonymization, legal holds, and the cleanup schedule, see the [Data Retention & Privacy](/docs/user-guide/data-retention/) guide.

### AI processing, AI label and customer files

Also on this page:

- **AI processing** shows the region (**EU only (EU data centres)** or **Any region**), the provider, and whether **Photos and PDFs** are read, with a **Change in AI Models** link.
- **AI on your channels** sets the **Label text** that tells shoppers a reply comes from AI (leave it empty to use "AI assistant" in the shopper's language) and, **per channel**, whether to **Show label** and what **AI replies** do: **Send automatically**, **Drafts only (a person approves)** or **Off (your team replies)**. For organizations that serve the EU, the label is always shown. **Start on Web Chat only** sets every other channel to Off.
- **Customer photos and files** has **Read images** (the AI looks at photos and PDFs shoppers send), **Let the AI read ID documents** (off by default, Owner only) and **Product case email**, where Hay.chat sends the summary when the AI completes a product-problem case.

### Initiate Privacy Request

Start a GDPR data export or deletion request on behalf of a customer:

1. Choose **Identify Customer By** — Email Address, Phone Number, or External Customer ID
2. Enter the customer identifier
3. Click **Request Data Export** or **Request Data Deletion**

The customer receives a verification email to confirm the request — the organization cannot complete it unilaterally.

### Request History

View and track all customer privacy requests initiated by your organization.

## My Profile

**Settings** → **My Profile** ("Manage your personal information and account security"). Available to every role.

- **Profile Picture** — upload a square image, max 2MB (JPG, PNG, WebP, or GIF); change or remove any time
- **Profile Information** — First Name and Last Name
- **Email Address** — change your login email. A verification link is sent to the new address (expires in 24 hours); until you click it, the change is shown as **Pending** with options to **Resend Verification** or **Cancel Change**.
- **Change Password** — enter your current password and a new one. Passwords must be at least 8 characters and include uppercase, lowercase, a number, and a special character. A confirmation email is sent after a successful change.
- **Two-factor authentication** — click **Set up two-factor**, confirm your password, add the setup key to an authenticator app and enter the code. Save the recovery codes shown (each works once and they are not shown again). Afterwards you can get **New recovery codes** or **Turn off**.
- **Active Sessions** — devices signed in to your account; a session ends 30 days after sign-in. **Sign out** one device or **Sign out all other devices**.
- **Alerts on this device** — push notifications when a conversation needs a human, and **Hide customer details on the lock screen**. See [Team alerts, pause and business hours](/docs/user-guide/team-alerts-and-hours/).
- **Recent Security Activity** — a log of recent account events: profile updates, email changes, password changes, logins, and API key creation/revocation

## Common Settings Questions

### Who can access settings?

Owners and Admins see the full settings menu. All other roles see **My Profile**. Only Owners can delete the organization or set up single sign-on.

### Where do I find my organization ID?

Go to **Settings** → **Developers** → **API Tokens** — the **Connection details** card shows your Hay.chat Server URL and Organization ID.

### How do I put AI messages under human review?

Enable **Test Mode Default** in **Settings** → **Workspace** → **General** → **Platform Settings**. Pending AI messages then appear in the conversation view for approval before being sent. To hold replies on one channel only, use **Customer Privacy** → **AI on your channels** → **Drafts only**.

### Does Hay have billing, 2FA, or webhook settings?

Plan, extra-resolution switch and invoices live on [hay.chat/billing](https://hay.chat/billing/). The resolution count and ledger are under **Settings** → **Billing & Usage** → **Usage**. Two-factor authentication exists: each user sets it up under **Settings** → **My Profile** → **Two-factor authentication** (authenticator app code plus recovery codes), and an Owner can switch on **Require two-factor authentication** for the whole team under **Settings** → **Workspace** → **General**. There is no webhook configuration in Settings.

### Why is there a usage banner at the top of the dashboard?

Your workspace has used 80 % or more of its included resolutions for the period. At 100 % the banner tells you whether extra resolutions are being billed or the AI is paused (see the [FAQ](/docs/user-guide/faq/#what-happens-if-i-exceed-my-resolution-limit)). Dismissing the banner hides it for the current tab only.

## Next Steps

- Set up [Integrations](/docs/user-guide/integrations/) after configuring settings
- Set up your [AI assistant](/docs/user-guide/agents/)
- Review the [Data Retention & Privacy](/docs/user-guide/data-retention/) guide for compliance details
- Invite your team members to get started
