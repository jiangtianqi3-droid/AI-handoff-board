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

Status: active

### Agent

Claude Code

### Started At

2026-05-16 14:40 CST

### Last Updated

2026-05-16 15:10 CST

### Current Task

Continue fixing reranker score normalization after Codex handed off with one
failing pytest case.

### Current Changes

Updated `tests/test_reranker.py` to make the equal-score ordering expectation
explicit.

Started changing `src/reranker.py` so equal normalized scores preserve the
original input order as the final tie-breaker.

### Files Touched

- `src/reranker.py`
- `tests/test_reranker.py`

### Commands / Tests Run

- `pytest tests/test_reranker.py`: still failing
  `test_equal_score_order_is_stable`.

### Current Problems / Risks

The sort key now includes score and document id, but it still does not preserve
the original input index.

Changing the sort key may affect existing ranking expectations for documents
with equal combined scores.

### Next Step If Interrupted

In `src/reranker.py`, capture each document's original index before scoring.

Use that index as the final tie-breaker for equal combined scores.

Then re-run:

```text
pytest tests/test_reranker.py
```

---

## Finalized Records

### 2026-05-16 14:35 CST - Codex - handed off

#### Agent

Codex

#### Started At

2026-05-16 13:55 CST

#### Last Updated

2026-05-16 14:35 CST

#### Current Task

Fix reranker score normalization so dense and keyword scores are comparable.

#### Current Changes

Changed `src/reranker.py` to normalize dense scores into a 0..1 range before
combining them with keyword scores.

Added regression coverage in `tests/test_reranker.py` for mixed positive and
negative dense score inputs.

#### Files Touched

- `src/reranker.py`
- `tests/test_reranker.py`

#### Commands / Tests Run

- `pytest tests/test_reranker.py -k normalization`: passed.
- `pytest tests/test_reranker.py`: failed one stable-ordering test.

#### Current Problems / Risks

Equal normalized scores are not stable.

The current implementation may reorder documents with identical combined scores.

#### Next Step If Interrupted

Continue near the final sort operation in `src/reranker.py`.

Preserve original input order when combined scores are equal.

### 2026-05-16 12:10 CST - Claude Code - completed

#### Agent

Claude Code

#### Started At

2026-05-16 11:45 CST

#### Last Updated

2026-05-16 12:10 CST

#### Current Task

Add tests that document expected reranker behavior before changing the
implementation.

#### Current Changes

Created focused tests for empty input, single-document input, keyword-only
scoring, and deterministic ordering for equal scores.

#### Files Touched

- `tests/test_reranker.py`

#### Commands / Tests Run

- `pytest tests/test_reranker.py`: passed before normalization changes.

#### Current Problems / Risks

Dense score normalization remained unimplemented after this step.

#### Next Step If Interrupted

Implement dense score normalization in `src/reranker.py` while keeping the new
tests passing.

### 2026-05-16 10:30 CST - Codex - completed

#### Agent

Codex

#### Started At

2026-05-16 10:05 CST

#### Last Updated

2026-05-16 10:30 CST

#### Current Task

Map the reranker code path and identify the safest place to add normalization.

#### Current Changes

Reviewed `src/reranker.py` and confirmed scores are combined in a single helper
before sorting.

No production code was changed during this step.

#### Files Touched

- `src/reranker.py`

#### Commands / Tests Run

- `pytest tests/test_reranker.py`: passed.

#### Current Problems / Risks

Normalization must avoid division by zero when all dense scores are equal.

#### Next Step If Interrupted

Add tests for equal dense scores and negative dense scores before changing the
normalization helper.

---
