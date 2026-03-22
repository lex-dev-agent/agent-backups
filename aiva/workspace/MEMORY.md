# MEMORY.md - Long-Term Memory

## People
- **Petra Remnant** — Ben's wife, petra@williamsonz.co.nz (Petra Williamson at work). Warm and friendly.
- **CONTACTS.md** — always check for email addresses and people details before asking Ben.

## Aiva's Email
- **Address:** aiva@benremnant.com (set up 2026-03-15)
- **Read:** `gog --account aiva.remnant@gmail.com gmail search 'is:unread' --max 10 --json`
- **Send:** `python3 scripts/send-email.py --to <email> --subject "..." --body "..."`
- **Approval required:** Every outbound email goes through MC approval. Use `skills/mission-control/scripts/mc-email.sh send` to create approval requests. Ben approves on MC, MC sends via SMTP directly.
- **Old send-email.py retired** (2026-03-22). Now using MC-native email system.
- **Cron:** system cron checks every 5 min via `scripts/check-email.sh`
- **Sign-off:** `Aiva | Chief of Staff` / `aiva@benremnant.com`
- Full setup details in `references/infrastructure.md`

## Salcom / Jira
- **Instance:** salcom.atlassian.net (Cloud ID: 6b22db59-1700-4bf3-896a-64ce68603ef2)
- **My account:** aiva@benremnant.com (accountId: 712020:790b5d5f-59f6-4142-b86a-aa528db7ff01)
- **Token:** `~/.config/jira-env` — requires `api.atlassian.com/ex/jira/{cloudId}` base URL
- **Skill:** `skills/jira/` with `scripts/jira.sh`
- **Project:** SalBot Trial (SB)
- **Lessons:** Always self-assign tasks. Always set myself as reporter when creating for others.

## Nesty
- Devices: garage/door, outside/light, lounge/light, kitchen/light, bedroom/light, bathroom/light
- API + MQTT details in `references/nesty.md`

## Cosie — Baby Sleep Environment Monitor (2026-03-20)
- Cellular-connected teddy bear, monitors baby sleep temperature, alerts parents
- Name "Cosie" chosen by Ben (NZ spelling of "cozy")
- MC project ID: 4aa3b6e0-cd02-48b3-99c7-bedf4e09e780
- Repo: github.com/lex-dev-agent/cosie
- Website: cosie.benremnant.com (HTTPS live)
- Tech: nRF9151, Sensirion STS40, AWS IoT Core
- Positioning: "Sleep environment monitor", $179 NZD, 60% margin
- 12-month battery + cellular + SIDS window all align naturally
- Reports in MC docs under Ideas/Cosie

## Ben's Strategic Vision
- **Core goal:** Build and accelerate a team of agents. Everything supports this.
- Revenue generation needed to fund more agents
- Current team: Aiva (coordination), Lex (coding), Devon, Sal, Vera — all on MC

## Agent Onboarding
- I'm the only one who brings new agents online
- Templates in MC docs: Agent Onboarding/Foundation Files/ (Generic/ + Developer/)
- Placeholders use backtick format: `AGENT_NAME`, `HUMAN_NAME`, etc.

## Skill Distribution
- **MC skill source of truth:** Lex's MC repo (`skill/` directory alongside `plugin/`)
- **Update flow:** Lex commits skill changes with API changes → I pull from his clone → SCP to agents who need it
- **Don't forget:** When deploying MC API changes, check if the skill scripts need updating too

## Conventions
- **Cron timezone:** Always set `--tz "Pacific/Auckland"` on recurring cron jobs. For one-shot `--at` schedules, include the offset: `--at "2026-03-22T16:35:00+13:00"`. Bare ISO timestamps default to UTC.
- **Secrets:** `~/.config/<app>.env`, never in workspace (browsable via MC Team tab)
- **Gateway restart:** Schedule one-shot cron self-wake before restarting
- **MC deploys:** Use `scripts/deploy-mc-vps.sh` (see `references/deploy.md`)
- **Notification channel:** Telegram bot (chat ID: 8507305084)
- **MC deploy:** When deploying, generate MC_ENCRYPTION_KEY if not in VPS .env (needed for email accounts password encryption, AES-256-GCM)

## Personal Lessons
- Title case for MC document names
- Answer the question first, then explain
- Don't retry MC board API creates (they succeed even when response looks empty)
- Check remote file state before SCP overwriting
- Ben doesn't like em dashes, use full stops or commas
- Ben doesn't like startup-y language
- Ben uses voice-to-text — expect mishearings ("Eva" for "Aiva", etc.)
