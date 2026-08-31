---
layout: docs.njk
title: Data Retention & Privacy
description: How Hay automatically manages conversation data retention, anonymization, and GDPR compliance.
section: user-guide
---

## Overview

Hay includes a built-in data retention system that automatically removes personal information from old conversations. This helps you comply with GDPR and other data protection regulations while preserving anonymized metadata for analytics.

Retention is **opt-in** — by default, conversations are kept indefinitely. Once you set a retention period, Hay handles cleanup automatically.

## How It Works

When a conversation passes the retention window, Hay **anonymizes** it rather than deleting it outright. This means:

**Removed (personal data):**

- All messages in the conversation
- Customer link (who the conversation was with)
- Conversation title
- Context and metadata
- Linked document references
- Associated vector embeddings

**Preserved (for analytics):**

- Timestamps (created, closed, updated)
- Conversation status and channel
- Agent assignment
- Organization ID

After anonymization, the conversation record remains with the title `[Anonymized]` and a `deleted_at` timestamp. This lets you keep aggregate metrics (resolution times, volume trends, channel distribution) without retaining any personal data.

## Configuring Retention

1. Go to **Settings** → **Customer Privacy**
2. Under **Conversation Retention Period**, choose a timeframe: Disabled (keep forever), 30, 60, 90, 180, or 365 days
3. Save your changes

The retention period counts from when a conversation was **closed**. Only conversations with status `closed` or `resolved` are eligible for anonymization. Open or in-progress conversations are never touched.

## Legal Hold

Sometimes you need to preserve a specific conversation regardless of your retention policy — for example, during legal proceedings or an active dispute.

**To place a conversation on legal hold:**

1. Open the conversation
2. Use the legal hold toggle to enable it

Conversations on legal hold are **exempt from automatic anonymization**, even if they exceed the retention period. They remain fully intact until the hold is removed.

## Cleanup Schedule

The retention job runs automatically once per day (at 3:00 AM UTC). Each run:

1. Finds all organizations with a retention policy configured
2. Identifies closed/resolved conversations past the retention window
3. Skips conversations on legal hold
4. Deletes associated embeddings from the vector store
5. Deletes all messages
6. Anonymizes the conversation record
7. Writes an entry to the audit log

If an error occurs while processing one organization, the job continues with the remaining organizations.

## Audit Trail

Every retention cleanup is logged in the audit trail with:

- Number of conversations anonymized
- Number of messages deleted
- Number of embeddings deleted
- The retention period that was applied
- The cutoff date used
- The list of affected conversation IDs

You can review these logs to verify that retention is operating as expected.

## FAQ

**What happens if I change the retention period?**
The new period takes effect on the next daily run. Conversations that already exceed the new window will be anonymized.

**Can I undo anonymization?**
No. Anonymization permanently removes personal data. This is by design — GDPR requires that deleted data cannot be recovered.

**Does retention affect analytics?**
Aggregate analytics (conversation volume, resolution times, channel breakdown) are preserved. Per-conversation details and message content are not.

**What about customer data exports?**
Data exports (via **Settings** → **Customer Privacy**) include all current data. If a conversation has already been anonymized, it will not appear in the export.

**Are embeddings cleaned up too?**
Yes. All vector embeddings linked to anonymized conversations are deleted, ensuring no semantic traces of the conversation remain in the vector store.
