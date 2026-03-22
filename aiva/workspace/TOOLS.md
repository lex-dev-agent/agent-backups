# TOOLS.md

## Reference Files

Detailed references live in `references/`. Read on demand:
- `references/nesty.md` — Nesty API, devices, MQTT commands
- `references/infrastructure.md` — VPS, DNS, email setup

## Mission Control

Use the **MC skill** (`skills/mission-control/`) for all MC interactions. It handles auth, API calls, and formatting.

**Secrets:** `~/.config/<app>.env` files, never in workspace. Source at runtime.

### MC Workflow

- **@mention** the person you're talking to in every message, even Ben
- **Only DM Ben.** Inter-agent coordination goes through All chat or kanban tasks
- **Kanban:** Move to In Progress when starting. Move to Review when done. Never move to Done.
- **Creating tasks for others:** Backlog (auto-notifies) or Roadmap for later
- **Auth:** `source ~/.config/mc.env` — has MC_URL, MC_TOKEN, MC_WEB_PASSWORD
- **MC docs** (for human reference): available at `https://mc.benremnant.com` under Documents > Mission Control/

## OpenRouter Balance

Script: `scripts/openrouter-balance.sh` — shows credits, key limits, usage warnings.

## gog (Google CLI)

`~/bin/gog` (v0.9.0) — Gmail, Calendar, Drive, Docs, Sheets, Contacts.

- Accounts: `ben.remnant@gmail.com` (Ben), `aiva.remnant@gmail.com` (Aiva)
- Env: `GOG_ACCOUNT=ben.remnant@gmail.com`, keyring passphrase in `GOG_KEYRING_PASSWORD`

```bash
gog gmail search 'is:unread newer_than:2h' --max 10 --json
gog calendar events --max 5 --json
gog drive ls --max 10
```

## Agents

See the **ssh-agents skill** (`skills/ssh-agents/`) for full host table and SSH commands.
See the **deploy skill** (`skills/deploy/`) for MC deployments and plugin updates.

Quick reference:
- Aiva + Lex → `https://mc.benremnant.com` (cloud MC)
- Sal, Vera, Devon → `http://192.168.8.211` (Sal's local MC)
- MC Plugin: v0.3.4, package `openclaw-plugin-mission-control`

## Secret Files

| File | Contains |
|------|----------|
| `~/.config/mc.env` | MC_URL, MC_TOKEN, MC_WEB_PASSWORD |
| `~/.config/nesty.env` | NESTY_API_KEY, NESTY_URL, MQTT credentials |
| `~/.config/vps.env` | VPS_HOST, VPS_USER, VPS_KEY path |
| `~/.config/jira-env` | Jira API token and email |
| `~/.config/aws-ses.env` | AWS SES SMTP credentials |
| `openclaw.json` | Channel tokens (MC, Telegram) |

## Cron Jobs

- Always set `--tz "Pacific/Auckland"` on recurring cron schedules.
- For one-shot `--at` schedules, include the timezone offset: `--at "2026-03-22T16:35:00+13:00"` (not bare ISO which defaults to UTC).
- Never assume local time is UTC.
