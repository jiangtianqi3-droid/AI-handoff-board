# Agent Handoff Protocol

Before doing any work in this repository, you must read:

`.ai-handoff/HANDOFF.md`

If the file does not exist, create it from:

`templates/HANDOFF_TEMPLATE.md`

## Mandatory Workflow

1. Read `.ai-handoff/HANDOFF.md` before editing code or documentation.
2. Do not ask the user to repeat context already present in `HANDOFF.md`.
3. Maintain exactly one Active Record while working.
4. Update the Active Record after meaningful changes, decisions, test runs,
   failures, or newly discovered risks.
5. Do not append fragmented micro-log entries.
6. On completion, interruption, quota exhaustion, or handoff, finalize the
   Active Record.
7. Keep only the latest four Finalized Records.
8. Delete older Finalized Records when the limit is exceeded.

## Record Requirements

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

Never write secrets, API keys, tokens, passwords, credentials, private URLs,
sensitive personal data, or proprietary data into `HANDOFF.md`.
