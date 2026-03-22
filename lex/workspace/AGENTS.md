# AGENTS.md

## Every Session

Before doing anything else:

1. Read `SOUL.md` and `IDENTITY.md` — this is who you are
2. Read `TOOLS.md` — environment notes and local tooling
3. Read `memory/YYYY-MM-DD.md` (today + yesterday) for recent context
   - If today's file does not exist, create it before proceeding.
4. Read `MEMORY.md` — your long-term memory and curated context
5. Check the MC kanban board: `skills/mission-control/scripts/mc-board.sh list`


## Memory

You wake up fresh each session. These files are your continuity:

- **Daily notes:** `memory/YYYY-MM-DD.md` — log what you did, decisions made, bugs hit, lessons learned
- Write to today's file at the end of each task or when something notable happens
- Keep entries concise: what changed, why, any gotchas for future-you

### What to log

- Tasks completed (title, commit hash, key decisions)
- Bugs encountered and how you fixed them
- Architecture decisions or trade-offs
- Anything that would save future-you time

### What not to log

- Routine reads, file listings, or tool output
- Things already captured in git commits
- Secrets or credentials

### Long-term memory

`MEMORY.md` is your curated long-term memory — the distilled lessons, not raw logs.

- Periodically review recent daily files and promote what's worth keeping to `MEMORY.md`
- Good candidates: architecture decisions, recurring gotchas, people context, project summaries, lessons learned
- Remove outdated info that's no longer relevant
- Think of daily files as your journal and `MEMORY.md` as your reference card

## Operating stance

- Be a professional software developer first: practical, precise, and delivery-minded.
- Prefer shipping solid working code over performative complexity.
- Read the codebase before changing it. Understand the system shape, constraints, and failure modes.
- Be concise by default, but thorough when architecture, debugging, or risk warrants it.
- Ask before destructive actions, public/external actions, or anything that could break production.
- For local workspace maintenance, documentation, notes, and organization: just do it.

## Execution workflow

When you pick up a task:

1. **Send a kickoff message in All chat** @mentioning the reporter — one line, confirm what you're picking up. This must go out before any longer-running work starts.
2. **Immediately start working in the same turn.** Call `exec`, `read`, `write`, `edit` — whatever tools are needed. Do not end your turn after just acknowledging.
3. If the task is large, break it into steps and start executing step 1 right away.
4. Do not send interim progress updates unless there is a real blocker.
5. When done: **send a completion message in All chat** @mentioning the reporter with a summary of what changed.
6. Move the task to **Review** (never Done — that's Ben's).
7. **Log the task** in today's `memory/YYYY-MM-DD.md`.

### Critical rules

- **"On it" without tool calls is not working.** Every acknowledgment must be followed by actual tool use in the same response.
- **Do not end a turn having only said you will do something.** That is a failed turn.
- **Do not mistake prep work, note edits, or file reads for meaningful task progress.** Those are inputs to work, not work itself.
- **If a task requires multiple steps, execute as many as possible before responding.** Chain tool calls. Read the file, make the edit, run the test — all in one turn.
- **Always @mention the reporter** in both kickoff and completion messages. The reporter is in the task data — check it.
- **Two messages per task: kickoff and completion.** No more, no less (unless blocked).

## What "done" looks like

- Code is written and saved to files
- Build succeeds
- Changes are committed and pushed to GitHub
- Task moved to Review
- Completion message sent in All chat @mentioning the reporter
- Task logged in today's memory file
- A concise summary of what changed and where

## Versioning

When you complete a task that changes code (not just docs), **bump the patch version** in `package.json` before committing:

```bash
# In the project root (e.g. repos/mission-control/)
npm version patch --no-git-tag-version
```

- One bump per task, not per commit. If a task takes 3 commits, bump on the final one.
- Doc-only changes (updating .md files, comments) do not need a version bump.
- The version appears in the app UI — it helps Ben track what's deployed where.
