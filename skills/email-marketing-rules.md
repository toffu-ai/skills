---
id: email-marketing-rules
title: Email Marketing Rules
description: How email actually works at scale - deliverability, segmentation, and what moves revenue
tags:
  - email
  - newsletter
  - nurture
  - writing
category: channels
author:
  name: Toffu
  pic_url: /toffu-logo.png
  profile_url: https://toffu.ai
---

Apply these rules whenever writing or reviewing marketing emails.

## Deliverability Is the Whole Game

- Nothing else matters if you're landing in spam. Monitor inbox placement rate, not just send/delivery rate. "Delivered" means it reached the server, not the inbox.
- Gmail and Yahoo's 2024 sender requirements are table stakes: authenticated domain (SPF, DKIM, DMARC), visible unsubscribe, under 0.3% spam complaint rate. Violate these and your entire domain gets throttled, not just the offending campaign.
- Clean your list quarterly. Anyone who hasn't opened in 90 days goes to a re-engagement segment. Anyone who hasn't opened in 180 days gets removed. A smaller, engaged list outperforms a large, dead one every time. ISPs watch engagement ratios.
- Warm up new sending domains slowly. Start with your most engaged segment (recent purchasers, frequent openers). Scale volume by 20-30% per day. Skip this and you'll hit spam traps on day one.

## What Actually Drives Revenue

- Automated flows (welcome, cart abandonment, post-purchase, winback) generate 30-50% of email revenue from 5% of the sends. Optimize these before touching your campaign calendar.
- Cart abandonment emails have the highest ROI of any email type. Three emails over 48 hours (1h, 24h, 48h) with escalating urgency. First email is a reminder, second adds social proof, third adds scarcity or incentive.
- Segmentation is the difference between 0.5% click rate and 5% click rate. "Blast the whole list" is not a strategy. Segment by purchase behavior, engagement level, and lifecycle stage at minimum. A lapsed customer and a first-time buyer should never receive the same email.
- Plain text-style emails from a person ("Sarah from Acme") outperform designed HTML templates for B2B. For DTC, designed emails with product imagery perform better. Know your audience.

## What Open Rates Won't Tell You

- Apple Mail Privacy Protection inflates open rates by 30-50% since iOS 15. Open rate is no longer a reliable engagement metric. Click rate and conversion rate are what matter.
- A/B test subject lines on click rate, not open rate. A subject line that gets opens but not clicks is clickbait that erodes trust.
- Unsubscribes are healthier than spam complaints. An unsubscribe removes one person. A spam complaint trains ISP filters against your entire domain. Make unsubscribing easy and obvious.
- Revenue per email sent (RPE) is the metric that captures everything: content quality, targeting, timing, and deliverability in one number. Track it per campaign and per flow.

## Common Mistakes That Look Smart

- Sending to your "most engaged" segment every time trains the algorithm but starves the rest of your list. Rotate segments so you maintain deliverability across your full audience.
- Personalization beyond first name has diminishing returns unless you have genuinely relevant behavioral data. "We noticed you looked at [product]" works. "Hi [first_name], as a valued [city] customer" is uncanny valley.
- Re-engagement campaigns that offer discounts to inactive users teach your audience to go dormant until the discount arrives. Re-engage with content or novelty, not price.
