# TOOLS.md - Local Notes

## Mission Control

Use the **MC skill** (`skills/mission-control/`) for all MC interactions. It handles auth, API calls, and formatting.

### MC Workflow

- **@mention** the reporter in kickoff and completion messages
- **Kanban:** Move to In Progress when starting. Move to Review when done. Never move to Done.
- **Creating tasks for others:** Backlog (auto-notifies) or Roadmap for later
- **Auth:** `source ~/.config/mc.env` — has MC_URL, MC_TOKEN

### Local app workflow

When changing the `mission-control` codebase:
1. Make the code change.
2. Build: `npm run build` in `repos/mission-control/`.
3. Redeploy: kill stale next-server (`pkill -f "next-server"`), then restart. Stale processes cause chunk mismatch errors.
4. Do NOT commit `.next/` — it is gitignored. Always rebuild on deploy.
5. Commit the repo after the change is verified and working.
6. Don't say a change is done until rebuilt, redeployed, and committed.

## GitHub

- Account: `lex-dev-agent`
- Auth: HTTPS via `gh auth`, fine-grained PAT
- `gh` is ready to use — no setup needed.

## Cron Jobs

- Always set `--tz "Pacific/Auckland"` on recurring cron schedules.
- For one-shot `--at` schedules, include the timezone offset: `--at "2026-03-22T16:35:00+13:00"` (not bare ISO which defaults to UTC).
- Never assume local time is UTC.
