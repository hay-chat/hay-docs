---
layout: docs.njk
title: Account & Workspace Settings
description: Configure your Hay organization, team, privacy, API access, webchat widget, Git connections, and AI models.
section: user-guide
---

## Accessing Settings

Click **Settings** in the left sidebar to expand the settings menu. What you see depends on your role:

**Admins and Owners** see the full menu:

- **General** — organization details, platform preferences, AI confidence guardrails
- **Agents** — manage your AI agents (see the [Agents guide](/docs/user-guide/agents/))
- **Channels** — manage communication channels
- **Users** — team members and invitations
- **Privacy & Data** — your own GDPR rights (export/delete your account)
- **API Tokens** — organization-level API access
- **Webchat** — website chat widget configuration
- **Git Connections** — install plugins from GitHub repositories
- **AI Models** — the AI provider and models behind your agents

**All roles** see:

- **Customer Privacy** — GDPR requests on behalf of customers, and the data retention policy
- **My Profile** — your personal profile, email, and password

Installed plugins can add their own entries to the Settings menu (these are shown to Admins and Owners only).

## General Settings

**Settings** → **General** ("Manage your platform preferences and configuration"). Use the **Save Changes** button in the header to persist changes, or **Reset to Defaults** to start over.

### Organization

- **Organization Name** — the name of your organization
- **About Organization** — a free-text description of what your organization does (max 10,000 characters). This is not just cosmetic: it's given to the AI assistant as business context, so a good description improves response quality.
- **Logo** — upload a square image, max 2MB (JPG, PNG, WebP, or GIF). You can remove it at any time.

### Platform Settings

- **Default Language** — default language for new conversations and system messages (English or Portuguese)
- **Timezone** — used for displaying timestamps
- **Date Format** — MM/DD/YYYY (US), DD/MM/YYYY (EU), YYYY-MM-DD (ISO), or DD MMM YYYY, with a live preview
- **Time Format** — 12-hour (AM/PM) or 24-hour, with a live preview
- **Default Agent** — the agent that handles conversations when no specific agent is assigned
- **Test Mode Default** — when the "Require approval for AI messages by default" checkbox is enabled, AI messages require human approval before being sent to customers. Individual agents can override this setting, and playground conversations always auto-send regardless.

### AI Confidence Guardrails

Configure how AI responses are evaluated for confidence and accuracy:

- **High Confidence Threshold** / **Medium Confidence Threshold** — minimum scores (0.0–1.0) for each quality tier
- **Enable Automatic Recheck** — when confidence is medium, automatically retrieve more documents and regenerate the response. The recheck uses two tunable parameters:
  - **Max Documents** — number of documents to retrieve (default: 10)
  - **Similarity Threshold** — lower threshold for broader search (default: 0.3)
- **Enable Human Escalation** — when confidence is low, escalate to a human agent instead of sending the AI response
- **Fallback Message** — message shown when confidence is low and escalation is disabled. It is automatically translated to match the conversation language.

### Danger Zone

Visible to **Owners only**.

**Delete organization** permanently deletes the organization and all associated data. The confirmation dialog warns that this will immediately:

- Stop all active conversations
- Delete all agents, playbooks, and documents
- Remove all team members from the organization
- Delete all customer data and conversation history
- Invalidate all API keys

You must type **DELETE** to confirm. This action is irreversible — export anything you need first.

## Users (Team Members)

**Settings** → **Users** ("Manage who has access to your organization").

### Roles

Hay has six roles: **Owner**, **Admin**, **Contributor**, **Member**, **Viewer**, and **Agent**. Owners and Admins have full access to the settings menu; only Owners can delete the organization.

### Inviting a team member

1. Click **Invite Member**
2. Enter the **Email Address**
3. Select a **Role**
4. Optionally add a personal **Message** to the invitation
5. Click **Send Invitation**

### Managing members and invitations

- **Search** members by name or email, and filter by role
- **Change Role** — update a member's role
- **Remove Member** — remove someone from the organization (cannot be undone)
- **Pending Invitations** — see invitations that haven't been accepted yet, with options to **resend** or **cancel** each one

## Privacy & Data (Your Own Data)

