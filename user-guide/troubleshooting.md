---
layout: docs.njk
title: Troubleshooting Common Issues
description: Having issues with Hay? This guide covers common problems and their solutions.
section: user-guide
---

## Assistant Not Responding

### Symptoms

- Customer messages but no reply
- Long delays before response
- "Processing..." message stuck

### Solutions

**Check whether the AI is paused or off:**

1. Open the **Inbox**. If it says **AI paused**, click **Resume AI** (see [Team alerts, pause and business hours](/docs/user-guide/team-alerts-and-hours/))
2. Go to **Settings** → **Privacy** → **Customer Privacy** → **AI on your channels** and check the channel's **AI replies** is **Send automatically**, not **Drafts only** or **Off**
3. Check **Settings** → **Billing & Usage** → **Usage**: if the plan's resolutions are used up and extra resolutions are off, the AI is paused and conversations wait for your team
4. Check the assistant has the documents and playbooks it needs

**Check integration:**

1. Go to **Settings** → **Channels** → **Channel list** (for channel plugins like WhatsApp, Email) or **Integrations** → **Marketplace** (for data plugins like Shopify, Zendesk)
2. Open the integration and verify its status is **Connected** (a failing channel shows **This channel is failing**)
3. Test with a sample message

**Check conversation status:**

1. Open the conversation
2. Check if status is "Open" (not "Closed", "Manual Control" or "Awaiting Approval")
3. Check for error messages in conversation

**If still not working:**

- Try creating a test conversation
- Check system status (if available)
- Contact support with conversation ID

## Poor Response Quality

### Symptoms

- The assistant gives wrong answers
- Responses are vague or unhelpful
- Frequent "I don't know" messages
- Information is outdated

### Solutions

**Add better documentation:**

1. Go to **Documents**
2. Upload FAQs covering the topics
3. Make sure information is clear and complete
4. Update outdated documents

**Improve the assistant's instructions:**

1. Go to **Settings** → **Assistant** → **Identity**
2. Review instructions
3. Add specific examples
4. Be more explicit about what to do/not do

**Create targeted playbooks:**

1. Identify which questions cause issues
2. Create playbooks for those scenarios
3. Include step-by-step instructions

**Review and learn:**

1. Read recent conversations
2. See where the assistant struggles (**Quality** → **Insights** lists the patterns)
3. Add documents to fill knowledge gaps

## High Escalation Rate

### Symptoms

- Too many conversations landing in the Inbox
- The assistant frequently says it can't help
- Lots of "transfer to human" requests

### Solutions

**Identify why escalations happen:**

1. Go to **Conversations** and filter by **Escalated** status (this shows conversations requiring human attention, including those with pending-human and human-took-over statuses)
2. Review escalation reasons
3. Look for patterns

**Common causes and fixes:**

**Cause: Low confidence**

- Solution: Add more detailed documents
- Include more examples
- Make instructions clearer

**Cause: Specific topics**

- Solution: Create playbooks for those topics
- Add focused documentation
- Update the assistant's instructions

**Cause: Customer frustration**

- Solution: Add empathy guidance to the assistant's instructions
- Respond faster
- Offer alternatives before escalating

**Adjust confidence threshold:**

1. Go to **Settings** → **Workspace** → **General** → **AI Confidence Guardrails**
2. Lower the confidence threshold slightly
3. Monitor results
4. Adjust as needed

## Integration Problems

### Integration Shows "Disconnected"

**Solutions:**

1. **Re-authenticate:**

   - Go to **Integrations**
   - Go to the plugin's settings page and reconfigure the connection, or disable and re-enable the integration
   - Sign in again if prompted
   - Authorize permissions

2. **Check credentials:**

   - Verify API keys haven't expired
   - Ensure passwords haven't changed
   - Confirm account is still active

3. **Verify permissions:**
   - Check that app still has necessary permissions
   - Re-authorize if needed

### Messages Not Syncing

**For channel integrations (WhatsApp, Email, etc.):**

1. **Check the channel status:**

   - Go to **Channels** → Your channel
   - Confirm the connection shows **Connected**
   - Reconnect if the status looks off

2. **Test connection:**

   - Send test message
   - Check if it appears in Hay
   - Verify bidirectional sync

3. **Check filters:**
   - Ensure no rules are blocking messages
   - Verify routing is configured correctly

**For data integrations (Shopify, Zendesk, etc.):**

1. **Force re-sync:**

   - Go to integration settings
   - Click **Sync Now**
   - Wait for completion

