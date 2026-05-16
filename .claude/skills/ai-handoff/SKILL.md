---
name: ai-handoff
description: Use at the start, continuation, interruption, or completion of coding work in a repository to read and update .ai-handoff/HANDOFF.md. This skill maintains a bounded handoff board for Codex, Claude Code, and other AI coding agents.
---

# AI Handoff Skill

## Purpose

Use this skill to preserve concise, actionable project state across AI coding
agents and sessions.

The handoff file is:

```text
.ai-handoff/HANDOFF.md
```

This file is a bounded handoff board.

It is not an append-only log.

It keeps one Active Record and up to four Finalized Records.

## Startup Procedure

1. Before editing, read `.ai-handoff/HANDOFF.md`.
2. If `.ai-handoff/HANDOFF.md` is missing, create it from
   `templates/HANDOFF_TEMPLATE.md`.
3. Review the Active Record.
4. Review the latest Finalized Records.
5. Continue from the recorded next step when it is relevant.
6. Do not ask the user to repeat context already present in `HANDOFF.md`.
7. If there is no active work, create or refresh the Active Record for the
   current task.

## Working Procedure

Maintain exactly one Active Record while working.

Update only the Active Record during work.

Update it after meaningful changes, decisions, test runs, failures, or newly
discovered risks.

Do not create noisy micro-logs.

Keep the Active Record concise, current, and specific.

## Stop / Handoff Procedure

On completion, interruption, quota exhaustion, or handoff:

1. Update the Active Record with the latest task state.
2. Include changed files, concrete changes, commands, tests, results, risks,
   and the next step if interrupted.
3. Move the Active Record into the top of Finalized Records.
4. Mark the finalized record as completed, interrupted, or handed off.
5. Reset the Active Record to `Status: none`.
6. Keep only the latest four Finalized Records.
7. Delete older Finalized Records when the limit is exceeded.

## Retention Policy

`HANDOFF.md` must contain:

- one Active Record
- up to four Finalized Records
- max five records total

The Active Record is the only record updated during work.

Finalized Records are the latest completed, interrupted, or handed-off states.

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

## Security Rules

Never write secrets or private data into `HANDOFF.md`.

Do not write:

- API keys
- tokens
- passwords
- credentials
- private URLs
- sensitive personal data
- proprietary business data

If command output contains sensitive content, summarize only the safe result.

## HANDOFF.md Template

```markdown
# AI Handoff Board

This file keeps one active record and up to four finalized records.

## Rules

- Read this file before editing code.
- Maintain only one Active Record while working.
- Move the Active Record to Finalized Records when stopping or handing off.
- Keep only the latest four Finalized Records.
- Delete older Finalized Records from this file.
- Do not store secrets, tokens, passwords, credentials, private URLs, or sensitive personal data.

---

## Active Record

Status: none

### Agent

None

### Started At

None

### Last Updated

None

### Current Task

None

### Current Changes

None

### Files Touched

None

### Commands / Tests Run

None

### Current Problems / Risks

None

### Next Step If Interrupted

None

---

## Finalized Records

None.

---
```
