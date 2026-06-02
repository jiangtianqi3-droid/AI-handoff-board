# AI Project Brief

This file is for AI coding agents.
It describes stable project intent, long-term direction, and boundaries.
It is not a changelog, README, task list, or file map.

---

## Purpose

Provide a bounded, repository-local handoff protocol so Codex, Claude Code, and other AI coding agents can continue work across sessions, quota limits, devices, and interrupted workflows without losing context.

## Long-Term Goal

Make AI-assisted coding handoff reliable, lightweight, copyable, and tool-agnostic.

## Direction

- Remain a lightweight Markdown-based protocol.
- Use `AGENTS.md` and `CLAUDE.md` as startup bootstraps.
- Use `PROJECT_BRIEF.md` for stable project intent.
- Use `HANDOFF.md` for short-term transition state.
- Avoid heavy frameworks, databases, background services, and unnecessary dependencies.

## Users

- Developers who switch between Codex and Claude Code.
- Users working across multiple computers.
- Small teams using AI coding agents.
- AI agents that need stable repository-local context.

## Boundaries

- Not a task manager.
- Not a dashboard.
- Not a database-backed sync system.
- Not a replacement for README.md.
- Not a replacement for Git history.
- Not a place for secrets or private data.

## Constraints

- Must work across Codex and Claude Code.
- Must work across multiple devices through Git.
- Must keep `HANDOFF.md` bounded.
- Must not depend on large generated summaries every session.

## Update Policy

Only update this file when long-term purpose, design direction, target users, or major constraints change.

Do not update it for normal bug fixes, small refactors, README edits, template edits, example edits, ordinary feature work, or one-off experiments.

If uncertain, update `HANDOFF.md` instead.

## Last Major Direction Change

- Date: 2026-06-02
- Reason: Added `PROJECT_BRIEF.md` as the long-term AI project context layer.