2. **Check last sync time:**

   - Should be recent
   - If old, investigate why sync stopped

3. **Verify API limits:**
   - Some platforms have rate limits
   - May need to wait before retry

### Web Chat Widget Not Appearing

**Solutions:**

1. **Check the install status:**

   - Go to **Channels** → **Webchat** → **Install**
   - **Working** means the chat has loaded on a page of your site; the line next to it says where and when
   - **Not installed yet** means no page with the chat has loaded yet
   - **Off** means **Show on your website** is switched off. Turn it on and click **Save**

2. **Verify embed code:**

   - Copy the code again from the **Install** tab
   - Check code is placed before `</body>` tag on every page
   - Ensure no extra characters or spaces
   - Verify the script URL is correct

3. **Check conflicts:**

   - Other chat widgets may interfere
   - Check browser console for JavaScript errors
   - Test on different browser

4. **Cache issues:**

   - Clear browser cache
   - Hard refresh (Ctrl+Shift+R or Cmd+Shift+R)
   - Try incognito/private browsing

5. **Domain restrictions:**
   - On the **Install** tab, check **Allowed websites**
   - Add your domain (one per line, `*.shop.com` for every subdomain) and click **Save**

See the [Webchat](/docs/user-guide/webchat/) guide for all settings.

## Login & Access Issues

### Can't Log In

**Solutions:**

1. **Check credentials:**

   - Verify email is correct
   - Check for typos in password
   - Try copy-pasting password

2. **Reset password:**

   - Click **Forgot Password**
   - Check email for reset link
   - Check spam folder if not received
   - Create new strong password

3. **Account status:**

   - Ensure account isn't deactivated
   - Contact organization admin
   - Verify email is verified

4. **Two-factor issues:**
   - If your organization requires two-factor authentication, sign-in asks for a 6-digit code from your authenticator app. If you lost your device, enter one of your recovery codes instead (each works once). If you have neither, ask an Owner or Admin for help.

### Access Denied / Permission Error

**Solutions:**

1. **Check your role:**

   - Go to **Settings** → **Workspace** → **Users**
   - Verify your role and permissions
   - Contact admin if permissions need changing

2. **Organization context:**
   - Ensure you're in the right organization
   - Switch organizations if needed
   - Verify org is active

## Slow Performance

### Dashboard Loads Slowly

**Solutions:**

1. **Check date range:**

   - Loading lots of data can be slow
   - Try shorter date range
   - Use filters to narrow results

2. **Browser issues:**

   - Clear cache and cookies
   - Update to latest browser version
   - Try different browser
   - Disable browser extensions temporarily

3. **Network:**
   - Check internet connection
   - Try different network if possible
   - Check if other sites are slow too

### Assistant Response Delays

**Solutions:**

1. **Simplify the setup:**

   - Reduce instruction complexity
   - Remove unnecessary playbooks
   - Consolidate documents

2. **Optimize documents:**

   - Remove duplicate information
   - Break large documents into smaller ones
   - Better organization and tagging

3. **Check system load:**
   - High conversation volume?
   - Peak usage times may be slower
   - Contact support if consistently slow

## Data & Analytics Issues

### Numbers Don't Look Right

**Solutions:**

1. **Check filters:**

   - Verify date range is correct
   - Reset filters and try again

2. **Cache delay:**

   - Some metrics update every few minutes
   - Try refreshing page
   - Wait 5-10 minutes and check again

3. **Definition confusion:**
   - Read metric definitions
   - Understand what's being measured
   - Compare with raw data if needed

### Can't Export Data

**Note:** The Dashboard has no export. You can export a conversation as PDF or CSV, the Feedback list (**Export CSV**) and the Usage resolution ledger (**Export CSV**).

## Notifications Not Working

### Not Receiving Emails

**Solutions:**

1. **Check settings:**

   - Alerts for conversations that need a human also go by email to available team members, after the push alerts
   - Verify your SMTP configuration is correct in the server environment

2. **Check spam:**

   - Look in spam/junk folder
   - Mark Hay emails as "Not Spam"
   - Add to safe senders list

3. **Email address:**

   - Verify email is correct in profile
   - Update if needed
   - Save changes

4. **Email service issues:**
   - Try sending test notification
   - Contact support if tests fail

### Not Getting Alerts When a Conversation Needs a Human

**With the dashboard open** you should hear a sound and see a toast. If not, check that the browser tab is allowed to play sound and show notifications.

**With the dashboard closed**, alerts only arrive on devices where **Alerts on this device** is on. Check:

