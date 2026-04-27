---
id: meta-ads-creative-fatigue-detection
title: Meta Ads Creative Fatigue Detection
description: Concrete thresholds for spotting fatigued Meta ads before CPMs balloon and ROAS collapses
tags:
  - meta-ads
  - creative
  - fatigue
  - diagnostics
category: advertising
author:
  name: Toffu
  pic_url: /toffu-logo.png
  profile_url: https://toffu.ai
---

Apply this whenever asked "is my ad fatigued?", "why is CPM rising?", or when reviewing Meta ad-level performance over 14+ days.

## How to actually measure fatigue

Don't eyeball a single number. Split the requested window in half and compare ad-level metrics between halves. A trend over 14 days has signal; a single 7-day snapshot doesn't tell you whether things are getting better or worse. Minimum analyzable window is 4 days — anything shorter is noise.

Run fatigue analysis at the **ad** level, not the ad set or campaign level. Fatigue is a property of a specific creative, not a strategy. An ad set with one tired creative and two fresh ones looks fine in aggregate while burning money on the dead one.

## The three flags that matter

Fire any one of these and the creative is suspect. Fire two or more and it's done.

1. **Frequency ≥ 3.0** in the second half of the window. The audience has seen this ad three times per person — diminishing engagement is mechanical at this point. For broad prospecting, 2.5 is already worrying. For retargeting against small audiences, 4.0 can still be fine.
2. **CTR fell to less than 70% of first-half CTR.** A 30%+ relative drop means the creative is wearing out faster than the audience is refreshing. The exact ratio matters more than the absolute CTR — a 0.8% → 0.5% drop is fatigue; a steady 0.5% is not.
3. **CPM rose 20%+ while CTR fell.** This is Meta's delivery-quality penalty in action. The auction is charging you more because your engagement signals are weakening. When you see CPM up + CTR down together, the algorithm has already made its decision.

## The noise floor

Ignore any ad that spent less than $10 (or local equivalent) in the second half of the window. Below that, percent-change math is meaningless — a $2 ad going from 1% CTR to 0.5% is two clicks worth of variance, not fatigue.

## Severity ranking

When multiple ads are flagged, prioritize by `flag_count × second_half_spend`. A two-flag ad burning $500/week is more urgent than a three-flag ad burning $40/week. Always present the user a sorted list, not an undifferentiated dump.

## What to do once a creative is fatigued

- **Don't expand the audience.** Audience expansion almost never revives a tired creative — you're showing the same dead asset to more people. The creative is the problem.
- **Don't rotate within the same ad set.** A new ad inside a fatigued ad set inherits the ad set's accumulated negative delivery signals. Launch the replacement creative in a new ad set.
- **Don't pause a fatigued ad inside a CBO campaign without a replacement queued.** CBO will reallocate budget to whatever's left, which may be the second-most-fatigued ad. Pause + launch in one move.
- **Refresh the hook first.** If the body of the ad still tests well, replacing only the first 3 seconds of video or the headline can reset frequency without rebuilding the whole creative.
