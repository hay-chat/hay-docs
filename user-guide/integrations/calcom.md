---
layout: docs.njk
title: Cal.com
description: Connect Cal.com to Hay to let your AI agent check availability and book, reschedule, or cancel meetings for customers.
section: user-guide
---

## What is Cal.com?

Cal.com is a scheduling platform that lets people book meetings on your calendar based on the availability and meeting types you define. Connecting Cal.com to Hay lets your AI agent handle scheduling directly in a conversation — offering open time slots, booking a demo or support call, and rescheduling or cancelling existing bookings, without ever sending the customer away to a booking page.

## How to Connect Cal.com

1. In Cal.com, go to **Settings** → **Developer** → **API keys** and create an API key (it starts with `cal_`)
2. In the Hay dashboard, go to **Integrations** → **Marketplace**
3. Find **Cal.com** and click **Connect**
4. Paste the key into the **API Key** field (it's stored encrypted)
5. Save — Hay verifies the key against your Cal.com account immediately

That single API key is all the configuration needed. For technical details, see the plugin page on GitHub: [https://github.com/hay-chat/hay-core/tree/master/plugins/core/calcom](https://github.com/hay-chat/hay-core/tree/master/plugins/core/calcom)

## What Hay Can Do with Cal.com

Once connected, your agent gets 8 Cal.com tools:

### Availability & Booking

- **List Event Types** — see the bookable meeting types you offer and their durations
- **Check Availability** — find open time slots for a meeting type within a date range
- **Book Meeting** — schedule a meeting for the customer on one of your event types

### Managing Existing Bookings

- **List Bookings** — look through bookings, with filters, to find the right one
- **Get Booking** — pull up a single booking's details by its reference
- **Reschedule Booking** — move an accepted or pending booking to a new time
- **Cancel Booking** — cancel an accepted or pending booking

### Account

- **Get Account** — look up the connected Cal.com account (username, email, and default timezone)

Your agent chains these tools together on its own: when a customer asks to book a call, it checks your meeting types, offers available slots, and confirms the booking — and it can just as easily find and move an existing booking when someone asks to reschedule.
