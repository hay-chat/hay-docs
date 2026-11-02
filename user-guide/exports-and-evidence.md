---
layout: docs.njk
title: Exports, Evidence Packs and Reviewing AI Replies
description: Export a conversation as PDF or CSV, build an evidence pack for a payment dispute, download your whole workspace, and see how confident the AI was in each reply.
section: user-guide
---

## Export a conversation

Open any conversation and use the export actions at the top: **Export as PDF** or **Export as CSV**. The file contains the conversation as Hay.chat stored it.

## Evidence pack for a payment dispute

When a customer disputes a payment, your payment provider asks for proof. An evidence pack puts every conversation about an order into one document.

1. Open **Evidence packs** in the sidebar, or choose **Evidence pack for a dispute** from a conversation.
2. Search by **order number**. Hay.chat finds orders it looked up in your store or that a message named (for example "#1001"), or you can build the pack from one conversation only.
3. Pick the **dispute reason** (general, item not received, item not as described or defective, cancelled order or credit not processed). This decides which store actions and policies the summary puts first. The full conversation is always included.
4. Download a **PDF**, or a **ZIP**. The ZIP adds the attachments, a README and a SHA-256 manifest so anyone can check that no file was changed.

Good to know:

- Large images may be left out of a PDF to keep it under 4 MB (some payment providers reject bigger files). They are in the ZIP.
- A pack is a record of what Hay.chat stored. It does not guarantee any dispute outcome; each provider decides what evidence it accepts.
- Conversations older than your [data retention](/docs/user-guide/data-retention/) period are anonymized and cannot be in a pack. Disputes can arrive months after an order, so a short retention leaves you less evidence.

### Disputes reported by Stripe and Shopify Payments

The Evidence packs page lists disputes your connected payment platforms reported, with their due date. For a dispute with a known order you can **Attach evidence**: Hay.chat uploads the PDF to the dispute on the platform as a draft and adds a short note.

Hay.chat **never submits** the dispute response. Some platforms (Shopify Payments, for example) send saved evidence automatically on the due date, so review it on the platform before then. If the dispute already holds files you added there, Hay.chat does not replace them; download the pack and add it yourself. Reconnect the integration if it asks for new permissions.

Hay.chat alerts the owner when a new dispute arrives and reminds them again before it is due.

## See how confident the AI was

On each AI reply in a conversation, a badge shows a confidence percentage (and a mark when the reply was rechecked with more documents). Open the reasoning view of a reply to see the grounding, retrieval and certainty scores behind it.

When a reply was **replaced by the fallback message** because confidence was too low, the conversation shows "Low Confidence Response (Replaced with Fallback)". Expand it to compare the **original AI response** with the **fallback message sent to the customer**. For replies held for review, the reviewer also sees the original AI reply while editing. You can tune the thresholds under **Settings** → **Workspace** → **General** (see [Settings](/docs/user-guide/settings/#ai-confidence-guardrails)).

## Download your whole workspace

Go to **Settings** → **Privacy** → **Workspace Data** (Admins and Owners) and click **Prepare export**. Hay.chat builds one ZIP with everything the workspace keeps: conversations and messages, customers, documents, playbooks and agent settings.

- Each kind of data is a JSON Lines file; conversations, messages and customers are also CSV.
- Files sent in chats are included, up to 1.5 GB in total.
- Large workspaces can take a few minutes. The page updates when the export is ready and shows how long the download stays available. Each download is logged.

This is separate from your personal data export described in [Settings](/docs/user-guide/settings/#export-your-data), and from customer GDPR requests under **Customer Privacy**.
