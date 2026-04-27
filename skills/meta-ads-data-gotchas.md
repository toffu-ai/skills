---
id: meta-ads-data-gotchas
title: Meta Ads Data Gotchas
description: API quirks, hidden limits, and silent failure modes that make Meta Ads data lie if you don't know about them
tags:
  - meta-ads
  - api
  - data-quality
  - gotchas
category: analytics
author:
  name: Toffu
  pic_url: /toffu-logo.png
  profile_url: https://toffu.ai
---

Apply this whenever pulling data from the Meta Marketing API or interpreting reports built on top of it. These are the failure modes that produce wrong-but-plausible numbers, not the ones that throw obvious errors.

## The 37-month data wall

Meta's insights API only serves data from the last **37 months**. Anything older returns error code `3018` and is invisible. If a user asks "what was my best campaign ever" and their activity predates the window, the API literally cannot answer — they need Ads Manager's archived reports UI. Surface this proactively before launching a query that's going to fail. A common subtle case: a 36-month "year over year" comparison straddles the wall and silently truncates one side.

## Currency minor units everywhere

Most monetary fields (`amount_spent`, `balance`, `spend_cap`, `daily_budget`, `lifetime_budget`) come back as strings in **minor units** — agorot, cents, pence. ₪75.00 ILS is `"7500"`. Divide by 100 to get human-readable amounts. **Exception:** zero-decimal currencies (JPY, KRW, VND, ISK, TWD, CLP, XAF, XOF, UGX) come back as whole units — don't divide. Always read the account's `currency` field first and branch on it.

## Account status codes — many failure states look identical

Don't show "active" or "inactive" without decoding `account_status`:

- `1` ACTIVE — normal
- `2` DISABLED — admin disabled by Meta
- `3` UNSETTLED — outstanding unpaid balance; reads work, writes succeed but **nothing delivers**
- `7` PENDING_RISK_REVIEW — under review, can't run new ads
- `8` PENDING_SETTLEMENT
- `9` IN_GRACE_PERIOD
- `100` PENDING_CLOSURE
- `101` CLOSED

`UNSETTLED` is the killer: every API call succeeds, no error is thrown, and yet not a cent will spend. Users assume their campaigns are "running" and only discover the issue days later. Always show status alongside any "this campaign is live" claim.

## Attribution windows reshape every reported number

Meta's default attribution is 7-day click + 1-day view, the most generous setting. Switching to 1-day click changes reported conversions by 30–60% on the same campaign on the same day. Before comparing periods or reporting against an external source (GA4, Shopify, backend revenue), confirm the attribution window. When pulling insights for analysis, explicitly set `action_attribution_windows` to a fixed value rather than relying on the account default — the default can change under your feet.

## `purchase_roas` and `actions` are list-of-dicts, not numbers

`purchase_roas` returns `[{"action_type":"omni_purchase","value":"3.4"}]` — a list, not a scalar. To get the actual ROAS you have to filter for `action_type` in (`omni_purchase`, `purchase`, `offsite_conversion.fb_pixel_purchase`) depending on the account's setup. Same for `actions`. Treating these as scalars silently returns zero for accounts that report under a different `action_type`.

## DELETED campaigns are invisible

Meta's `/campaigns` endpoint returns error 1815001 if you ask for status `DELETED`. Deleted campaigns can only be inspected through Ads Manager UI archived reports. If a user asks "what happened to campaign X" and you can't find it, "deleted" is the answer — not "I have no data."

## Insights queries time out — async is the workaround

Wide queries (multi-month, multi-breakdown, ad-level across thousands of ads) hit the synchronous insights timeout and return an error. Switch to **async insights**: POST the query, get a `report_run_id`, poll until `async_status: Job Completed`, then fetch the result. As a rule of thumb, anything broader than 90 days × 1 breakdown × campaign-level should default to async.

## Path A vs Path B token visibility — they don't see the same accounts

A System User token (Path A) only sees ad accounts owned by the Business Manager that issued it. A long-lived user token (Path B) sees every ad account the user's personal Facebook profile can access, including the personal ad accounts created when someone "boosts" a post from the Instagram app. Boosted-post ad accounts almost never live in a BM, so users on Path A will swear "all my Meta data" is connected and still be missing their boosted-post spend. If a user reports missing data, confirm which token type they're on first.

## Click-to-WhatsApp ads aren't a separate surface

CTWA ads run on Facebook and Instagram placements; they show up in the API as regular ads with `destination_type=WHATSAPP` and conversions buried in `actions` under types like `onsite_conversion.messaging_conversation_started_7d`. There is no "WhatsApp ads" filter or dashboard. To answer "how are my WhatsApp ads doing?" you filter ads by `destination_type` and pull conversation-started actions — don't promise the user a CTWA-specific report path.

## Pagination and rate-limit subcodes

The API paginates everything beyond 25–100 records. Rate-limit retries should specifically handle subcodes `1487742` (request rate limit), `2446079` (call limit), and `1487390` (CPU/time budget exceeded) with exponential backoff. Generic 4xx-on-error retry logic catches everything except the codes that actually matter and burns the rate limit faster.

## Audience Network is on by default and pollutes everything

New ad sets include Audience Network unless you explicitly exclude it. Audience Network impressions inflate reach and lower CPM in a way that masks true creative performance on the placements you care about. Always either exclude Audience Network or break out reports `by_publisher_platform` so the user sees what's actually happening on Facebook and Instagram independent of AN's noise.
