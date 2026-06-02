# Agent Handoff Protocol

This repository uses two AI-readable context files:

1. `.ai-handoff/PROJECT_BRIEF.md` for stable project intent.
2. `.ai-handoff/HANDOFF.md` for short-term agent handoff state.

Before editing code or documentation, first read:

1. `.ai-handoff/PROJECT_BRIEF.md`
2. `.ai-handoff/HANDOFF.md`

If `.ai-handoff/PROJECT_BRIEF.md` does not exist, create it from
`templates/PROJECT_BRIEF_TEMPLATE.md`.

If `.ai-handoff/HANDOFF.md` does not exist, create it from
`templates/HANDOFF_TEMPLATE.md`.

## Mandatory Startup Flow

1. Read `AGENTS.md` or `CLAUDE.md`.
2. Read `.ai-handoff/PROJECT_BRIEF.md`.
3. Read `.ai-handoff/HANDOFF.md`.
4. If `HANDOFF.md` contains another agent's unfinalized Active Record, treat it
   as interrupted work.
5. Do not overwrite another agent's Active Record immediately.
6. First preserve it by converting it into a Finalized Record with
   `Status: interrupted` or `Status: inherited`.
7. Create a new Active Record for the current agent.
8. Start work.

## Mandatory Working Flow

1. Maintain exactly one Active Record while working.
2. Update only the current Active Record.
3. Record meaningful changes, touched files, commands, tests, failures, risks,
   and the next step.
4. Do not append fragmented micro-log entries.
5. On completion, interruption, quota exhaustion, or handoff, convert the
   current Active Record into a Finalized Record.
6. Put the newest Finalized Record at the top of Finalized Records.
7. Keep only the latest four Finalized Records.
8. Delete older Finalized Records when the limit is exceeded.

## Status Values

Use only these status values:

```text
none | active | interrupted | inherited | completed | blocked
```

- `none`: no active record exists.
- `active`: the current agent is working.
- `interrupted`: the previous agent's work stopped before normal finalization.
- `inherited`: the current agent took over another agent's unfinished work.
- `completed`: the task finished normally.
- `blocked`: the task cannot continue without outside input or a state change.

## Multi-Device Git Sync Rule

This repository may be edited from multiple computers.

Before making changes:

1. Check the current branch.
2. Check `git status --short`.
3. If a remote is configured, fetch or pull the latest changes before editing
   when safe.
4. Read `.ai-handoff/PROJECT_BRIEF.md`.
5. Read `.ai-handoff/HANDOFF.md`.

Do not assume the local checkout is up to date.

If local uncommitted changes exist, do not blindly pull.

After meaningful changes:

1. Update `.ai-handoff/HANDOFF.md`.
2. Commit changes when appropriate.
3. Push changes when the remote is available and credentials are already
   configured.
4. Never ask for, print, or store credentials.

## Record Requirements

Each Active Record and Finalized Record must include:

- status
- agent
- started at
- last updated
- current task
- files touched
- specific changes made
- commands or tests run
- test results
- current problems or risks
- next step if interrupted

## PROJECT_BRIEF.md Policy

`PROJECT_BRIEF.md` stores stable project intent, long-term direction, target
users, boundaries, non-goals, and major constraints.

Only update it when the project's long-term purpose, design direction, target
users, or major constraints change.

Do not update it for normal bug fixes, small refactors, dependency updates,
README edits, template edits, example edits, ordinary feature work, or one-off
experiments.

If uncertain, update `HANDOFF.md` instead.

## Security Rules

Never write secrets, API keys, tokens, passwords, credentials, private URLs,
sensitive personal data, or proprietary data into `PROJECT_BRIEF.md`,
`HANDOFF.md`, templates, examples, or agent instructions.
