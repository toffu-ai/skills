---
id: google-ads-account-context
title: Google Ads Account Context
description: Hard-won rules for managing Google Ads accounts - what the platform won't tell you
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

## How Google Ads Actually Works

- Google's automated bidding optimizes for what you tell it to, not what you actually want. If your conversion tracking counts page views, Target CPA will buy you cheap page views. Fix tracking before touching bid strategy.
- Performance Max is a black box by design. It will cannibalize your brand search traffic and report it as Performance Max conversions. Always run a brand exclusion list on PMax campaigns, and compare PMax results against a holdout period before declaring it a win.
- The "Recommendations" tab exists to increase your spend, not your performance. Auto-applying recommendations has destroyed more accounts than bad targeting. Review every suggestion against actual account data.
- Google rep advice is quota-driven. They will push broad match, PMax, and budget increases regardless of account context. Treat their suggestions as hypotheses to test, not instructions to follow.

## Structure That Actually Scales

- Single keyword ad groups (SKAGs) are dead. Google's matching has drifted so far that exact match behaves like old phrase match. Use tight thematic ad groups (5-8 closely related keywords) instead.
- Separate brand and non-brand campaigns. Always. Brand traffic inflates ROAS and masks non-brand underperformance. Blended reporting hides real acquisition cost.
- Segment by margin, not just product. A campaign optimized for ROAS on a 10% margin product will look great while losing money. Feed margin data into value-based bidding or segment manually.
- Build negative keyword lists proactively, not reactively. The search terms report now hides 30-50% of actual queries. Compile industry negatives before launch.

## Bidding Traps

- Don't switch bid strategies on a campaign that's performing. The learning period will tank performance for 2-3 weeks and there's no guarantee it recovers to the prior level.
- Target ROAS needs 50+ conversions per month to optimize effectively. Below that, the algorithm oscillates. Use Maximize Conversion Value with no target until you have enough volume.
- Manual CPC is not a fallback - it's a precision tool. Use it when you need granular control on a small keyword set (competitor terms, high-value long-tail) and nowhere else.
- Portfolio bid strategies across campaigns with different margins or conversion values will average everything toward mediocrity. Keep bid strategies scoped to campaigns with similar economics.

## Metrics That Lie

- Quality Score is directional at best. Don't chase 10/10. A 6/10 keyword with strong conversion data is more valuable than a 9/10 keyword that doesn't convert.
- Impression Share loss due to budget looks like an opportunity but is often Google telling you it wants to spend more of your money on marginal queries. Check Search Impression Share at the keyword level before increasing budget.
- View-through conversions on Display/YouTube inflate reported performance by 3-10x. Always evaluate these campaigns on click-through conversions only, or use incrementality testing.
- "Conversions" in the Google Ads UI may include micro-conversions, duplicates, or cross-device estimates depending on settings. Verify what's actually counted before reporting.
