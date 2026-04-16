---
id: meta-ads-account-context
title: Meta Ads Account Context
description: Hard-won rules for managing Meta Ads - what burns budgets and what actually scales
tags:
  - meta-ads
  - facebook
  - instagram
  - context
category: channels
author:
  name: Toffu
  pic_url: /toffu-logo.png
  profile_url: https://toffu.ai
---

Apply this context whenever working with Meta Ads (Facebook and Instagram) data or making recommendations.

## How Meta Ads Actually Works

- Meta's algorithm is a creative testing engine that happens to serve ads. Audience targeting matters far less than it did pre-iOS 14. The creative IS the targeting - a video about running shoes finds runners regardless of interest targeting.
- Advantage+ Shopping Campaigns (ASC) consolidate the algorithm's learning across your entire catalog. They work well for e-commerce but destroy reporting granularity. You won't know which audiences or placements drove results. Accept the tradeoff or don't use them.
- The attribution window you choose changes your reported performance by 30-60%. 7-day click + 1-day view is Meta's default and the most generous. Compare against GA4 or your own backend data before trusting Meta's numbers.
- Meta's "estimated" metrics (estimated ad recall lift, estimated reach) are modeled, not measured. Never use them in client-facing reports.

## Creative Is the Only Lever That Matters

- If a campaign isn't performing, the answer is almost always new creative, not audience changes. Swapping targeting feels productive but rarely moves the needle.
- Creative fatigue is real and measurable. When frequency exceeds 2.5x per week and CTR drops 20%+ from peak, the creative is done. No amount of audience expansion will revive it.
- UGC and founder-led content outperform polished brand creative for direct response in almost every vertical. The content that looks "too rough" for a marketing director to approve often converts 2-3x better.
- Test the hook (first 3 seconds of video, or headline of static) before anything else. If the hook fails, nothing downstream matters. Run 5 hooks against the same body content before testing different bodies.
- Static images are not dead. Carousel and single image still outperform video in many B2B and high-consideration categories. Test format before assuming video wins.

## Budget and Structure Traps

- CBO (Campaign Budget Optimization) starves ad sets that need ramp-up time. New ad sets within a CBO campaign get suppressed by the one that's already optimized. Launch new creative in its own ad set with ad set-level budget, then merge into CBO once it proves itself.
- Scaling budgets by more than 20-30% per day triggers re-learning. The algorithm resets and performance tanks for 24-72 hours. Scale gradually or duplicate the ad set at higher budget.
- Advantage+ audience expansion will happily spend your retargeting budget on cold traffic. Check the audience breakdown (in Ads Manager breakdown by audience segment) to verify your retargeting campaign is actually reaching past visitors.
- Audience Network placement inflates reach and impressions while delivering low-quality traffic. Exclude it by default on conversion-optimized campaigns. Re-test quarterly if you want, but it rarely justifies inclusion.

## What the Numbers Won't Tell You

- A campaign can show declining ROAS while actually becoming more efficient, if you've saturated cheap converters and are now reaching harder-to-convert segments. Check new vs. returning customer breakdown before killing a campaign.
- Low CTR on Meta isn't always a problem. Conversion-optimized campaigns can deliver strong CPA with sub-1% CTR because the algorithm is selecting for people likely to convert, not likely to click.
- CPM is not a performance metric. It's a market price. Rising CPMs in Q4 don't mean your campaign is worse - everyone is bidding more. Evaluate CPA and ROAS, not CPM.
