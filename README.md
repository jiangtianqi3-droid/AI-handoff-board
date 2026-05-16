# Agent Handoff Board

A bounded handoff board for AI coding agents, keeping only the current working
note and the latest four transition records.

一个有容量上限的 AI 编码助手交接白板，只保留当前工作记录和最近四条交接记录，
避免 Codex、Claude Code 等工具切换时丢失上下文。

## Quick Install

Copy these files into your repository:

```text
AGENTS.md
CLAUDE.md
.ai-handoff/HANDOFF.md
.agents/skills/ai-handoff/SKILL.md
.claude/skills/ai-handoff/SKILL.md
templates/HANDOFF_TEMPLATE.md
```

Then tell your AI coding agent:

```text
Before editing, read .ai-handoff/HANDOFF.md.
Maintain the Active Record while working.
Finalize the Active Record before stopping or handing off.
```

## 中文版

Agent Handoff Board 是一个轻量级、基于 Markdown 的 AI 编码助手交接协议。

它适合放进任何代码仓库，让 Codex、Claude Code 和其他 AI 编码 Agent
在轮流接手项目时，可以先读取同一个交接文件，再继续工作。

核心文件是：

```text
.ai-handoff/HANDOFF.md
```

这个文件不是无限增长的日志，而是一个有容量上限的交接白板。

### 为什么需要它

使用 AI 编码助手时，经常会遇到这些情况：

- Codex 当前 session 或额度用完，需要切到 Claude Code。
- Claude Code 工作到一半中断，需要切回 Codex。
- 新开一个 AI session 后，之前的上下文不在了。
- 用户不得不反复解释刚才改了什么、测试跑了哪些、现在卡在哪里。

Agent Handoff Board 的目标是把关键上下文保存在仓库内。

下一个 AI Agent 可以直接读取 `.ai-handoff/HANDOFF.md` 并继续工作，
而不是让用户重新讲一遍。

### 核心思路

- `AGENTS.md` 是 Codex 等 Agent 的启动入口。
- `CLAUDE.md` 是 Claude Code 的启动入口。
- `.agents/skills/ai-handoff/SKILL.md` 是 Codex Skill 工作流。
- `.claude/skills/ai-handoff/SKILL.md` 是 Claude Code Skill 工作流。
- `.ai-handoff/HANDOFF.md` 是真正的交接白板。
- `HANDOFF.md` 是 bounded sliding window，不是无限日志。

### 保留策略

`HANDOFF.md` 必须始终保持有界：

- 1 条 Active Record：当前 AI 正在维护的工作记录
- 最多 4 条 Finalized Records：最近四次完成、中断或交接记录
- 总计最多 5 条记录

工作过程中，AI 只更新 Active Record。

不要追加碎片化 micro-log。

停止、完成、额度耗尽、被打断或需要交接时，
把 Active Record 转成 Finalized Record。

Finalized Records 超过 4 条时，删除最旧记录。

### 安装方式

#### 用于 Codex

把这些文件复制到目标项目根目录：

```text
AGENTS.md
.ai-handoff/HANDOFF.md
.agents/skills/ai-handoff/SKILL.md
templates/HANDOFF_TEMPLATE.md
```

支持 `AGENTS.md` 的环境会让 Codex 在开始工作前读取其中的规则。

`AGENTS.md` 要求 Codex 在任何编码或文档修改前，
必须先读取 `.ai-handoff/HANDOFF.md`。

#### 用于 Claude Code

把这些文件复制到目标项目根目录：

```text
CLAUDE.md
AGENTS.md
.ai-handoff/HANDOFF.md
.claude/skills/ai-handoff/SKILL.md
templates/HANDOFF_TEMPLATE.md
```

Claude Code 会读取 `CLAUDE.md`。

本项目中的 `CLAUDE.md` 第一行导入 `AGENTS.md`，
然后补充 Claude Code 专用规则。

#### 最小安装

如果只想给任意 AI 编码助手使用，可以至少复制：

```text
.ai-handoff/HANDOFF.md
templates/HANDOFF_TEMPLATE.md
```

然后要求 AI：

- 开始编辑前必须读取 `.ai-handoff/HANDOFF.md`。
- 工作中只维护 Active Record。
- 停止前 finalize 当前记录。
- Finalized Records 最多保留 4 条。

### 文件格式

`HANDOFF.md` 的基本结构如下：

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

当前正在处理的具体任务。

### Current Changes

目前已经做出的具体改变。

### Files Touched

- path/to/file

### Commands / Tests Run

- command: result

### Current Problems / Risks

