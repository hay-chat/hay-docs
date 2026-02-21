---
layout: docs.njk
title: Analytics & Performance Metrics
description: Track your AI agent's performance, understand customer behavior, and make data-driven decisions to improve your support operations.
section: user-guide
---

## Accessing Analytics

Click **Analytics** in the left sidebar to view your performance dashboard.

## Overview Dashboard

Your analytics home shows the most important metrics at a glance.

### Key Performance Indicators (KPIs)

**Total Conversations**

- How many customer interactions you've had
- Tracks growth over time
- Filter by date range to see trends

**Resolution Rate**

- Percentage of conversations solved without human intervention
- **Good:** 70-85%
- **Excellent:** 85%+
- **Needs work:** Below 60%

**Average Response Time**

- How quickly customers get their first answer
- **Excellent:** Under 3 seconds
- **Good:** 3-10 seconds
- **Acceptable:** 10-30 seconds

**Customer Satisfaction**

- Based on customer ratings (👍 thumbs up/down)
- Shown as percentage of positive ratings
- **Excellent:** 90%+
- **Good:** 75-90%
- **Needs improvement:** Below 75%

**Escalation Rate**

- Percentage of conversations requiring human help
- **Excellent:** Under 15%
- **Good:** 15-30%
- **High:** Over 30% (consider more training)

## Conversation Analytics

Dive deeper into conversation data.

### Conversation Volume

**Daily/Weekly/Monthly views:**

- Line chart showing conversation volume over time
- Identify busy periods
- Plan staffing accordingly
- Spot unusual spikes or drops

**By Channel:**

- Which channels get most traffic?
- Web chat vs WhatsApp vs Email
- Allocate resources appropriately

**By Time of Day:**

- When are customers most active?
- Peak hours vs quiet periods
- Plan support coverage
- Consider time zone differences

### Conversation Outcomes

**Resolved Successfully:**

- AI handled completely
- Customer got their answer
- No escalation needed
- ✅ This is your goal!

**Escalated to Human:**

- Needed human intervention
- See reasons for escalation
- Identify training opportunities

**Abandoned:**

- Customer left before resolution
- May indicate frustration
- Could signal response time issues

**Still Open:**

- Active conversations
- Waiting for response
- In progress

### Conversation Duration

**Average conversation length:**

- How many messages exchanged?
- How long did it take?
- Shorter usually better (faster resolution)

**Duration by Topic:**

- Simple questions: 1-3 messages
- Complex issues: 5-10 messages
- Very complex: 10+ messages

Use this to:

- Identify areas needing better documentation
- Find topics that consistently take longer
- Optimize agent instructions

## Agent Performance

Track how each AI agent is performing.

### Per-Agent Metrics

For each agent, see:

- **Conversations handled** - Volume
- **Resolution rate** - Success without escalation
- **Response time** - Speed
- **Customer satisfaction** - Ratings
- **Most common topics** - What they handle most

### Comparing Agents

If you have multiple agents:

- Which performs best?
- Which needs improvement?
- Which handles specific topics better?
- Should you consolidate or specialize?

### Agent Improvement Tracking

**Week over week:**

- Is resolution rate improving?
- Are response times faster?
- Are customers more satisfied?

Track impact of changes:

- Added new documents → Resolution rate up?
- Updated instructions → Better responses?
- New playbook → Fewer escalations?

## Topic & Intent Analysis

Understand what customers are asking about.

### Top Topics

See most common conversation topics:

1. Order status (35% of conversations)
2. Shipping questions (20%)
3. Return requests (15%)
4. Product information (12%)
5. Account issues (10%)
   ...

**Use this to:**

- Prioritize documentation efforts
- Create targeted playbooks
- Staff appropriately for common issues
- Product/service improvements

### Intent Detection

What do customers want?

- Information seeking (most common)
- Problem solving
- Transaction/action needed
- Complaint/frustration

### Trending Topics

**Rising:**

- Topics increasing in frequency
- May indicate new product launch
- Or emerging issue needing attention

**Falling:**

- Topics decreasing
- May indicate successful resolution
- Or reduced interest

## Time-Based Analytics

### Response Time Analysis

**First response time:**

- How long until customer gets first reply
- Goal: Under 5 seconds for AI

**Average response time:**

- Throughout the conversation
- Should stay consistently fast

**Resolution time:**

- Total time from start to resolution
- Lower is usually better (unless complex issue)

### Peak Times

**Busiest hours:**

- When do most conversations happen?
- Plan team coverage
- Ensure AI is performing well during peaks

**Quietest times:**

- Best for maintenance
- Good for training updates
- Review and optimization time

### Day of Week Patterns

**Weekday vs Weekend:**

- Different volumes?
- Different types of questions?
- Adjust staffing and AI settings

## Customer Satisfaction Metrics

### Rating System

Customers can rate messages with:

- 👍 Helpful/good response
- 👎 Not helpful/poor response

### Satisfaction Trends

Track over time:

- Is satisfaction improving?
- Which responses get best ratings?
- Which get worst ratings?

### Feedback Comments

When customers leave feedback:

- Read the comments
- Identify patterns
- Address common complaints
- Learn from praise

### Net Promoter Score (NPS)

If you ask "How likely are you to recommend us?":

- Score of 9-10: Promoters
- Score of 7-8: Passives
- Score of 0-6: Detractors

**NPS = % Promoters - % Detractors**

## Source Performance

### Document Usage

Which documents are referenced most?

