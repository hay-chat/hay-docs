---
layout: docs.njk
title: Data Retention & Privacy
description: How Hay automatically manages conversation data retention, anonymization, and GDPR compliance.
section: user-guide
---

## Overview

Hay includes a built-in data retention system that automatically removes personal information from old conversations. This helps you comply with GDPR and other data protection regulations while preserving anonymized metadata for analytics.

New organizations start with a retention period of **365 days**. You can shorten it, or choose **Disabled (keep forever)**. Once a period is set, Hay handles cleanup automatically. Organizations created earlier may still be set to keep conversations forever; check the page below.

## How It Works

When a conversation passes the retention window, Hay **anonymizes** it rather than deleting it outright. This means:

**Removed (personal data):**

- All messages in the conversation
- Customer link (who the conversation was with)
- Conversation title and summary
- Context, metadata, and resolution metadata
- Media files and attachments
- Linked document references
- Associated vector embeddings
- Related records (action logs, pending actions, team questions, notification deliveries, and order links)

**Preserved (for analytics):**

- Timestamps (created, closed, updated)
- Conversation status and channel
- Organization ID

After anonymization, the conversation record remains with the title `[Anonymized]` and a `deleted_at` timestamp. This lets you keep aggregate metrics (resolution times, volume trends, channel distribution) without retaining any personal data.

## Configuring Retention

1. Go to **Settings** → **Customer Privacy** (under the Privacy section)
2. Under **Conversation Retention Period**, choose a timeframe: Disabled (keep forever), 30, 60, 90, 180, or 365 days (365 days is marked recommended)
3. Click **Save Retention Policy**

The retention period counts from when a conversation was **closed** (`closed_at`). If a conversation's `closed_at` is not set, the system falls back to its creation date. Only conversations with status `closed`, `resolved`, or `unresolved` are eligible for anonymization. Open or in-progress conversations are never touched.

## Legal Hold

Sometimes you need to preserve a specific conversation regardless of your retention policy — for example, during legal proceedings or an active dispute.

**To place a conversation on legal hold:**

1. Open the conversation
2. Use the legal hold toggle to enable it

Conversations on legal hold are **exempt from automatic anonymization**, even if they exceed the retention period. They remain fully intact until the hold is removed.

## Cleanup Schedule

The retention job runs automatically once per day (at 3:00 AM server time). Each run:

1. Finds all organizations with a retention policy configured
2. Identifies closed, resolved, or unresolved conversations past the retention window
3. Skips conversations on legal hold
4. Deletes associated embeddings from the vector store
5. Deletes media files and attachments
6. Deletes all messages and related records
7. Anonymizes the conversation record
8. Writes an entry to the audit log
9. Sweeps expired customers — customer records with no remaining live conversations past the retention window are deleted
10. Expires old insight text (at 2× the retention period)

If an error occurs while processing one organization, the job continues with the remaining organizations.

## Audit Trail

Every retention cleanup is logged in the audit trail with:

- Number of conversations anonymized
- Number of messages deleted
- Number of embeddings deleted
- Number of media files deleted
- Number of media file deletion failures
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
You can initiate customer data export requests via **Settings** → **Privacy** → **Customer Privacy**. The export is delivered to the customer's email with a secure download link. If a conversation has already been anonymized, it will not appear in the export.

**Are embeddings cleaned up too?**
Yes. All vector embeddings linked to anonymized conversations are deleted, ensuring no semantic traces of the conversation remain in the vector store.
