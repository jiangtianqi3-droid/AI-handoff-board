# Agent Handoff Protocol

Before doing any work in this repository, read `.ai-handoff/HANDOFF.md`.

If `.ai-handoff/HANDOFF.md` does not exist, create it from `templates/HANDOFF_TEMPLATE.md` before editing project files.

## Required Workflow

1. Read `.ai-handoff/HANDOFF.md` before making code or documentation changes.
2. Do not ask the user to repeat context already present in `HANDOFF.md`.
3. Maintain exactly one Active Record while working.
4. Update the Active Record after meaningful changes, decisions, test runs, or newly discovered risks.
5. Do not append fragmented micro-log entries. Keep the Active Record concise, current, and specific.
6. On completion, interruption, quota exhaustion, or handoff, finalize the Active Record.
7. Keep only the latest four Finalized Records.
8. Delete older Finalized Records from `HANDOFF.md` when the limit is exceeded.

## Active Record Requirements

The Active Record must include:

- current task
- current AI agent name
- start time and last update time
- files touched
- specific changes made
- commands or tests run
- test results
- current risks or unresolved problems
- next step if interrupted

Use concrete details. Do not write vague notes such as "updated code" or "fixed issue" without naming what changed and where.

## Finalized Record Requirements

When stopping, convert the Active Record into a Finalized Record that preserves the useful context for the next agent. Then reset the Active Record to `Status: none`.

Finalized Records must remain bounded:

- keep up to four Finalized Records
- keep the newest records
- remove the oldest records first

## Security Rules

Do not store secrets or private data in `.ai-handoff/HANDOFF.md`.

Never write:

- API keys
- tokens
- passwords
- credentials
- private URLs
- sensitive personal data
- private business data or proprietary text

If sensitive content appears in command output, summarize only the safe result.
