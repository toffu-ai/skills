---
id: meta-ads-anomaly-detection
title: Meta Ads Anomaly Detection
description: How to flag real performance shifts in Meta ad accounts without screaming about noise
tags:
  - meta-ads
  - monitoring
  - anomalies
  - diagnostics
category: analytics
author:
  name: Toffu
  pic_url: /toffu-logo.png
  profile_url: https://toffu.ai
---

Apply this when comparing Meta ad performance period-over-period, building a "what changed" report, or building automated alerts.

## The comparison frame

Always compare a window to the **immediately prior equivalent window** — last 7 days vs. the 7 days before that, last 14 vs. the 14 before, etc. Don't compare to "last month" loosely; calendar drift introduces seasonality artifacts (weekday/weekend mix, paydays, holidays) that masquerade as performance changes. Run anomaly detection at the **campaign** level. Account-level smooths out exactly the signals you're trying to find; ad set/ad level fragments small movements into statistical noise.

## The two-gate rule for "real" anomalies

A change must clear **both** gates to count. Either alone produces too many false positives.

1. **Relative gate: ≥25% percent change.** Below that, you're inside normal Meta delivery variance.
2. **Absolute gate: change must clear a metric-specific floor.** Without this, a campaign going from $0.50/day to $0.75/day spend triggers a "+50%" alert that means nothing.

Default absolute floors:
- **Spend:** half of the campaign's minimum-meaningful daily spend (e.g. if you ignore <$10/day campaigns, the floor is $5 of absolute movement).
- **CTR:** 0.5 percentage points (0.005). A campaign moving from 1.2% → 1.5% CTR clears this; 1.20% → 1.24% doesn't.
- **CPM:** $1 swing.
- **CPC:** $0.10 swing.
- **Purchase ROAS:** 0.3x absolute change.

## Distinguish three kinds of "anomaly"

Don't lump these together — they have different responses:

- **Started spending** (prior = $0, current > 0): a campaign came online. Verify it was intentional, then judge it on its own performance, not as a "spike."
- **Stopped spending** (current = $0, prior > 0): a campaign went dark. Check: did it run out of budget, get paused, lose all delivery, or hit its end date? Often a budget exhaustion masquerades as a "performance" issue.
- **Active change**: campaign was running both periods, metrics shifted. This is the only case where percent-change interpretation makes sense.

## Severity ranking

Sort flagged campaigns by `flagged_metrics_count × (current_spend + prior_spend)`. Three metrics moving on a $50K campaign is the day's #1 issue. One metric moving on a $200 campaign goes at the bottom of the list. Without weighting by spend, you spend your whole report on tiny accounts.

## What to do before reporting an anomaly as a real change

- **Check the calendar.** Did the prior window contain a holiday, a flash sale, a tracking outage, or a creative refresh? Compare against the campaign's launch/edit history before claiming the metric "moved."
- **Check the conversion volume.** ROAS on a campaign with 4 conversions vs. 6 conversions has a 50% delta that's pure noise. Below ~30 conversions per period, the algorithm itself isn't statistically stable, let alone the metric.
- **Check attribution settings.** A single attribution-window flip (1-day click → 7-day click) can show up as a "+40% conversions" anomaly that's actually a measurement change.
- **Check for breakdown shifts.** A campaign whose spend moved from Instagram Reels to Facebook Feed will show CPM/CTR changes that reflect placement mix, not creative or audience health. Always pull the placement breakdown before recommending action.
