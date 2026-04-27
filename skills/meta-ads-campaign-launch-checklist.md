---
id: meta-ads-campaign-launch-checklist
title: Meta Ads Campaign Launch Checklist
description: Pre-flight checks that catch the silent failures before a new Meta campaign starts spending
tags:
  - meta-ads
  - campaigns
  - launch
  - checklist
category: advertising
author:
  name: Toffu
  pic_url: /toffu-logo.png
  profile_url: https://toffu.ai
---

Apply this when launching a new Meta campaign — whether for an A/B test, a fresh angle flight, or a full client onboarding. Every item here exists because skipping it has cost real budget.

## Pre-flight: account-level checks

Run these before the first campaign object is created.

1. **Pixel installed and firing?** No pixel = no conversion data = no conversion-optimized campaign. If the account has no installed pixel events, force the objective to `OUTCOME_TRAFFIC` with `optimization_goal: LINK_CLICKS`. Don't build a conversions campaign that has nothing to optimize toward; you'd be paying Meta to optimize toward an event it never sees. Surface this and let the user choose: install the pixel first, or launch on traffic and switch later.
2. **Account balance settled?** Account status `3` (UNSETTLED) means objects can be created and saved, but nothing will deliver until the unpaid balance clears. Don't block the launch — but tell the user before they expect spend.
3. **Currency confirmed?** Read the account's `currency` and confirm it matches what the user's budget figures assume. Launching a ₪10,000/day campaign on a USD-denominated account is a real, expensive mistake.
4. **Right ad account?** Users with Business Manager access often own multiple ad accounts (personal, agency, client, archived). Confirm the target account by name and ID before creating anything. The fastest validation: is this the account where their proven historical winners ran?

## Pre-flight: identity checks

5. **Right Facebook Page.** Users routinely own multiple pages; the wrong page identity in the ad creative produces ads that "look right but feel off" and tank CTR. The right answer is usually the page that ran their recent winning ads — pull a handful of recent ads and inspect `creative.effective_object_story_id`; the prefix is the page ID.
6. **Instagram identity attached.** If the campaign should run on Instagram placements, an Instagram account must be linked to the page or specified explicitly. Without it, Instagram placements are silently dropped — the campaign launches and only delivers on Facebook, which the user discovers a week later via missing IG impressions.

## Spec validation — before any API call

7. **Default everything to PAUSED.** Campaign, ad sets, ads — all created in `PAUSED` status. The user flips to ACTIVE themselves in Ads Manager once they've eyeballed the creative. This is the last safety net between an automation bug and a real-money disaster.
8. **Run a dry-run first.** A dry-run prints the resolved plan: exact targeting, currency-corrected budgets, resolved interest IDs, and the full campaign tree. Walk it back to the user in plain language. The dry-run is also where interest-resolution issues surface (e.g., "no interest match for [name]") before they show up as silently empty audiences.
9. **Confirm before `--confirm`.** Per the write-action protocol, the actual write requires a fresh, in-the-moment, action-naming confirmation ("confirm create campaign," not just "ok"). A dry-run approval earlier in the session does not authorize the create.
10. **Audit trail / state file.** Every object the launch creates (campaign, each ad set, each creative, each ad) gets logged with its returned ID, in the order it was created. This file is the rollback handle. No state file = no launch.

## Spec validation — naming and structure

11. **Discipline names from the start.** Apply the campaign naming convention before launch, not after. Renaming objects post-launch in Ads Manager works but breaks any analytics dashboards that key on names.
12. **Match ad-set count to budget.** A 4-ad-set campaign with $20/day per ad set ($80 total) won't escape Meta's learning phase on any of them. Either consolidate ad sets or scale total budget. Rule of thumb: each ad set needs `~50 conversions / 7 days` to exit learning. Below that, the campaign is in permanent "Learning Limited."
13. **Don't spread creatives thin.** Three to five ads per ad set is usually right. Below 3, the algorithm has no real choice. Above 5–6, the lowest-performing ads starve and never get enough impressions to fairly evaluate.

## Format and feature gates

14. **Single-image link ads only via the standard launch path.** Dynamic creative, catalog ads, and collection ads need different API shapes. Don't try to coerce a single-image launcher into producing them — extend the launcher or build a dedicated path.
15. **Custom Audiences must already exist.** A new campaign can reference an existing custom audience by ID, but creating the audience is a separate API call. Confirm the audience ID before referencing it; a typo produces a campaign whose targeting silently falls back to broad.
16. **CTAs must be valid.** Meta accepts only a fixed set of CTA codes (`LEARN_MORE`, `SIGN_UP`, `SHOP_NOW`, `BOOK_TRAVEL`, `DOWNLOAD`, `GET_OFFER`, `SUBSCRIBE`, `CONTACT_US`, `APPLY_NOW`, `WATCH_MORE`, `INSTALL_MOBILE_APP`, `MESSAGE_PAGE`, `WHATSAPP_MESSAGE`, `NO_BUTTON`, etc.). A misspelled CTA throws an error mid-create and leaves the partial campaign in place — see rollback below.

## When something fails mid-launch

A multi-object launch can fail after the campaign is created but before all ads are. Don't leave debris.

- **Use the state file for rollback.** Pause every object the launch created (don't delete — pause is reversible and preserves debug info).
- **Surface the failure and offer two clean options:** retry from where it failed, or roll back and rebuild the spec.
- **Never partial-launch into ACTIVE.** If half the ad sets created and half didn't, the half that did should stay PAUSED until the full structure is in place. A launched-but-incomplete campaign delivers from a non-representative subset of the test.
