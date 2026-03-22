# MEMORY.md

## Key projects

- **Mission Control** — v0.4.102, repo at `repos/mission-control`, pushed to GitHub (`lex-dev-agent/mission-control`, main branch). Source-of-truth docs at `https://mc.benremnant.com/documents/mission_control/`.
- **Nesty** — Ben's home MQTT control app (Node.js/Express), at `repos/nesty`.

## Recent changes (2026-03-22)

- **Email system shipped** — three tasks in one session: Settings tab + Email accounts CRUD (v0.5.18), Email sending engine (v0.5.19), Approval-to-send trigger (v0.5.20). All pushed to GitHub.
- Next backlog task: "Approvals tab: rich email preview + Sent view" (priority 6, reporter: aiva)
- Sal deployment task still in backlog — no SSH key auth from Lex's host to Sal (192.168.8.211). Needs Aiva or Ben.

## Architecture notes

- **globalThis for shared state in Next.js**: Turbopack bundles each route handler in its own module context. Module-level variables (Maps, Sets) are NOT shared between instrumentation.ts and API routes. Use `globalThis.__mcSomething` as the cross-module singleton pattern. This bit me hard on the WS tunnel — the registry Map was empty in route handlers because they had a different copy.
- **WS tunnel port**: 3002 (configurable via `WS_TUNNEL_PORT` env var). Needs nginx `/ws` → 3002 proxy with upgrade headers on the VPS.
- **Plugin v0.3.3**: Includes tunnel client. Skips tunnel for localhost/LAN MC URLs automatically.
- **MC plugin current version**: v0.3.3 (published to npm, CI auto-publishes on version bump).
- **MC app version**: v0.5.38. Model selector dropdown shipped 2026-03-22. Plugin v0.3.6.
- **Image upload architecture**: uploads write to `data/uploads/chat/`, served via `/api/messages/files/{filename}`. Next.js rewrite maps legacy `/uploads/chat/*` URLs. Files API has fallback to `public/uploads/chat/` for pre-migration files. Nginx no longer has a static `/uploads/` block — Next.js handles it.
- **Email system architecture** (v0.5.18–v0.5.20):
  - `src/lib/encryption.ts` — AES-256-GCM encrypt/decrypt using `MC_ENCRYPTION_KEY` env var
  - `email_accounts` SQLite table — SMTP configs with encrypted passwords
  - CRUD API: `/api/settings/email-accounts` + `/[id]` + `/[id]/test`
  - `src/lib/email.ts` — `sendEmail()` function, looks up SMTP by from address or default sender
  - `POST /api/email/send` — internal API endpoint
  - Approval-to-send: decide handler triggers `sendEmail()` on approved email approvals
  - Delivery tracking: `sent_at`, `delivery_status`, `delivery_error` columns on approvals table
  - `POST /api/approvals/:id/retry` — retry failed sends
  - Frontend: Settings tab (gear icon), settings-tab.tsx, delivery status on approval cards
  - Dependencies: nodemailer, @types/nodemailer
- **`repos/` directory**: may not persist between sessions. Clone fresh from GitHub (`lex-dev-agent/mission-control`) if missing. `npm install` needed after clone. Also `npm install nodemailer @types/nodemailer` — not in lockfile properly.
- **Approvals tab structure**: Type-based inner tabs (Email, Deploy, Phone Call etc). Each type has its own Pending/History view with sub-filters. Types auto-derived from data, Email always shown.
- **Settings tab structure**: Inner tabs — Notifications (first/default), Emails. Notification toggle uses `usePushSubscription` hook. Bell removed from sidebar.
- **Board API params**: `?columns=backlog,in-progress,review` filters cards by column. `action=update` on POST modifies existing cards. GET `/api/board/:id` returns single card.
- **Messages API params**: `order=desc`, `authorId=<handle>`, `q=<text>` for search. Limit max 200.

## Conventions & gotchas

- DM scope convention: agent messages use `dm:{agent_id}` as scopeId so they appear in the recipient's DM view.
- Voice input requires HTTPS — Ben uses Chrome flag for insecure origin override.
- Voice corrections in `use-speech-input.ts`: Ava→Aiva, cells→Sals, get hub→GitHub, licks→Lex.
- MC plugin current version: v0.3.3 (published to npm, CI auto-publishes on version bump).
- `.gitignore` at workspace root prevents `repos/` from being staged in the workspace git repo.
