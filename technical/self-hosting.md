---
layout: docs.njk
title: Self-Hosting Configuration
description: Environment variables for self-hosted Hay.chat installations - AI providers, EU-only processing, push alerts, telemetry and operator integrations.
section: technical
navGroup: Introduction
navOrder: 4
---

## Overview

This page lists the optional environment variables that change how a self-hosted Hay.chat behaves. The canonical, commented list is `.env.example` in the repository; copy it to `.env` and set only what you need. Database, Redis and JWT settings are required and described there.

Hay.chat runs with Docker, PostgreSQL 16 with pgvector, and Redis 7.

## AI providers

Chat models can come from OpenAI, Anthropic, Gemini, or any OpenAI-compatible endpoint (for example DigitalOcean Gradient, or a local server such as Ollama, which is untested). Embeddings use OpenAI `text-embedding-3-small`, so an OpenAI key is still required. No telemetry is sent unless you set `POSTHOG_KEY`.

| Variable | What it does |
| --- | --- |
| `OPENAI_API_KEY` | Required. Used for embeddings, voice-note transcription and, by default, chat. |
| `OPENAI_BASE_URL` | Base URL for every call made with `OPENAI_API_KEY`. Empty means `api.openai.com`. |
| `LLM_BASE_URL`, `LLM_VENDOR`, `LLM_API_KEY` | Move the default chat provider to another OpenAI-compatible host. Set all three. |
| `LLM_TIER_HARD`, `LLM_TIER_MEDIUM`, `LLM_TIER_EASY` | Override the model used for each task-difficulty tier on the default provider. |
| `OPENAI_EMBEDDING_MODEL`, `OPENAI_CHAT_MODEL`, `OPENAI_TRANSCRIPTION_MODEL`, `OPENAI_VISION_MODEL` | Model names. |

`LLM_VENDOR` picks how structured output is requested from the host:

- `openai`: strict JSON schema
- `custom-tools`: forced tool choice, for hosts that advertise tool calling
- `custom`: loose JSON that Hay.chat validates and repairs, for unknown hosts

The `LLM_*` settings are the default for calls with no organization and the fallback for organizations that have not chosen their own provider under **Settings** → **AI Models**. Embeddings ignore them and always use `OPENAI_API_KEY`, because the vector column and index are sized for one embedding model.

## EU-only AI processing

Organizations can turn on **Process AI in the EU only** under **Settings** → **AI Models**. Every chat, embedding and transcription call for that organization then goes to an EU endpoint and never to the default one.

| Variable | What it does |
| --- | --- |
| `OPENAI_EU_BASE_URL` | EU endpoint, for example `https://eu.api.openai.com/v1`. |
| `OPENAI_EU_API_KEY` | Key from an OpenAI project with EU data residency (OpenAI has to approve your account for this). |
| `OPENAI_EU_TIER_HARD`, `OPENAI_EU_TIER_MEDIUM`, `OPENAI_EU_TIER_EASY` | Optional model overrides for EU organizations. `LLM_TIER_*` does not apply to them. |

The switch only appears when both `OPENAI_EU_BASE_URL` and `OPENAI_EU_API_KEY` are set. **It fails closed**: if you remove these values later, AI replies, searches and transcripts for EU-only organizations pause instead of falling back to another region. Photos and PDFs sent by shoppers are still read on the default provider in that case, and the dashboard says so.

## Push alerts

`VAPID_PUBLIC_KEY`, `VAPID_PRIVATE_KEY` and `VAPID_SUBJECT` enable browser push alerts when a conversation needs a person. Generate a key pair with `npx web-push generate-vapid-keys`. `VAPID_SUBJECT` is a `mailto:` or `https:` URL identifying your deployment. Without these the Inbox says alerts are not set up. See [Team alerts](/docs/user-guide/team-alerts-and-hours/).

## Telemetry

Telemetry is off unless you turn it on. If `POSTHOG_KEY` is unset, no analytics script is loaded in the browser and the server never creates a PostHog client or makes an outbound request. Set `POSTHOG_KEY` (and optionally `POSTHOG_HOST`, which may be a reverse proxy that forwards the server ingestion paths) to enable dashboard analytics and server-side error reporting.

## Operator integration

Two optional settings let an external operator (for example a billing system) manage organizations:

- `INTERNAL_API_SECRET`: shared secret for the internal API used to suspend organizations or push access notices. Empty disables it.
- `OPERATOR_EVENTS_URL`: where core posts signed events, such as an organization being deleted. **Empty by default**, which means nothing is sent.

## Conversation timing

`CONVERSATION_INACTIVITY_INTERVAL` (milliseconds, default 1800000) sets when quiet conversations close: a warning at half the value, a close with a sign-off message at the value and a silent close at double. See [Billing and Usage](/docs/user-guide/billing-and-usage/#how-conversations-close-by-themselves).

## Other optional settings

- `TURNSTILE_SITE_KEY` and `TURNSTILE_SECRET_KEY`: Cloudflare Turnstile keys for the web chat bot protection plugin. Unset, no challenge is served.
- `UNDER_ATTACK_CREATE_THRESHOLD` and `UNDER_ATTACK_HOURS`: when one organization gets more new web chats than the threshold in five minutes (default 30), every visitor is challenged for the given hours (default 4). `0` disables the automatic trigger.
- `SMTP_FROM_EMAIL` and `RESEND_API_KEY`: sender for system email and the Email inbox channel. Sending from a customer's own domain needs a Resend account.
