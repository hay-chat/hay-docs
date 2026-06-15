---
layout: docs.njk
title: Account & Workspace Settings
description: Configure your Hay workspace, manage your account, and control security, privacy, billing, and team access.
section: user-guide
---

## Accessing Settings

Click **Settings** in the left sidebar to access all configuration options.

## General Settings

### Organization Information

**Organization Name**

- How your organization appears in Hay
- Visible to your team members
- Used in reports and exports

**Business Details**

- Industry
- Company size
- Primary use case
- Helps us provide better support

**Contact Information**

- Primary contact email
- Phone number (optional)
- Support escalation contacts

### Preferences

**Language**

- Default language for dashboard
- Doesn't affect customer-facing language
- 33+ languages supported, including English, Spanish, French, German, Portuguese, Italian, Dutch, Japanese, Korean, Chinese, Arabic, and more

**Time Zone**

- Sets times for analytics
- Affects scheduled reports
- Shows in conversation timestamps

**Date Format**

- MM/DD/YYYY (US)
- DD/MM/YYYY (International)
- YYYY-MM-DD (ISO)
- DD MMM YYYY (Readable, e.g., 15 Jun 2026)

## User Management

### Team Members

**Adding users:**

1. Go to **Settings** → **Users**
2. Click **Invite User**
3. Enter email address
4. Select role (see below)
5. Click **Send Invitation**

**User roles:**

| Role            | Permissions                                     |
| --------------- | ----------------------------------------------- |
| **Owner**       | Full access, billing, delete organization       |
| **Admin**       | Manage settings, users, integrations            |
| **Agent**       | Handle conversations, view analytics            |
| **Member**      | Access conversations and documents              |
| **Contributor** | Add and edit documents and playbooks            |
| **Viewer**      | Read-only access to conversations and analytics |

**Managing users:**

- View all team members
- Change roles
- Deactivate users
- Resend invitations

### Your Account

**Profile Information**

- Name
- Email
- Profile picture
- Contact preferences

**Change Password**

1. Go to **Settings** → **Profile**
2. Click **Change Password**
3. Enter current password
4. Enter new password
5. Confirm new password
6. Save

**Email Notifications**
Choose what emails you receive:

- ☐ Conversation escalations
- ☐ Daily summary
- ☐ Weekly performance report
- ☐ System alerts
- ☐ Product updates

## Security Settings

### Authentication

**Password Requirements**

- Minimum 8 characters
- Must include uppercase and lowercase
- Must include numbers
- Special characters recommended

**Two-Factor Authentication (2FA)**
Add extra security to your account:

1. Go to **Settings** → **Security**
2. Click **Enable 2FA**
3. Scan QR code with authenticator app
4. Enter verification code
5. Save backup codes

**Recommended authenticator apps:**

- Google Authenticator
- Authy
- Microsoft Authenticator

**Session Management**

- View active sessions
- See last login time and location
- Sign out of all other sessions

### API Tokens

Create API keys for integrations and custom development.

**Creating an API Token:**

1. Go to **Settings** → **API Tokens**
2. Click **Create Token**
3. Enter token name (e.g., "Shopify Integration")
4. Select permission scopes:
   - Conversations (read/write)
   - Messages (read/write)
   - Customers (read/write)
   - Content (documents, playbooks, agents)
   - Analytics (read)
   - Privacy & DSAR (manage requests)
   - Settings (read/write)
   - Full Access (all scopes)
