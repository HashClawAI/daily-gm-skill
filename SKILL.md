---
name: daily-gm
description: "Daily morning briefing that combines multiple content modules into a single message push. Default modules: Zen/Buddhist wisdom, Chinese metaphysics daily forecast (BaZi), calendar/todo review, email digest, top news headlines, and academic paper picks. Use when setting up a daily morning push, daily briefing, daily GM, morning report, or scheduled morning message to a channel. Supports customization of modules, schedule time, timezone, and channel target."
---

# Daily GM — Morning Briefing Skill

Push a consolidated daily briefing to a channel as one message. Designed for cron-based
scheduled delivery but can also be triggered manually.

## Quick Start

When a user asks to set up a daily morning briefing:

1. Read `references/modules.md` for available content modules and their prompts.
2. Ask the user which modules to include (or use all by default).
3. If the **metaphysics** module is selected, collect the user's birth info (date, time, location) for BaZi analysis.
4. Create a cron job at the user's preferred time (default 06:30 local) with `sessionTarget: "isolated"`.
5. Set timeout to 600s. Include efficiency notes in the prompt to minimize search calls.

## Module System

Six default modules are defined in `references/modules.md`:

| # | Module | Requires | Key |
|---|--------|----------|-----|
| 1 | Zen / Buddhist Wisdom | None | `zen` |
| 2 | Chinese Metaphysics Daily Forecast | User birth info | `metaphysics` |
| 3 | Calendar & Todos | memory files / gog skill | `calendar` |
| 4 | Email Digest | himalaya or gog skill | `email` |
| 5 | Top 10 News Headlines | web_search | `news` |
| 6 | Academic Paper Picks | web_search | `academic` |

Users may enable/disable any module. The skill works with any subset.

## Cron Job Setup

Build the cron payload by:

1. Reading `references/modules.md` to get the prompt template for each enabled module.
2. Concatenating enabled module prompts into one `agentTurn` message.
3. Prepending the format/delivery instructions (target channel, emoji separators, max 2000 chars, skip failed modules).
4. Setting `timeoutSeconds: 600` and `sessionTarget: "isolated"`.

Example schedule:
```json
{
  "schedule": { "kind": "cron", "expr": "30 6 * * *", "tz": "Asia/Hong_Kong" },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "<assembled prompt>",
    "timeoutSeconds": 600
  }
}
```

## Manual Trigger

To run immediately: use `cron(action=run, jobId=<id>, runMode=force)`.
If the cron job times out, fall back to running each module inline in the current session,
then combine results into one `message(action=send)` call.

## Formatting Rules

- All modules in a single message.
- Separate modules with `━━━━━━━━━━━━━━━━━━━━` dividers.
- Each module gets an emoji header.
- Total length ≤ 2000 characters.
- If a module's data source is unavailable, skip it silently.

## Efficiency Guidelines

- Minimize web_search calls (rate-limited). Combine queries where possible.
- Prefer web_fetch on aggregator pages over multiple searches.
- Space searches ≥ 2s apart to avoid 429 errors.
- Total searches per run should not exceed 3.
