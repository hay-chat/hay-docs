---
layout: docs.njk
title: Help Center
description: Publish your documents as a public help center your customers can search and read, on a Hay address or your own domain.
section: user-guide
---

## What is the help center?

The help center is a public website built from your [documents](/docs/user-guide/documents/). Customers can browse it by topic, search it, and rate each article with "Was this helpful?". Hay hosts it for you: there is nothing to install.

Open it from **Knowledge → Help center** in the dashboard. Only owners and admins can set it up or change it.

> If you host Hay yourself and the page says help centers are not set up on this server, see [Help center on a self-hosted server](/docs/technical/self-hosting/#help-center).

## Setting it up

The first time you open the page, a short setup walks you through three steps. You can change everything later.

1. **Basics.** The name shown at the top of the site, its address (`yourname.haychat.co` on hosted Hay) and its language. The language sets the site's own labels, such as the search box; your articles are shown as written.
2. **Brand.** A ready-made theme to start from, an accent colour, a font and a home page layout: one card per collection, or every article listed under its collection.
3. **Content.** Your first collection and the documents in it. You can skip this step.

A new help center is **offline**: nobody outside your team can see it until you go live.

## Collections and articles

Articles are grouped into **collections**, such as "Getting started" or "Billing". Each collection has a title, a short description and an optional emoji icon, shown on the home page.

On the **Content** tab you can create, reorder, edit and delete collections, and add documents to them. A document can be in one collection at a time; adding it to another moves it.

An article shows on the site only when its document is:

- **published** (not a draft),
- **public**, and
- **placed in a collection**.

The Content tab marks any article that is not live and says why ("Not live: draft", "Not live: not public").

Adding documents to a collection marks them public. Removing an article from a collection, or deleting the collection, does not make the document private again, and the dashboard reminds you of that. Change it on the document's page if you need to (see [Visibility](/docs/user-guide/documents/#visibility)).

Deleting a collection never deletes the documents in it.

## Appearance

The **Appearance** tab has a live preview next to the settings. It shows the whole page scaled down to fit, as a desktop browser or as a phone. The preview shows your unsaved changes; visitors see them once you save.

### Themes

Start from one of six ready-made themes, then change anything you like. Each theme keeps your accent colour, hero text and "Powered by Hay" setting.

| Theme     | Look                                                          |
| --------- | ------------------------------------------------------------- |
| Classic   | Accent-coloured hero, centred, with cards                     |
| Aurora    | Purple-to-pink gradient hero, centred, with cards             |
| Minimal   | No hero band, left aligned, always light                      |
| Midnight  | Deep blue gradient hero, always dark                          |
| Editorial | Serif type, light tint of your accent, list layout, sharp corners |
| Playful   | Rounded type, orange-to-pink gradient, pill-shaped corners    |

Once you change a setting, the dashboard shows which theme you started from ("Customised from Aurora").

### Settings

- **Accent colour.** If it is too light to read on white, links use the normal text colour and buttons stay readable.
- **Font**: Sans, Serif, Rounded or Mono.
- **Home page layout**: cards or list.
- **Colour scheme**: Automatic follows each visitor's light or dark setting; Light or Dark always uses that one.
- **Corners**: Sharp, Rounded or Pill, for cards, the search box and buttons.
- **Hero background**:
  - **Accent**: your accent colour.
  - **Gradient**: two colours of your choice, running diagonally, across or down. The dashboard warns you if text would be hard to read on it.
  - **Tint**: a light wash of your accent colour.
  - **Image**: a photo you upload (JPEG, PNG or WebP, up to 5 MB; a wide photo works best). The text on it is white, so you can darken the image a little, a medium amount or a lot. The image is saved as soon as you upload it. Until you upload one, the hero uses your accent colour.
  - **None**: no band; the hero sits on the page background.
- **Hero alignment**: centred or left.
- **Hero title and subtitle** above the search box.
- Whether **"Powered by Hay"** is shown.

### Search engines and link previews

Every page is set up for search engines and for sharing without any work from you:

- Each page has a title and a description. Where an article has no description, Hay uses the start of its text.
- Links shared in chat apps and social networks show a preview card with the page's title and description, and your organization's logo if you have one.
- Search engines receive a sitemap and structured data (articles, breadcrumbs and the site's search), which helps them show your pages well.

## Going live

On the **Overview** tab, **Go live** makes the site public at its address. **Go offline** hides it again: visitors get a "not found" page and links to articles stop working until you go live again. Nothing is deleted.

From the same tab you can:

- open a **Preview** of the site, including while it is offline,
- change the name, address and language (changing the address breaks links to the old one; you can change it at most five times a day),
- turn off **Search engines can index this site** to ask search engines such as Google not to list it.

## Using your own domain

On the **Domain** tab you can serve the help center from a domain you own, such as `help.yourcompany.com`. The built-in address keeps working either way.

1. Enter the domain and click **Add domain**.
2. Hay shows two DNS records. Add one of them where you manage your domain's DNS:
   - **CNAME (recommended):** points your domain at Hay, which both proves it is yours and sends visitors to your help center.
   - **TXT:** only proves the domain is yours. Use it when your DNS provider hides CNAME records, for example when the domain is proxied through Cloudflare, and send traffic to Hay some other way.
3. Click **Check now**. DNS changes can take a while to spread, so try again later if the check fails.

Once the domain is **Connected**, HTTPS can take a few minutes to start working.

Hay checks connected domains again every day. If the record is missing three days in a row, the domain stops serving your help center until it passes a check again.

## The chat widget on your help center

When your [web chat](/docs/user-guide/webchat/) is turned on, the chat widget appears on every page of your help center, and article pages end with a "Still need help?" note pointing to it. On an article, the assistant is told which article the visitor is reading.

## Known limits

- There is no button on the page that opens the chat widget; visitors open it from the widget's own launcher.
- Tables and level 4 to 6 headings in imported markdown show as plain text.
