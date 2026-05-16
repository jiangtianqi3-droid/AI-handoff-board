@AGENTS.md

# Claude Code Handoff Instructions

Claude Code must follow the imported `AGENTS.md` handoff protocol.

Before editing files, Claude Code should read `.ai-handoff/HANDOFF.md`. If the file does not exist, create it from `templates/HANDOFF_TEMPLATE.md`.

While working, Claude Code should maintain the single Active Record and update it after meaningful changes, decisions, test runs, or newly discovered risks.

Before stopping, completing the task, or handing work to another agent, Claude Code should finalize the Active Record and prune Finalized Records so only the latest four remain.

Claude Code should not ask the user to repeat context that is already present in `.ai-handoff/HANDOFF.md`.
