---
layout: docs.njk
title: Troubleshooting Common Issues
description: Having issues with Hay? This guide covers common problems and their solutions.
section: user-guide
---

## Agent Not Responding

### Symptoms

- Customer messages but no reply
- Long delays before response
- "Processing..." message stuck

### Solutions

**Check agent status:**

1. Go to **Agents**
2. Verify agent is **Enabled**
3. Check if agent has required documents and playbooks

**Check integration:**

1. Go to **Integrations**
2. Verify connection status is **Connected**
3. Test with a sample message

**Check conversation status:**

1. Open the conversation
2. Check if status is "Open" (not "Closed" or "Manual Control")
3. Check for error messages in conversation

**If still not working:**

- Try creating a test conversation
- Check system status (if available)
- Contact support with conversation ID

## Poor Response Quality

### Symptoms

- Agent gives wrong answers
- Responses are vague or unhelpful
- Frequent "I don't know" messages
- Information is outdated

### Solutions

**Add better documentation:**

1. Go to **Documents**
2. Upload FAQs covering the topics
3. Make sure information is clear and complete
4. Update outdated documents

**Improve agent instructions:**

1. Go to **Agents** → Your agent
2. Review instructions
3. Add specific examples
4. Be more explicit about what to do/not do

**Create targeted playbooks:**

1. Identify which questions cause issues
2. Create playbooks for those scenarios
3. Include step-by-step instructions

**Review and learn:**

1. Read recent conversations
2. See where agent struggles
3. Add documents to fill knowledge gaps

## High Escalation Rate

### Symptoms

- Too many conversations going to queue
- Agent frequently says it can't help
- Lots of "transfer to human" requests

### Solutions

**Identify why escalations happen:**

1. Go to **Conversations** and filter by **Needs Attention** status
2. Review escalation reasons in individual conversations
3. Look for patterns

**Common causes and fixes:**

**Cause: Low confidence**

- Solution: Add more detailed documents
- Include more examples
- Make instructions clearer

**Cause: Specific topics**

- Solution: Create playbooks for those topics
- Add focused documentation
- Update agent instructions

**Cause: Customer frustration**

- Solution: Train agent on empathy
- Respond faster
- Offer alternatives before escalating

**Adjust confidence threshold:**

1. Go to **Settings** → **General**
2. Lower confidence threshold slightly
3. Monitor results
4. Adjust as needed

## Integration Problems

### Integration Shows "Disconnected"

**Solutions:**

1. **Re-authenticate:**

   - Go to **Integrations**
   - Click **Reconnect**
   - Sign in again
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

1. **Check the integration status:**

   - Go to **Integrations** → Your integration
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

1. **Verify embed code:**

   - Check code is placed before `</body>` tag
   - Ensure no extra characters or spaces
   - Verify script URL is correct

2. **Check conflicts:**

   - Other chat widgets may interfere
   - Check browser console for JavaScript errors
   - Test on different browser

3. **Cache issues:**

   - Clear browser cache
   - Hard refresh (Ctrl+Shift+R or Cmd+Shift+R)
   - Try incognito/private browsing

4. **Domain restrictions:**
   - Verify domain is allowed
   - Check integration settings
   - Add domain if needed

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

4. **2FA issues:** (2FA is not yet available — this section will apply once the feature ships)
   - Ensure time is correct on device
   - Try backup codes
   - Contact support to reset 2FA

### Access Denied / Permission Error

**Solutions:**

1. **Check your role:**

   - Go to **Settings** → **Users**
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

### Agent Response Delays

**Solutions:**

1. **Simplify agent:**

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
   - Check channel filters
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

**Note:** Exporting data from Analytics is not yet available. The export button is currently a placeholder and does not produce a file. This feature is planned for a future release.

## Notifications Not Working

### Not Receiving Emails

**Solutions:**

1. **Check settings:**

   - Go to **Settings** → **General**
   - Verify email notifications are enabled
   - Check which events you're subscribed to

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

### Not Getting In-App Notifications

**Solutions:**

1. **Browser permissions:**

   - Check browser allows notifications
   - Enable if blocked
   - Restart browser

2. **Do Not Disturb:**

   - Check system DND settings
   - Temporarily disable
   - Test notifications

3. **Dashboard notifications:**
   - Check settings in Hay
   - Enable desired notification types
   - Save preferences

## Document Problems

### Documents Not Being Used

**Symptoms:**

- Agent says "I don't know" despite having relevant docs
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
   - If you can't find it, neither can the agent
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
   - Agent ID (if relevant)
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
4. Conversation or agent ID
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
- Review and optimize agents
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
   - Update agent instructions

4. **Stay organized**
   - Clear naming conventions
   - Good tagging system
   - Archive old/unused items

## Common Error Messages

### "Agent not found"

**Cause:** Agent was deleted or disabled
**Fix:** Check agent status or select different agent

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