- High usage = Very valuable
- Zero usage = Maybe not needed or poorly tagged
- High usage + low ratings = Needs improvement

**For each document:**

- Times referenced
- Success rate when used
- Customer satisfaction
- Last used date

**Use this to:**

- Identify valuable documents
- Find gaps in coverage
- Update or remove unused docs
- Improve unclear documents

### Playbook Performance

For each playbook:

- Times triggered
- Completion rate
- Resolution rate
- Average duration
- Customer satisfaction

**Insights:**

- Which playbooks work best?
- Which need improvement?
- Which are underutilized?
- Should any be retired?

## Channel Performance

### By Channel Comparison

Compare metrics across channels:

| Metric       | Web Chat | WhatsApp | Email   |
| ------------ | -------- | -------- | ------- |
| Volume       | 500/day  | 200/day  | 100/day |
| Resolution   | 82%      | 75%      | 88%     |
| Satisfaction | 89%      | 85%      | 92%     |
| Avg Response | 3s       | 4s       | 2s      |

**Insights:**

- Which channels are most efficient?
- Where should you focus improvements?
- Are expectations different by channel?

## Custom Reports

### Creating a Custom Report

1. Go to **Analytics** → **Reports**
2. Click **Create Report**
3. Choose metrics to include
4. Select date range
5. Add filters (agent, channel, topic)
6. Choose visualizations (charts, tables)
7. Save and generate

### Report Types

**Summary Report:**

- High-level overview
- Key metrics only
- Great for executives

**Detailed Analysis:**

- Deep dive into specific area
- Multiple metrics and dimensions
- For operations team

**Performance Report:**

- Agent and playbook performance
- Identify training needs
- Quality assurance

**Trends Analysis:**

- Changes over time
- Seasonal patterns
- Growth tracking

### Scheduling Reports

Set up automatic reports:

1. Create your custom report
2. Click **Schedule**
3. Choose frequency (daily, weekly, monthly)
4. Select recipients
5. Choose format (PDF, CSV, dashboard link)
6. Save schedule

**Common schedules:**

- Daily summary report (every morning)
- Weekly performance review (Monday mornings)
- Monthly executive summary (first of month)

## Exporting Data

### Export Options

**PDF Report:**

- Professional formatted document
- Charts and graphs included
- Great for presentations

**CSV Export:**

- Raw data for analysis
- Import into Excel/Google Sheets
- Custom calculations

**Interactive Dashboard:**

- Share live dashboard link
- Updates automatically
- Collaborative analysis

### What You Can Export

- All conversations (with filters)
- Performance metrics
- Customer satisfaction data
- Agent statistics
- Topic analysis
- Time-based data

## Interpreting Your Data

### Understanding Trends

**Improving metrics:**

- ✅ What changed recently?
- ✅ What should you keep doing?
- ✅ Can you apply this to other areas?

**Declining metrics:**

- ❌ What changed?
- ❌ External factors? (seasonality, marketing campaign)
- ❌ What needs to be fixed?

### Red Flags to Watch

**Resolution rate dropping:**

- New types of questions?
- Agent needs more training?
- Documentation outdated?

**Response time increasing:**

- Too many conversations?
- Need to optimize?
- Technical issues?

**Satisfaction decreasing:**

- Read recent conversations
- What's frustrating customers?
- Quick wins to improve?

**Escalation rate rising:**

- Why are more going to humans?
- Can AI handle these?
- Need new playbooks?

## Using Analytics to Improve

### Weekly Review Routine

**Every week, check:**

1. Key metrics vs last week
2. Any unusual spikes or drops?
3. Read 5-10 recent conversations
4. Check top escalation reasons
5. Review lowest-rated responses
6. Identify one thing to improve

### Monthly Deep Dive

**Every month:**

1. Full trend analysis
2. Compare to previous months
3. Review all agents and playbooks
4. Identify documentation gaps
5. Plan next month's improvements
6. Set specific goals

### Setting Goals

**Example goals:**

- Increase resolution rate from 75% to 80%
- Reduce escalation rate by 10%
- Improve satisfaction score to 90%
- Cut response time to under 3 seconds

**Track progress:**

- Monitor weekly
- Adjust tactics if not improving
- Celebrate when goals reached!

## Common Analytics Questions

### Why do my numbers seem low at first?

Brand new agents need time to learn and optimize. Expect metrics to improve over 2-4 weeks as you:

- Add more documents
- Refine playbooks
- Adjust agent instructions

### What's a "good" resolution rate?

Depends on your industry and complexity, but generally:

- **Simple FAQ-based support:** 80-90%
- **E-commerce support:** 70-85%
- **Technical support:** 60-75%
- **Complex B2B:** 50-70%

### Should I worry about negative feedback?

Some negative ratings are normal. Focus on:

- Overall trends (improving or declining?)
- Patterns in negative feedback
- Opportunities for improvement

If satisfaction is 80%+, you're doing well!

### How often should I check analytics?

**Daily:** Quick glance at key metrics (5 min)
**Weekly:** Deeper review and planning (30 min)
**Monthly:** Comprehensive analysis (2 hours)

### Can I compare my performance to others?

Industry benchmarks coming soon! For now, compare to your own historical performance.

## Next Steps

- Use insights to improve your [Agents](/docs/user-guide/agents/)
- Create better [Playbooks](/docs/user-guide/playbooks/) based on data
- Add [Documents](/docs/user-guide/documents/) for low-performing topics
- Monitor your [Conversations](/docs/user-guide/conversations/) for escalation patterns
