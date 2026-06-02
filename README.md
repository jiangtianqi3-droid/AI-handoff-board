# Agent Handoff Board

A lightweight Markdown protocol for AI coding agents to preserve repository
context across sessions, quota limits, interrupted workflows, and multiple
devices.

一个轻量级、基于 Markdown 的 AI 编码助手交接协议，用仓库内文件保存上下文，
让 Codex、Claude Code 和其他 AI 编码 Agent 在切换 session、额度耗尽、设备切换
或中断后继续工作。

## Core Idea

This project separates stable project intent from short-term agent handoff.

`PROJECT_BRIEF.md` explains where the project is going.
`HANDOFF.md` explains what the last agent just changed.

本项目将长期项目意图和短期 AI 交接记录分离。

`PROJECT_BRIEF.md` 说明项目往哪里走。
`HANDOFF.md` 说明上一位 AI 刚刚改了什么。

## Why This Exists

AI coding agents are useful, but their context is fragile. When a user switches
from Codex to Claude Code, starts a new session, hits a quota limit, or edits
from another computer, the next agent can lose the current goal and the latest
work state.

Agent Handoff Board keeps that context in the repository instead of relying on
conversation history.

## Startup Chain

This project does not rely on skills alone for startup automation.

`AGENTS.md` and `CLAUDE.md` are the startup bootstraps.
`SKILL.md` defines the detailed workflow.
`PROJECT_BRIEF.md` stores stable project intent.
`HANDOFF.md` stores bounded short-term handoff state.

本项目不只依赖 Skill 自动触发。
真正的启动入口是 `AGENTS.md` 和 `CLAUDE.md`。
Skill 负责详细流程。
`PROJECT_BRIEF.md` 保存长期稳定的项目意图。
`HANDOFF.md` 保存有上限的短期交接状态。

The intended startup flow is:

```text
AI starts in repository
↓
Read AGENTS.md / CLAUDE.md
↓
Read .ai-handoff/PROJECT_BRIEF.md
↓
Read .ai-handoff/HANDOFF.md
↓
If HANDOFF.md contains another agent's unfinalized Active Record:
    finalize it as interrupted / inherited
↓
Create current agent's Active Record
↓
Start work
↓
Update HANDOFF.md after meaningful changes
↓
Finalize Active Record before stopping, handoff, quota exhaustion, or interruption
```

## Quick Install

Copy these files into a repository:

```text
AGENTS.md
CLAUDE.md
.ai-handoff/PROJECT_BRIEF.md
.ai-handoff/HANDOFF.md
.agents/skills/ai-handoff/SKILL.md
.claude/skills/ai-handoff/SKILL.md
templates/PROJECT_BRIEF_TEMPLATE.md
templates/HANDOFF_TEMPLATE.md
```

Then start Codex or Claude Code from the repository root.

The startup files instruct the agent to read:

1. `.ai-handoff/PROJECT_BRIEF.md`
2. `.ai-handoff/HANDOFF.md`

## Files

- `AGENTS.md`: startup instructions for Codex and other agents that read
  repository agent files.
- `CLAUDE.md`: Claude Code startup file. Its first line imports `AGENTS.md`.
- `.agents/skills/ai-handoff/SKILL.md`: Codex skill workflow.
- `.claude/skills/ai-handoff/SKILL.md`: Claude Code skill workflow.
- `.ai-handoff/PROJECT_BRIEF.md`: stable project intent for AI agents.
- `.ai-handoff/HANDOFF.md`: bounded short-term handoff board.
- `templates/PROJECT_BRIEF_TEMPLATE.md`: canonical project brief template.
- `templates/HANDOFF_TEMPLATE.md`: canonical handoff template.
- `examples/PROJECT_BRIEF.example.md`: realistic project brief example.
- `examples/HANDOFF.example.md`: handoff example with interrupted takeover.

## PROJECT_BRIEF.md

`PROJECT_BRIEF.md` is for long-term project context. It explains why the
project exists, where it is going, who it is for, and what boundaries matter.

It is not a README, not a changelog, not a file map, and not a task list.

Only update `PROJECT_BRIEF.md` when the project's long-term purpose, design
direction, target users, or major constraints change.

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

## HANDOFF.md

`HANDOFF.md` is for short-term transition state. It records what the current AI
is doing, what changed, which files were touched, which commands or tests ran,
what risks remain, and where the next agent should continue if interrupted.

It is a bounded sliding window:

- 1 Active Record
- up to 4 Finalized Records
- max 5 records total

During work, update only the current Active Record. Do not append fragmented
micro-logs.

Before stopping, handoff, quota exhaustion, or interruption, convert the Active
Record into the newest Finalized Record and reset the Active Record to
`Status: none`.

## Status Values

`HANDOFF.md` supports these statuses:

```text
none | active | interrupted | inherited | completed | blocked
```

- `none`: no active record exists.
- `active`: the current agent is working.
- `interrupted`: the previous agent's work stopped before normal finalization.
- `inherited`: the current agent took over another agent's unfinished work.
- `completed`: the task finished normally.
- `blocked`: the task cannot continue without outside input or a state change.

## Active Record Takeover

If an Active Record exists from another agent, treat it as interrupted work.

Do not overwrite it immediately.

First preserve it by converting it into a Finalized Record with
`Status: interrupted` or `Status: inherited`.

Then create a new Active Record for the current agent.

中文含义：如果当前 AI 接手时发现 Active Record 是另一个 Agent 留下的，说明上一
个 Agent 可能因为额度、session 中断、设备切换等原因没有完成交接。当前 AI 不得
直接覆盖该 Active Record。必须先把它转成 Finalized Record，状态标记为
`interrupted` 或 `inherited`，然后再创建当前 AI 自己的 Active Record。

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

## Why Not Only Skills

Skills are useful, but startup behavior can vary by tool, installation, and
session. This project keeps the startup rule in repository files first:

- `AGENTS.md` tells Codex and compatible agents what to read.
- `CLAUDE.md` imports `AGENTS.md` as its first line for Claude Code.
- `SKILL.md` gives the detailed handoff workflow when the skill is available.
- `PROJECT_BRIEF.md` gives stable direction.
- `HANDOFF.md` gives the latest bounded transition state.

This makes the protocol usable even when a skill is missing or not triggered.

## Security

Never write sensitive information into `PROJECT_BRIEF.md`, `HANDOFF.md`,
examples, templates, skills, or startup instruction files.

Do not store:

- API keys
- tokens
- passwords
- credentials
- private URLs
- sensitive personal data
- private business data or proprietary text

If command output contains secrets or private data, summarize only the safe
result instead of copying the output.

## Future Optional Features

The first version intentionally stays Markdown-based and dependency-free.

Future optional helpers could include:

- `handoff-init`
- `handoff-finalize`
- `handoff-prune`
- `handoff-status`

Local machine notes can be reserved under:

```text
.ai-handoff/local/DEVICE_NOTES.md
```

That file can record local OS, available tools, missing tools, or local path
issues. `.ai-handoff/local/` is ignored so device-specific notes do not enter
Git.

Optional scripts should support the protocol, not replace the human-readable
Markdown files.