5. Click **Generate Token**
6. **Copy the token immediately** (you won't see it again!)
7. Store securely

**Managing tokens:**

- View all active tokens
- See last used date
- Revoke tokens no longer needed

> 🔒 **Security tip:** Treat API tokens like passwords. Never share them publicly or commit to code repositories.

### Access Logs

View security-related activity:

- Login attempts
- Failed authentications
- API token usage
- Settings changes
- User role changes

**Filtering logs:**

- By date range
- By user
- By action type
- By success/failure

## Privacy & Data

### Customer Data Privacy

**Data retention:**
Configure how long conversation data is stored under **Settings** → **Customer Privacy**. When a retention period is set, closed conversations past the window are automatically anonymized — messages and personal data are removed while analytics metadata is preserved.

- Conversations: 30, 60, 90, 180, 365 days, or indefinitely
- Customer information: Active until requested deletion

> For full details on how anonymization works, legal holds, the cleanup schedule, and audit logging, see the [Data Retention & Privacy](/docs/user-guide/data-retention/) guide.

**Customer rights:**

- Data access requests
- Data deletion requests
- Data portability requests

### Privacy Requests

Handle GDPR and privacy requests:

**Customer Data Export:**

1. Go to **Settings** → **Privacy**
2. Click **Data Export**
3. Enter customer email
4. Select data types to export
5. Click **Generate Export**
6. Download when ready

**Customer Data Deletion:**

1. Go to **Settings** → **Privacy**
2. Click **Data Deletion Request**
3. Enter customer email
4. Confirm deletion
5. Request processed within 30 days

> ⚠️ **Note:** Deletion is permanent and cannot be undone.

### GDPR Compliance

Hay helps you comply with GDPR:

- ✅ Data processing agreements available
- ✅ Customer consent tracking
- ✅ Right to access
- ✅ Right to deletion
- ✅ Right to portability
- ✅ Data breach notification

**Compliance documentation:**

- Download DPA (Data Processing Agreement)
- View security certifications
- Access compliance reports

## Billing & Subscription

### Current Plan

View your subscription details:

- Plan name and features
- Number of conversations included
- Current usage
- Billing cycle
- Next billing date

### Usage

Track your consumption:

- **Conversations this month:** 1,450 / 2,000
- **Team members:** 3 / 10
- **Agents:** 2 / 5
- **Storage used:** 450 MB / 5 GB

**Approaching limits?**

- Get notified at 80% usage
- Consider upgrading plan
- Or optimize to reduce usage

### Payment Methods

**Adding a payment method:**

1. Go to **Settings** → **Billing**
2. Click **Add Payment Method**
3. Enter card information
4. Click **Save**

**Updating payment:**

- Update card details
- Change billing address
- Switch payment methods

### Invoices

**Viewing invoices:**

- See all past invoices
- Download as PDF
- Email to accounting

**Invoice details include:**

- Date and amount
- Services used
- Tax information
- Payment method

### Upgrading/Downgrading

**To change your plan:**

1. Go to **Settings** → **Billing**
2. Click **Change Plan**
3. Compare available plans
4. Select new plan
5. Confirm change

**Prorated billing:**

- Upgrades: Charged difference immediately
- Downgrades: Credit applied to next bill

## Integrations Settings

### Manage Integrations

View and configure all connected integrations:

- Connection status
- Last sync time
- Configuration options
- Usage statistics

**Per-integration settings:**

- Enable/disable
- Webhook URLs
- API credentials
- Sync preferences
- Notification settings

### Webhooks

Set up webhooks to notify your systems:

**Creating a webhook:**

1. Go to **Settings** → **Integrations** → **Webhooks**
2. Click **Add Webhook**
3. Enter webhook URL
4. Select events to trigger:
   - New conversation
   - Conversation resolved
   - Message received
   - Escalation created
5. Save

**Testing webhooks:**

- Send test payload
- View delivery history
- Check failure logs
- Retry failed deliveries

## Advanced Settings

### Conversation Settings

**Auto-close conversations:**

- Close after X hours of inactivity
- Options: 24h, 48h, 72h, Never
- Helps keep queue clean

**Message approval:**

- Require approval before sending (test mode)
- Perfect for training and quality control
- Can enable per agent

**Initial greeting:**

- Default message customers see first
- Customizable per agent
- Can include rich media

### Agent Behavior

**Global settings for all agents:**

**Confidence threshold:**

- How confident agent must be to answer
- Lower = answers more, may be less accurate
- Higher = answers less, but more accurate
- Default: 75%

**Escalation triggers:**

- Negative sentiment detection
- Specific keywords trigger human escalation
- Customer frustration indicators

**Response style:**

- Maximum response length
- Use of emojis
- Formatting preferences

### Notifications

**System notifications:**

- Queue waiting time alerts
- Failed integrations
- High error rates
- Unusual activity

**Performance notifications:**

- Daily performance summary
- Weekly trends report
- Monthly executive summary

**Team notifications:**

- New team member joined
- User role changed
- Settings updated

## Workspace Appearance

### Branding (if available in your plan)

**Logo and colors:**

- Upload your company logo
- Set primary brand color
- Set secondary colors
- Customize chat widget appearance

**Email templates:**

- Customize email notifications
- Add your branding
- Edit footer information

## Import/Export

### Exporting Data

**Bulk export options:**

- All conversations
- All analytics
- Customer data
- Agent configurations
- Documents

**Export formats:**

- JSON (for developers)
- CSV (for analysis)
- PDF (for reports)

### Importing Data

**Bulk import:**

- Customer data
- Historical conversations
- Documents
- FAQs

**Import formats accepted:**

- CSV
- JSON
- XML
- Text files

## Organization Management

### Multiple Organizations

If you belong to multiple organizations:

- Switch between organizations
- See which is currently active
- Each has separate settings

### Organization Transfer

**Transferring ownership:**

1. Must be current owner
2. Go to **Settings** → **Organization**
3. Click **Transfer Ownership**
4. Enter new owner's email
5. They must accept transfer

### Deleting Organization

> ⚠️ **Warning:** This is permanent and irreversible!

**To delete your organization:**

1. Must be owner
2. Export any data you need
3. Go to **Settings** → **Organization**
4. Click **Delete Organization**
5. Type organization name to confirm
6. All data will be permanently deleted

## Common Settings Questions

### Who can access settings?

- **Owners and Admins:** Full access
- **Agents:** Limited to personal settings
- **Viewers:** Read-only for most settings

### Will changing settings affect active conversations?

Most changes apply to new conversations only. Some (like agent instructions) apply immediately to all conversations.

### How do I reset a setting to default?

Click the **Reset to Default** button next to the setting.

### Can I undo changes?

Most settings can be changed back. However, some actions (like deleting data or downgrading plans) cannot be undone.

### Where do I find my organization ID?

Go to **Settings** → **Organization** → **Details**. Useful for API integrations and support.

## Next Steps

- Set up [Integrations](/docs/user-guide/integrations/) after configuring settings
- Review [Security best practices](/docs/user-guide/settings/) for your organization
- Configure [Privacy settings](/docs/user-guide/settings/) for compliance
- Invite your team members to get started
