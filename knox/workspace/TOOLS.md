# TOOLS.md

## Mission Control

Use the **MC skill** (`skills/mission-control/`) for all MC interactions. It handles auth, API calls, and formatting.

### MC Workflow

- @mention the person you're talking to in every message
- Kanban: Move to In Progress when starting. Move to Review when done. Never move to Done.
- Creating tasks for others: Backlog (auto-notifies) or Roadmap for later
- Auth: `source ~/.config/mc.env` — has MC_URL, MC_TOKEN, MC_AUTHOR_ID

## Secret Files

| File | Contains |
|------|----------|
| `~/.config/mc.env` | MC_URL, MC_TOKEN, MC_AUTHOR_ID |
| `openclaw.json` | Channel tokens (MC) |

## Cron Jobs

- Always set `--tz "Pacific/Auckland"` on recurring cron schedules.
- For one-shot `--at` schedules, include the timezone offset: `--at "2026-03-22T16:35:00+13:00"` (not bare ISO which defaults to UTC).
- Never assume local time is UTC.
