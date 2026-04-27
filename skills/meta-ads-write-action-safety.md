---
id: meta-ads-write-action-safety
title: Meta Ads Write Action Safety
description: Confirmation protocol and safety rails for any change that touches a live Meta ad account
tags:
  - meta-ads
  - safety
  - write-operations
  - confirmation
category: advertising
author:
  name: Toffu
  pic_url: /toffu-logo.png
  profile_url: https://toffu.ai
---

Apply this protocol any time an action will pause, edit, duplicate, or create something inside a live Meta ad account. Read operations (insights, listings, breakdowns) don't require this — they're free to run.

## Read is safe, write is not

The classification is binary:
- **Read (safe):** insights, account/campaign/ad set/ad listings, breakdowns, account discovery. No real-world side effect.
- **Write (dangerous):** pause, resume, budget change, status change, creative edit, duplicate, full campaign creation. These touch real money — including campaigns that don't exist yet but will start spending the moment they're un-paused.

Treat write actions like sending email to a customer list: the action is irreversible in spirit even if technically reversible. A paused campaign that's resumed has lost its delivery momentum. A duplicated campaign is now a real object Meta is billing learning effort against.

## The confirmation protocol — non-negotiable

Every write requires its own explicit, in-the-moment confirmation. No exceptions, no shortcuts.

1. **State the action and its impact in plain language.** Not "pausing ad 120214867423." Instead: "Pause ad `Spring_Sale_v3` (ID 120214867423). This ad spent ₪47 in the last 7 days at 0.8% CTR. Confirm?"
2. **Wait for an explicit confirmation in this turn.** "yes," "confirm," "do it," or the user's-language equivalent. A "go ahead" from earlier in the conversation does **not** carry over to a new write. Each write needs its own fresh consent.
3. **One action per confirmation by default.** If the user wants to pause 12 ads, present a numbered list and confirm the whole batch in one explicit message ("yes, pause all 12") — but log each call separately so any failure is identifiable.
4. **Always dry-run first when the script supports it.** A dry-run prints the exact change without calling the API. Read the dry-run back to the user in plain language before asking for the real confirmation.
5. **Default new objects to PAUSED.** Anything you create — campaign, ad set, ad — is created in PAUSED state. The user flips to ACTIVE in Ads Manager themselves once they've eyeballed the result. This is the last safety net.

## Generic "ok" doesn't count for high-impact writes

For campaign creation, large budget changes, or batch operations, the confirmation phrase must name the action. "Confirm create campaign," "yes pause all 12," "approve budget change to $X." A bare "ok" or "sure" is too easily produced by autopilot — require a phrase that names what's about to happen.

## Specific dangerous patterns

- **Don't auto-shift budgets above +50% in one call** even with confirmation. Step it up over multiple calls with a fresh confirmation each time. Meta's learning phase resets on big jumps anyway, so you're not gaining speed by skipping steps.
- **Don't mass-pause without checking dependencies.** Pausing the only active ad in an ad set kills the ad set's delivery. Pausing all ad sets in a CBO campaign hands the entire budget to one ad. Always show dependents in the confirmation message.
- **Don't duplicate without renaming.** A duplicate that inherits its parent's name confuses every report and dashboard downstream. Force a name suffix (e.g. `_v2`, `_clone_2026-04-27`) on every duplicate.
- **Don't delete; pause.** Deletion is for trash collection in Ads Manager. From an automation, always pause — pause is reversible, deletion erases learning data and cross-references.

## Rollback discipline

Every multi-object write (especially `create_campaign`-style operations) must produce a state file or a rollback handle: every ID created, in the order created. If something fails halfway through, the rollback handle is the only thing that lets the user undo cleanly. No state file = no write.

## What's still the user's job

The user owns the strategic decision; you own the safety rails. You don't refuse to make a budget change because you think the campaign is bad — that's their call. You **do** refuse to make a budget change without confirmation, without a dry-run, or above a hard cap. Process safety is yours, judgment is theirs.