1. **Is it on for this device?**

   - Go to **Settings** → **My Profile** (**Alerts on this device**), or open the Inbox
   - The switch must be on for _this_ browser or phone; it is per device, not per account
   - Use **Send a test alert** to confirm delivery

2. **Browser permission:**

   - If the switch says alerts are blocked, notifications were denied for the Hay site
   - Allow them again in the browser's site settings (the lock icon next to the address), then turn the switch on

3. **iPhone or iPad:**

   - Add Hay to your Home Screen (Share → **Add to Home Screen**) and open it from that icon
   - Safari tabs cannot receive alerts; only the installed Home Screen app can (iOS 16.4 or later)

4. **Do Not Disturb / Focus:**

   - Check the system Do Not Disturb or Focus settings
   - Temporarily disable and send a test

5. **Server configuration (self-hosted):**
   - The server needs `VAPID_PUBLIC_KEY`, `VAPID_PRIVATE_KEY` and `VAPID_SUBJECT` set; without them the switch is not shown

## Document Problems

### Documents Not Being Used

**Symptoms:**

- The assistant says "I don't know" despite having relevant docs
- Document shows zero usage
- Responses don't reference documents

**Solutions:**

1. **Check document format:**

   - Is text readable?
   - Try re-uploading
   - Convert PDFs to text if needed

2. **Improve content:**

   - Make sure information is clear
   - Use complete sentences
   - Add context and examples

3. **Better tags:**

   - Add relevant tags
   - Use keywords customers use
   - Organize by topic

4. **Test search:**
   - Try searching for document content
   - If you can't find it, neither can the assistant
   - Improve titles and descriptions

### Upload Fails

**Solutions:**

1. **Check file size:**

   - Maximum file size limits
   - Compress large files
   - Upload in smaller chunks

2. **Check file type:**

   - Ensure format is supported
   - Convert to PDF or TXT if needed

3. **Network issues:**
   - Retry upload
   - Check connection
   - Try different network

## Getting More Help

### Before Contacting Support

**Gather this information:**

1. **What happened:**

   - Specific error message
   - When did it start?
   - Does it happen every time?

2. **What you tried:**

   - Steps already taken
   - What worked/didn't work

3. **Context:**
   - Conversation ID (if relevant)
   - Integration name
   - Browser and OS version

### How to Contact Support

**Email:**

- support@hay.ai
- Include "URGENT" in subject if critical

**In-App:**

- Click **Help** icon
- Start chat with support team
- Available during business hours

**What to include:**

1. Clear description of problem
2. Steps to reproduce
3. Screenshots if helpful
4. Conversation ID
5. When problem started
6. What you've tried

### Expected Response Times

| Priority                    | Response Time |
| --------------------------- | ------------- |
| Critical (service down)     | 1 hour        |
| High (major feature broken) | 4 hours       |
| Medium (minor issue)        | 24 hours      |
| Low (question, enhancement) | 48 hours      |

## Preventive Maintenance

### Regular Checks

**Weekly:**

- Review conversation quality
- Check integration status
- Monitor key metrics

**Monthly:**

- Update documents
- Review and optimize the assistant's instructions and playbooks
- Clean up unused playbooks
- Check team access

**Quarterly:**

- Full system review
- Update all documentation
- Review security settings
- Assess performance trends

### Best Practices

1. **Keep documents current**

   - Update as products/policies change
   - Remove outdated information
   - Regular content review

2. **Monitor metrics**

   - Watch for trends
   - Address issues early
   - Celebrate improvements

3. **Regular training**

   - Review recent conversations
   - Find areas for improvement
   - Update the assistant instructions

4. **Stay organized**
   - Clear naming conventions
   - Good tagging system
   - Archive old/unused items

## Common Error Messages

### "Conversation locked"

**Cause:** Another user or process is handling it
**Fix:** Wait a moment and try again

### "Rate limit exceeded"

**Cause:** Too many requests too quickly
**Fix:** Wait a few minutes and retry

### "Invalid credentials"

**Cause:** Authentication issue
**Fix:** Re-login or check API tokens

### "Webhook delivery failed"

**Cause:** Integration endpoint unavailable
**Fix:** Check integration settings and retry

## Still Need Help?

If you've tried these solutions and still have issues:

1. Check our [FAQ](/docs/user-guide/faq/) for quick answers
2. Visit the [Hay Status Page](https://status.hay.ai) for system status
3. Contact support with details about your issue
4. Join our community forum for peer help

We're here to help you succeed with Hay!
