---
name: ai-handoff
description: >-
  Use at the start, continuation, interruption, or completion of coding work in
  a repository to read and update .ai-handoff/HANDOFF.md. This skill maintains
  a bounded handoff board for Codex, Claude Code, and other AI coding agents.
---

# AI Handoff Skill

## Purpose

Maintain `.ai-handoff/HANDOFF.md` as a bounded handoff board:

- one Active Record
- up to four Finalized Records
- no append-only logs

## Startup Procedure

1. Read `.ai-handoff/HANDOFF.md` before editing.
2. If missing, create it from `templates/HANDOFF_TEMPLATE.md`.
3. Continue from the Active Record when present.
4. If no Active Record exists, create one for the current task.
5. Do not ask for context already present in `HANDOFF.md`.

## Working Procedure

- Maintain exactly one Active Record.
- Update only the Active Record during work.
- Replace stale details instead of adding micro-logs.
- Record meaningful changes, decisions, test runs, failures, and risks.
- Keep entries concrete enough for the next agent to continue.

## Stop / Handoff Procedure

On completion, interruption, quota exhaustion, or handoff:

1. Update the Active Record with the latest state.
2. Include files, changes, commands, results, risks, and next step.
3. Move it to the top of Finalized Records.
4. Mark it completed, interrupted, or handed off.
5. Reset Active Record to `Status: none`.
6. Keep only the latest four Finalized Records.
7. Delete older Finalized Records.

## Retention Policy

Maximum contents:

- one Active Record
- four Finalized Records
- five records total

## Required Record Fields

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

## Security Rules

Never write secrets or private data into `HANDOFF.md`.

Do not write API keys, tokens, passwords, credentials, private URLs, sensitive
personal data, or proprietary business data.

## HANDOFF.md Template

Use `templates/HANDOFF_TEMPLATE.md` as the canonical template.

Do not inline the full template unless both handoff files are missing.
