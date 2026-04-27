---
id: meta-ads-budget-change-rules
title: Meta Ads Budget Change Rules
description: Hard caps, learning phase rules, and currency pitfalls for changing budgets on live Meta ad sets and campaigns
tags:
  - meta-ads
  - budget
  - scaling
  - safety
category: advertising
author:
  name: Toffu
  pic_url: /toffu-logo.png
  profile_url: https://toffu.ai
---

Apply this whenever recommending or executing a budget change on a live Meta ad set or campaign.

## The hard caps

- **No single budget change above 2x (+100%) in one call.** This is a process limit, not a strategy preference. Larger jumps almost always trigger a learning-phase reset and unstable delivery for 3–7 days. If the user wants 3x, do it in two steps with a fresh confirmation each time.
- **The 20% rule for stable scaling.** Any change of ±20% or more risks resetting Meta's learning phase. Scale by 10–15% per change, wait 48–72 hours, then scale again. The "20% per day" rule of thumb is the soft ceiling for in-flight optimization, not a target.
- **Don't scale during learning.** If the ad set is in the "Learning" or "Learning Limited" state, leave the budget alone until it exits. Budget changes during learning extend learning. You're trading three days of stability for whatever you thought you were gaining.

## Daily vs. lifetime budget — they're mutually exclusive

A campaign or ad set has either a `daily_budget` or a `lifetime_budget`, never both. Before changing, read the current object to see which field exists, then update only that field. Trying to set the wrong one returns an API error or — worse — silently does nothing. The most common bug here: assuming "budget" is one field, when in fact it's two and only one is populated.

## Currency minor units — the most common arithmetic bug

Meta's API returns and accepts budget values in **minor units** (cents, agorot, pence). $50.00 USD is `5000`, ₪75.00 ILS is `7500`, £30.00 GBP is `3000`. **Zero-decimal currencies** (JPY, KRW, VND, ISK, TWD, CLP, XAF, XOF, UGX) are returned as whole units — 1000 JPY is `1000`, not `100000`.

If you read `daily_budget` as `7500` and tell the user "your daily budget is ₪7,500/day" you've just told them they're spending 100x what they actually are. Always:

1. Read the account's `currency` field first.
2. Divide minor-unit budget values by 100 unless the currency is in the zero-decimal list.
3. When writing a new budget, multiply user-entered major-unit amounts back to minor units (and skip the multiply for zero-decimal currencies).

## Cutting budget vs. pausing

Cutting a budget preserves the algorithm's learning. Pausing destroys it. If a campaign needs less spend but should keep running:

- **Reduce, don't pause.** A 50% budget cut keeps the optimization signal alive. Pausing for even an hour and resuming triggers re-learning.
- **Don't cut below the minimum daily threshold.** Meta won't deliver under ~$1/day for any campaign and far higher for conversion campaigns (effectively `~50× target CPA / month`, or roughly `1.5× target CPA / day`). Below that, the campaign is "running" but not delivering.
- **Don't yo-yo.** A campaign that's cut 30%, then raised 40% the next day has been through two learning resets. Make the directional call once.

## Scaling protocol for winners

When a campaign is performing and the user wants to increase spend:

1. **First check whether the ceiling is delivery, not budget.** If the campaign isn't currently spending its full budget, raising the budget changes nothing — the constraint is auction competitiveness, audience size, or creative fatigue, and more budget just makes those constraints more visible.
2. **Prefer horizontal scaling (duplicate + new audience/geo) past 2-3x of the original budget.** A campaign at 4x its original budget is rarely the same campaign — it's bidding against itself in the auction and reaching cheaper-to-show, lower-quality segments.
3. **Step in 10–20% increments every 48–72 hours.** Faster than that, you can't tell whether performance shifts are caused by the budget change or normal variance.
4. **Don't increase budget at the same time as creative refresh.** Two simultaneous changes mean you can't attribute either outcome. Stagger by at least one full optimization cycle (48–72 hours).

## When a budget change is the wrong tool

- The campaign is fatigued (frequency too high, CTR decay): budget changes won't help. Refresh creative.
- Conversion volume is low (<30/week): the algorithm isn't stable. More budget will buy more variance, not better performance.
- The user is hitting a daily spend cap on the account itself, not the campaign. The campaign budget can be raised forever and nothing happens. Check `spend_cap` on the ad account.
