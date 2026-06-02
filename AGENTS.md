# Agent Handoff Protocol

Before editing anything in this repository, you must read:

1. `.ai-handoff/PROJECT_BRIEF.md`
2. `.ai-handoff/HANDOFF.md`

If either file is missing, create it from the matching template:

- `templates/PROJECT_BRIEF_TEMPLATE.md`
- `templates/HANDOFF_TEMPLATE.md`

## Startup Rule

1. Read `.ai-handoff/PROJECT_BRIEF.md`.
2. Read `.ai-handoff/HANDOFF.md`.
3. If another agent's Active Record exists, finalize it as `interrupted` or `inherited`.
4. Create the current agent's Active Record.
5. Start work.

## Working Rule

1. Maintain exactly one Active Record.
2. Update only the current Active Record.
3. Record meaningful changes, touched files, commands, tests, failures, risks, and next step.
4. Do not append fragmented micro-logs.

## Stop / Handoff Rule

1. Convert the current Active Record into a Finalized Record.
2. Put it at the top of Finalized Records.
3. Keep only the latest four Finalized Records.
4. Delete older Finalized Records.
5. Reset Active Record to `Status: none`.

## Status Values

`none | active | interrupted | inherited | completed | blocked`

## Multi-Device Git Sync Rule

Before making changes:

1. Check the current branch.
2. Check `git status --short`.
3. If a remote is configured, fetch or pull the latest changes before editing when safe.
4. Do not assume the local checkout is up to date.
5. If local uncommitted changes exist, do not blindly pull.

After meaningful changes:

1. Update `.ai-handoff/HANDOFF.md`.
2. Commit changes when appropriate.
3. Push changes only when the remote is available and credentials are already configured.
4. Never ask for, print, or store credentials.

## PROJECT_BRIEF.md Policy

Only update `PROJECT_BRIEF.md` when the project's long-term purpose, design direction, target users, or major constraints change.

If uncertain, update `HANDOFF.md` instead.

## Security Rule

Never write secrets, API keys, tokens, passwords, credentials, private URLs, sensitive personal data, or proprietary data into project context files.
