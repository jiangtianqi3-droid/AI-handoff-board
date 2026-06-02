# AI Handoff Board

This file keeps one active record and up to four finalized records.

`PROJECT_BRIEF.md` explains stable project intent.
`HANDOFF.md` explains what the last agent just changed.

## Rules

- Read `.ai-handoff/PROJECT_BRIEF.md` before reading this file.
- Maintain only one Active Record while working.
- If another agent's Active Record exists, preserve it as interrupted or
  inherited before creating the current agent's Active Record.
- Move the current Active Record to Finalized Records when stopping or handing
  off.
- Keep only the latest four Finalized Records.
- Delete older Finalized Records from this file.
- Do not store secrets, tokens, passwords, credentials, private URLs, or
  sensitive personal data.

### Status

none | active | interrupted | inherited | completed | blocked

- `none`: no active record exists.
- `active`: the current agent is working.
- `interrupted`: the previous agent's work stopped before normal finalization.
- `inherited`: the current agent took over another agent's unfinished work.
- `completed`: the task finished normally.
- `blocked`: the task cannot continue without outside input or a state change.

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

### Files Touched

None

### Specific Changes Made

None

### Commands / Tests Run

None

### Test Results

None

### Current Problems / Risks

None

### Next Step If Interrupted

None

---

## Finalized Records

None.

---