当前风险、失败测试、阻塞点或不确定事项。

### Next Step If Interrupted

如果当前 AI 被打断，下一个 AI 应该从哪里继续。

---

## Finalized Records

### YYYY-MM-DD HH:mm TZ - Agent - completed

...
```

### 简短示例

```markdown
## Active Record

Status: active

### Agent

Claude Code

### Current Task

继续修复 Codex 交接下来的 reranker 分数归一化问题。

### Current Changes

调整了 `src/reranker.py` 中相同分数的排序逻辑，
并在 `tests/test_reranker.py` 中增加回归断言。

### Commands / Tests Run

- `pytest tests/test_reranker.py`: 仍有一个稳定排序测试失败

### Current Problems / Risks

排序逻辑可能需要保留原始输入顺序作为最后的 tie-breaker。

### Next Step If Interrupted

检查 `src/reranker.py` 的 sort key，把原始 index 加入相同分数排序逻辑。
```

更完整的示例见 [examples/HANDOFF.example.md](examples/HANDOFF.example.md)。

### 安全规则

不要把敏感信息写入 `HANDOFF.md`、模板、示例或 Agent 指令文件。

禁止写入：

- API key
- token
- password
- credentials
- private URL
- 敏感个人数据
- 商业私有数据正文

如果命令输出里包含敏感内容，只记录安全总结，不要复制原文。

### 在 GitHub 项目中使用

这个仓库可以作为模板使用。

你可以把这些文件复制到任何项目中：

```text
AGENTS.md
CLAUDE.md
.ai-handoff/HANDOFF.md
.agents/skills/ai-handoff/SKILL.md
.claude/skills/ai-handoff/SKILL.md
templates/HANDOFF_TEMPLATE.md
```

这样，无论 Codex、Claude Code 还是其他 AI 编码助手接手，
都可以通过同一个 `.ai-handoff/HANDOFF.md` 了解当前项目状态。

### 未来可选脚本

第一版故意保持轻量。

它不引入 npm、Python 包、数据库或复杂框架。

未来可以增加可选脚本：

- `handoff-init`
- `handoff-finalize`
- `handoff-prune`
- `handoff-status`

这些脚本应该辅助协议，而不是替代可读的 Markdown 交接文件。

## English Version

### Why This Exists

AI coding agents are useful, but their context is fragile.

When you switch tools, start a new session, hit a usage limit, or hand work
from Codex to Claude Code and back again, the next agent often does not know
what just changed, which tests were run, or where the work stopped.

Agent Handoff Board solves that with a small repository-local file:

```text
.ai-handoff/HANDOFF.md
```

Each agent reads this file before editing code, keeps it updated while working,
and finalizes the current record before stopping.

The next agent can then continue without asking the user to repeat context
that is already in the repository.

### Core Idea

- `AGENTS.md` and `CLAUDE.md` are startup entry points for coding agents.
- Skill files contain the detailed workflow.
- `.ai-handoff/HANDOFF.md` is the handoff board.
- `HANDOFF.md` is not an infinite log.
- `HANDOFF.md` is a bounded sliding window.

### Retention Policy

`HANDOFF.md` must contain:

- one Active Record
- up to four Finalized Records
- max five records total

The Active Record is the only record updated during a working session.

When work completes, is interrupted, hits quota exhaustion, or needs to be
handed off, the Active Record is converted into a Finalized Record.

If there are more than four Finalized Records, delete the oldest records.

### Installation

Use the Quick Install section above for the recommended setup.

For Codex, include:

```text
AGENTS.md
.ai-handoff/HANDOFF.md
.agents/skills/ai-handoff/SKILL.md
templates/HANDOFF_TEMPLATE.md
```

For Claude Code, include:

```text
CLAUDE.md
AGENTS.md
.ai-handoff/HANDOFF.md
.claude/skills/ai-handoff/SKILL.md
templates/HANDOFF_TEMPLATE.md
```

### File Format

`HANDOFF.md` tracks one current working record and up to four finalized
transition records.

Each record should include:

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

### Security

Never write sensitive information into `HANDOFF.md`, examples, templates,
or agent instructions.

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

### GitHub Usage

This repository is meant to be copied into other projects.

Use all files as-is, or copy only the files relevant to your agent setup.

### Future Optional Scripts

This first version intentionally stays Markdown-based and dependency-free.

Future versions could add optional helper scripts:

- `handoff-init`
- `handoff-finalize`
- `handoff-prune`
- `handoff-status`

Those scripts should support the protocol, not replace the human-readable
handoff file.
