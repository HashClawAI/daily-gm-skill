# Daily GM Skill

A modular morning briefing skill for [OpenClaw](https://github.com/openclaw/openclaw).

Pushes a consolidated daily briefing to your channel as a single message — configurable modules, schedule, and delivery target.

## Modules

| # | Module | Description | Requires |
|---|--------|-------------|----------|
| 1 | Zen / Buddhist Wisdom | Daily passage with commentary and life insight | None |
| 2 | Chinese Metaphysics Forecast | BaZi-based daily fortune analysis | User birth info |
| 3 | Calendar & Todos | Pending tasks and calendar events | memory files / gog |
| 4 | Email Digest | Unread email summary | himalaya or gog |
| 5 | Top 10 News | Major headlines across categories | web_search |
| 6 | Academic Papers | Notable recent research picks | web_search |

All modules are optional — enable whichever subset you want.

## Install

```bash
openclaw skill install daily-gm-skill.skill
```

Or clone this repo and copy the `daily-gm` folder into your skills directory.

## Usage

Ask your OpenClaw agent:

> "Set up a daily morning briefing at 7am to this channel"

The skill will guide module selection, collect any required info (e.g. birth date for metaphysics), and create the cron job.

## License

MIT
