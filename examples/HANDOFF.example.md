# AI Handoff Board

This file keeps one active record and up to four finalized records.

`PROJECT_BRIEF.md` explains stable project intent.
`HANDOFF.md` explains what the last agent just changed.

## Rules

- Read `.ai-handoff/PROJECT_BRIEF.md` before reading this file.
- Read `.ai-handoff/HANDOFF.md` after the project brief.
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

## Takeover Scenario

This example shows Claude Code taking over after Codex left an unfinalized
Active Record.

The previous Active Record looked like this before takeover:

```text
Status: active
Agent: Codex
Current Task: Fix reranker score normalization.
Next Step If Interrupted: Preserve original input order for equal combined scores.
```

Claude Code did not overwrite that record. It first preserved Codex's work as
the newest Finalized Record with `Status: interrupted`, then created the current
Active Record below with `Status: inherited`.

The next AI can continue from this file without asking the user to repeat what
happened.

---

## Active Record

Status: inherited

### Agent

Claude Code

### Started At

2026-06-02 10:25 CST

### Last Updated

2026-06-02 10:45 CST

### Current Task

Continue the reranker stability fix that Codex started before its session was
interrupted.

### Files Touched

- `src/reranker.py`
- `tests/test_reranker.py`

### Specific Changes Made

Preserved Codex's unfinished Active Record as an interrupted Finalized Record
before creating this new Active Record.

Updated the sort key so equal normalized scores preserve original input order.

### Commands / Tests Run

- `pytest tests/test_reranker.py`

### Test Results

The reranker tests pass locally.

### Current Problems / Risks

Only focused reranker tests have been run. Broader ranking tests may still need
coverage.

### Next Step If Interrupted

Run the full test suite and check whether any ranking expectations changed.

---

## Finalized Records

### 2026-06-02 10:24 CST - Codex - interrupted

#### Status

interrupted

#### Agent

Codex

#### Started At

2026-06-02 09:50 CST

#### Last Updated

2026-06-02 10:20 CST

#### Current Task

Fix reranker score normalization so dense and keyword scores are comparable.

#### Files Touched

- `src/reranker.py`
- `tests/test_reranker.py`

#### Specific Changes Made

Added normalization coverage for mixed positive and negative dense scores.

Started changing the combined score calculation, but did not finalize before
the session ended.

#### Commands / Tests Run

- `pytest tests/test_reranker.py -k normalization`

#### Test Results

The focused normalization test passed.

#### Current Problems / Risks

Equal normalized scores may still reorder documents because original input
order is not part of the sort key.

#### Next Step If Interrupted

Continue near the final sort operation in `src/reranker.py` and preserve input
order for equal combined scores.

### 2026-06-02 08:30 CST - Claude Code - completed

#### Status

completed

#### Agent

Claude Code

#### Started At

2026-06-02 08:00 CST

#### Last Updated

2026-06-02 08:30 CST

#### Current Task

Add tests that document expected reranker behavior before changing the
implementation.

#### Files Touched

- `tests/test_reranker.py`

#### Specific Changes Made

Created focused tests for empty input, single-document input, keyword-only
scoring, and deterministic ordering for equal scores.

#### Commands / Tests Run

- `pytest tests/test_reranker.py`

#### Test Results

Passed.

#### Current Problems / Risks

Dense score normalization remained unimplemented after this step.

#### Next Step If Interrupted

Implement dense score normalization in `src/reranker.py` while keeping the new
tests passing.

---
