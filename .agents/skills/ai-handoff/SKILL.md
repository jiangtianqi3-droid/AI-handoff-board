---
name: ai-handoff
description: Use at the start, continuation, interruption, or completion of coding work in a repository to read and update .ai-handoff/PROJECT_BRIEF.md and .ai-handoff/HANDOFF.md. This skill maintains stable project intent and bounded handoff state for Codex, Claude Code, and other AI coding agents.
---

# AI Handoff Skill

## Purpose

Maintain two repository-local AI context files:

- `.ai-handoff/PROJECT_BRIEF.md` for stable project intent.
- `.ai-handoff/HANDOFF.md` for bounded short-term handoff state.

The goal is to let Codex, Claude Code, and other AI coding agents continue work
across sessions, quota limits, devices, and interrupted workflows without
depending on conversation history.

## Startup Procedure

1. Read `.ai-handoff/PROJECT_BRIEF.md`.
2. Read `.ai-handoff/HANDOFF.md`.
3. If `.ai-handoff/PROJECT_BRIEF.md` is missing, create it from
   `templates/PROJECT_BRIEF_TEMPLATE.md`.
4. If `.ai-handoff/HANDOFF.md` is missing, create it from
   `templates/HANDOFF_TEMPLATE.md`.
5. If another agent's Active Record exists, finalize it as `interrupted` or
   `inherited`.
6. Create a new Active Record for the current agent.
7. Do not ask for context already present in `PROJECT_BRIEF.md` or
   `HANDOFF.md`.

For multi-device work, also check the current branch, check
`git status --short`, and fetch or pull the latest remote changes before
editing when safe. Do not assume the local checkout is up to date. If local
uncommitted changes exist, do not blindly pull.

## Working Procedure

- Maintain exactly one Active Record.
- Update only the current Active Record during work.
- Replace stale details instead of adding micro-logs.
- Do not create noisy micro-logs.
- Record meaningful changes, touched files, commands, tests, failures, risks,
  and the next step.
- Keep entries concrete enough for the next agent to continue.
- Update `PROJECT_BRIEF.md` only when long-term project intent changes.

## Stop / Handoff Procedure

On completion, interruption, quota exhaustion, or handoff:

1. Convert the current Active Record into a Finalized Record.
2. Put it at the top of Finalized Records.
3. Mark it `completed`, `interrupted`, `inherited`, or `blocked`.
4. Reset Active Record to `Status: none`.
5. Keep only the latest four Finalized Records.
6. Delete older Finalized Records.
7. Commit changes when appropriate.
8. Push changes when the remote is available and credentials are already
   configured.

Never ask for, print, or store credentials.

## Active Record Takeover Procedure

If an Active Record exists from another agent, treat it as interrupted work.

Do not overwrite it immediately.

First preserve it by converting it into a Finalized Record with
`Status: interrupted` or `Status: inherited`.

Then create a new Active Record for the current agent.

Use `Status: inherited` when the current agent is explicitly continuing the
unfinished work. Use `Status: interrupted` when the previous work was stopped
and preserved only for context.

## Retention Policy

`HANDOFF.md` must contain at most:

- one Active Record
- four Finalized Records
- five records total

Finalized Records are ordered newest first.

## Required Record Fields

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

### Status

none | active | interrupted | inherited | completed | blocked

- `none`: no active record exists.
- `active`: the current agent is working.
- `interrupted`: the previous agent's work stopped before normal finalization.
- `inherited`: the current agent took over another agent's unfinished work.
- `completed`: the task finished normally.
- `blocked`: the task cannot continue without outside input or a state change.

## PROJECT_BRIEF.md Policy

`PROJECT_BRIEF.md` stores stable project intent, long-term direction, target
users, boundaries, non-goals, and major constraints.

Only update it when the project's long-term purpose, design direction, target
users, or major constraints change.

Do not update it for:

- normal bug fixes
- small refactors
- dependency updates
- README edits
- template edits
- example edits
- ordinary feature work
- one-off experiments

If uncertain, update `HANDOFF.md` instead.

## Security Rules

Never write secrets or private data into `PROJECT_BRIEF.md`, `HANDOFF.md`,
templates, examples, or agent instructions.

Do not write API keys, tokens, passwords, credentials, private URLs, sensitive
personal data, or proprietary business data.

## HANDOFF.md Template

Use `templates/HANDOFF_TEMPLATE.md` as the canonical handoff template.

The template must stay empty by default:

- Active Record set to `Status: none`
- no real project maintenance records
- `Finalized Records` set to `None.`

## PROJECT_BRIEF.md Template

Use `templates/PROJECT_BRIEF_TEMPLATE.md` as the canonical project brief
template.

Do not include detailed file structure, short-term logs, ordinary changelog
entries, secrets, or private data.