**Settings** → **Privacy & Data** covers _your_ personal data as a Hay user, under GDPR. (For your customers' data, see [Customer Privacy](#customer-privacy) below.)

### Export Your Data

Request a copy of all your personal data in JSON format. The export includes:

- Profile information (email, name, account settings)
- Organization memberships and roles
- API keys metadata (no secrets included)
- Audit logs (last 1000 events)
- Documents and content you've created

Exports typically take less than 5 minutes; you'll receive an email with a download link when ready.

### Delete Your Account

Permanently delete your account and associated data:

- Your account is permanently deactivated
- All API keys are revoked
- Personal information is removed or anonymized
- Some audit logs may be retained (anonymized) for compliance

You must type **DELETE** to confirm, and then click a verification link sent to your email to complete the process. This cannot be undone.

### Active Privacy Requests

Track the status of your export and deletion requests on the same page, with a **Download** action when an export is ready.

## API Tokens

**Settings** → **API Tokens** ("Manage API tokens for organization-level access").

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

## Webchat Settings

**Settings** → **Webchat** ("Customize your website chat widget appearance and behavior").

### Widget Appearance

- **Widget Title** (e.g., "Chat with us") and **Widget Subtitle** (e.g., "We typically reply within minutes")
- **Position** — Left or Right side of the page
- **Theme** — Blue, Green, Purple, or Black

### Widget Behavior

- **Show greeting message when chat opens** — toggle, plus a customizable **Greeting Message**

### Security & Access

- **Allowed Domains** — one domain per line; use `*` to allow all domains. Controls where the widget can be embedded.
- **Enable webchat widget** — master on/off toggle

### Installation Code

The page provides a ready-to-copy **Installation Code** snippet — add it to your website before the closing `</body>` tag.

## Git Connections

**Settings** → **Git Connections** ("Connect your Git repositories to install and sync plugins automatically").

- **Connect GitHub** — connects a GitHub account via a GitHub App. (If the server isn't configured with GitHub App credentials, the page tells you which environment variables are required.)
- **Connected Accounts** — GitHub accounts connected to your organization, with **Browse Repos**, **Disconnect**, and **Disconnect & Remove Plugins** actions
- **Select a repository** — pick a repository and click **Install Plugin** to install it as a Hay plugin
- **Git Plugins** — plugins installed from Git, each showing sync status (Up to date / Update available), last sync time, commit, and a **Sync Now** button

## AI Models

**Settings** → **AI Models** ("Choose which AI provider and models power your agents").

This page is gated behind an advanced-settings warning — "**Advanced — changes here affect every agent**" — because changing the defaults can radically change response quality, cost, latency, and reliability for all agents in the organization. Click "I understand — let me edit" to unlock editing.

### Chat Provider

- **Provider** / **Vendor** — the provider and request format used to generate agent responses. OpenAI-compatible covers OpenAI, Mistral, Grok, and custom endpoints.
- **Base URL** — the provider's OpenAI-compatible API base URL (for custom providers)
- **Use my own API key** — when off, Hay's managed AI is used (billed with your plan). When on, enter your own provider **API Key** — it is stored encrypted and never shown again after saving.

### Model Tiers

Agents pick a tier by task complexity; map each tier to a model for your provider:

- **Hard (most capable)** — orchestration, planning, and guardrails
- **Medium (balanced)** — audits and translations
- **Easy (cheapest)** — bulk document summaries

Each tier offers a model list plus a **Custom…** option to enter any model id.

### Embeddings (Managed)

Embeddings are always managed by Hay and pinned to 1536 dimensions. You can pick the **Embedding Model** from Hay's list (it must produce 1536-dimension vectors, e.g., `text-embedding-3-small`), but you cannot bring your own embedding provider.

## Customer Privacy

**Settings** → **Customer Privacy** ("Manage GDPR data requests for your customers"). Available to all roles.

### Data Retention Policy

Configure automatic anonymization of closed conversations:

- **Conversation Retention Period** — Disabled (keep forever), 30, 60, 90, 180, or 365 days
- When enabled, closed conversations older than the retention period are automatically anonymized daily: messages are permanently deleted, while conversation metadata is preserved for analytics
- Conversations marked with **Legal Hold** are exempt from anonymization

> For full details on anonymization, legal holds, and the cleanup schedule, see the [Data Retention & Privacy](/docs/user-guide/data-retention/) guide.

### Initiate Privacy Request

Start a GDPR data export or deletion request on behalf of a customer:

1. Choose **Identify Customer By** — Email Address, Phone Number, or External Customer ID
2. Enter the customer identifier
3. Click **Request Data Export** or **Request Data Deletion**

The customer receives a verification email to confirm the request — the organization cannot complete it unilaterally.

### Request History

View and track all customer privacy requests initiated by your organization.

## My Profile

**Settings** → **My Profile** ("Manage your personal information and account security"). Available to all roles.

- **Profile Picture** — upload a square image, max 2MB (JPG, PNG, WebP, or GIF); change or remove any time
- **Profile Information** — First Name and Last Name
- **Email Address** — change your login email. A verification link is sent to the new address (expires in 24 hours); until you click it, the change is shown as **Pending** with options to **Resend Verification** or **Cancel Change**.
- **Change Password** — enter your current password and a new one. Passwords must be at least 8 characters and include uppercase, lowercase, a number, and a special character. A confirmation email is sent after a successful change.
- **Recent Security Activity** — a log of recent account events: profile updates, email changes, password changes, logins, and API key creation/revocation

## Common Settings Questions

### Who can access settings?

Owners and Admins see the full settings menu. All other roles can access **Customer Privacy** and **My Profile**. Only Owners can delete the organization.

### Where do I find my organization ID?

Go to **Settings** → **API Tokens** — the **Connection details** card shows your Hay.chat Server URL and Organization ID.

### How do I put AI messages under human review?

Enable **Test Mode Default** in **Settings** → **General** → **Platform Settings**. Pending AI messages then appear in the conversation view for approval before being sent. Individual agents can override the default.

### Does Hay have billing, 2FA, or webhook settings?

Not yet. There is currently no billing page, no two-factor authentication, and no webhook configuration in Settings.

## Next Steps

- Set up [Integrations](/docs/user-guide/integrations/) after configuring settings
- Configure your [Agents](/docs/user-guide/agents/)
- Review the [Data Retention & Privacy](/docs/user-guide/data-retention/) guide for compliance details
- Invite your team members to get started
