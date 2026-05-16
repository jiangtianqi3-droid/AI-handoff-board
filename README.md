# Agent Handoff Board

A bounded handoff board for AI coding agents, keeping only the current working note and the latest four transition records.

一个有容量上限的 AI 编码助手交接白板，只保留当前工作记录和最近四条交接记录，避免 Codex、Claude Code 等工具切换时丢失上下文。

## Why This Exists

AI coding agents are useful, but their context is fragile. When you switch tools, start a new session, hit a usage limit, or hand work from Codex to Claude Code and back again, the next agent often does not know what just changed, which tests were run, or where the work stopped.

Agent Handoff Board solves that with a small repository-local file:

```text
.ai-handoff/HANDOFF.md
```

Each agent reads this file before editing code, keeps it updated while working, and finalizes the current record before stopping. The next agent can then continue without asking the user to repeat context that is already in the repository.

## Core Idea

- `AGENTS.md` and `CLAUDE.md` are the startup entry points for coding agents.
- `.agents/skills/ai-handoff/SKILL.md` and `.claude/skills/ai-handoff/SKILL.md` contain the detailed workflow.
- `.ai-handoff/HANDOFF.md` is the handoff board.
- `HANDOFF.md` is not an infinite log. It is a bounded sliding window.

## Retention Policy

`HANDOFF.md` must contain:

- one Active Record
- up to four Finalized Records
- max five records total

The active record is the only record updated during a working session. When work completes, is interrupted, or needs to be handed off, the active record is converted into a finalized record. If there are more than four finalized records, delete the oldest finalized records.

## Installation

### Use With Codex

Copy these files into the root of any repository:

```text
AGENTS.md
.ai-handoff/HANDOFF.md
.agents/skills/ai-handoff/SKILL.md
templates/HANDOFF_TEMPLATE.md
```

Codex should read `AGENTS.md` automatically in supported environments. The first instruction in `AGENTS.md` requires Codex to read `.ai-handoff/HANDOFF.md` before coding.

### Use With Claude Code

Copy these files into the root of any repository:

```text
CLAUDE.md
AGENTS.md
.ai-handoff/HANDOFF.md
.claude/skills/ai-handoff/SKILL.md
templates/HANDOFF_TEMPLATE.md
```

Claude Code should read `CLAUDE.md` automatically in supported environments. `CLAUDE.md` imports the common rules from `AGENTS.md` and adds Claude Code-specific handoff requirements.

### Minimal Manual Install

For any AI coding agent, copy at least:

```text
.ai-handoff/HANDOFF.md
templates/HANDOFF_TEMPLATE.md
```

Then instruct the agent to read `.ai-handoff/HANDOFF.md` before editing files and to maintain the bounded handoff format.

## File Format

`HANDOFF.md` uses this structure:

```markdown
# AI Handoff Board

## Active Record

Status: active | none

### Agent
Codex | Claude Code | Other

### Started At
YYYY-MM-DD HH:mm TZ

### Last Updated
YYYY-MM-DD HH:mm TZ

### Current Task
Specific task being worked on.

### Current Changes
Specific changes made so far.

### Files Touched
- path/to/file

### Commands / Tests Run
- command: result

### Current Problems / Risks
Known risks, failures, blockers, or unknowns.

### Next Step If Interrupted
The exact place the next agent should continue.

---

## Finalized Records

### Record 1: YYYY-MM-DD HH:mm TZ - Agent
...
```

## Example

```markdown
## Active Record

Status: active

### Agent
Claude Code

### Started At
2026-05-16 14:40 CST

### Last Updated
2026-05-16 15:05 CST

### Current Task
Continue fixing reranker score normalization after Codex stopped with one failing pytest case.

### Current Changes
Adjusted the tie-breaking branch in `src/reranker.py` and added a regression assertion for equal scores.

### Files Touched
- src/reranker.py
- tests/test_reranker.py

### Commands / Tests Run
- `pytest tests/test_reranker.py`: still failing `test_equal_score_order_is_stable`

### Current Problems / Risks
The stable ordering fix may need to preserve original input index before sorting.

### Next Step If Interrupted
Inspect the sort key in `src/reranker.py` and include original index as the final tie-breaker.
```

See [examples/HANDOFF.example.md](examples/HANDOFF.example.md) for a fuller example.

## Security

Never write sensitive information into `HANDOFF.md`, examples, templates, or agent instructions.

Do not store:

- API keys
- tokens
- passwords
- credentials
- private URLs
- sensitive personal data
- private business data or proprietary text

If a command output contains secrets or private data, summarize the safe result instead of copying the output.

## GitHub Usage

This repository is meant to be copied into other projects. You can use all files as-is, or copy only the files relevant to your agent setup.

Recommended files for most repositories:

```text
AGENTS.md
CLAUDE.md
.ai-handoff/HANDOFF.md
.agents/skills/ai-handoff/SKILL.md
.claude/skills/ai-handoff/SKILL.md
templates/HANDOFF_TEMPLATE.md
```

## Future Optional Scripts

This first version intentionally stays Markdown-based and dependency-free. Future versions could add optional helper scripts:

- `handoff-init`
- `handoff-finalize`
- `handoff-prune`
- `handoff-status`

Those scripts should support the protocol, not replace the human-readable handoff file.
