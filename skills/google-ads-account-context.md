---
id: google-ads-account-context
title: Google Ads Account Context
description: Background knowledge for working with Google Ads accounts - structure, terminology, and defaults
tags:
  - google-ads
  - ppc
  - search
  - context
category: channels
author:
  name: Toffu
  pic_url: /toffu-logo.png
  profile_url: https://toffu.ai
---

Apply this context whenever working with Google Ads data or making recommendations.

## Account Structure Expectations

- Campaigns should be organized by objective (brand, non-brand, competitor, remarketing)
- Each ad group should have a tight keyword theme - no more than 15-20 keywords per ad group
- Every ad group should have at least 2 active responsive search ads for testing
- Negative keyword lists should exist at the account level for common irrelevant terms

## Default Benchmarks

Use these as starting points. Actual benchmarks vary by industry.

- Search CTR: 3-5% is healthy. Below 2% needs attention.
- Search CPA: varies by industry, but flag anything 3x above account average
- ROAS: 3x is a common baseline for profitable campaigns
- Quality Score: 7+ is good. Below 5 on high-spend keywords is a problem.
- Impression Share: below 50% on brand campaigns means budget or bid issues

## Bidding Context

- Prefer automated bidding (Target CPA, Target ROAS, Maximize Conversions) for campaigns with 30+ conversions per month
- Manual CPC is acceptable for new campaigns with insufficient conversion data
- Don't recommend switching bid strategies during the first 14 days after a change - the learning period needs to complete

## Common Pitfalls

- Broad match keywords without negative keywords bleed budget fast
- Smart campaigns and Performance Max hide data - always note the limited visibility
- Search Partners often have lower quality traffic - recommend separating or excluding
- Display placements in search campaigns (if auto-opted in) should be called out
