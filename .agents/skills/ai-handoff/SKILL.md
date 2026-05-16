---
name: ai-handoff
description: Use at the start, continuation, interruption, or completion of coding work in a repository to read and update .ai-handoff/HANDOFF.md. This skill maintains a bounded handoff board for Codex, Claude Code, and other AI coding agents.
---

# AI Handoff Skill

## Purpose

Use this skill to preserve concise, actionable project state across AI coding agents and sessions. The handoff file is `.ai-handoff/HANDOFF.md`.

The file is a bounded handoff board, not an append-only log. It keeps one Active Record and up to four Finalized Records.

## Startup Procedure

1. Read `.ai-handoff/HANDOFF.md` before editing code.
2. If `.ai-handoff/HANDOFF.md` does not exist, create it from `templates/HANDOFF_TEMPLATE.md`.
3. Review the Active Record and latest Finalized Records.
4. Continue from the recorded next step when it is relevant.
5. Do not ask the user to repeat context that is already present in the handoff file.
6. If there is no active work, create or refresh the Active Record for the current task.

## Working Procedure

Maintain exactly one Active Record while working.

Update the Active Record after meaningful changes, decisions, test runs, or newly discovered risks. Keep it concise and current. Do not add fragmented micro-log entries.

Use concrete descriptions:

- name the files changed
- describe the behavior changed
- include commands or tests run
- include test results
- record unresolved risks
- leave a clear next step if interrupted

## Stop / Handoff Procedure

Before stopping, completing work, handing off, or when interruption is likely:

1. Update the Active Record with the latest files, changes, commands, results, risks, and next step.
2. Move the Active Record into the top of `Finalized Records`.
3. Mark the record as completed, interrupted, or handed off.
4. Reset the Active Record to `Status: none`.
5. Prune Finalized Records so only the latest four remain.

## Retention Policy

`HANDOFF.md` must contain:

- one Active Record
- up to four Finalized Records
- max five records total

Delete the oldest Finalized Records when the limit is exceeded.

## Required Fields

Each Active Record and Finalized Record should include:

- status
- agent
- started at
- last updated
- current task
- current changes
- files touched
- commands or tests run
- current problems or risks
- next step if interrupted

## Security Rules

Never store sensitive information in the handoff file.

Do not write:

- API keys
- tokens
- passwords
- credentials
- private URLs
- sensitive personal data
- private business data or proprietary text

If command output includes sensitive data, summarize only the safe result.

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
```
