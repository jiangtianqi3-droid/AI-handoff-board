# AI Project Brief

This file is for AI coding agents.
It describes the stable project intent, long-term direction, and boundaries.
It is not a changelog, not a README, and not a file map.

---

## 1. Project Purpose

Provide a bounded, repository-local handoff protocol for AI coding agents so
Codex, Claude Code, and other tools can continue work across sessions, quota
limits, devices, and interrupted workflows without losing context.

---

## 2. Long-Term Goal

Make AI-assisted coding handoff reliable, lightweight, copyable, and
tool-agnostic.

---

## 3. Current Direction

Stay Markdown-based and dependency-free.

Use startup files to make agents read stable context before editing.

Separate long-term project direction from short-term transition state.

---

## 4. Target Users

- Developers who switch between Codex and Claude Code.
- Users working across multiple computers.
- Small teams using AI coding agents.
- AI agents that need stable repository-local context.

---

## 5. Core Design Principles

- Keep the protocol lightweight.
- Prefer repository-local files.
- Avoid relying on conversation history.
- Separate long-term project intent from short-term handoff logs.
- Keep AI-readable files short and stable.
- Do not turn this into an infinite logging system.

---

## 6. Non-Goals

- This is not a task manager.
- This is not a project management dashboard.
- This is not a database-backed sync system.
- This is not a replacement for README.md.
- This is not a replacement for Git history.
- This is not a place for secrets.
- This should not require a database or server.

---

## 7. Major Constraints

- Must work across Codex and Claude Code.
- Must work across multiple devices through Git.
- Must keep HANDOFF.md bounded.
- Must not require the user to manually remind the AI to read context.
- Must not depend on large generated summaries every session.

---

## 8. Update Policy

Only update this file when the project's long-term purpose, design direction,
target users, or major constraints change.

Do not update this file for normal bug fixes, README edits, template edits,
example edits, or one-off experiments.

If uncertain, update HANDOFF.md instead.

---

## 9. Last Major Direction Change

- Date: 2026-06-02
- Changed by: Codex
- Reason: Add stable AI project context for multi-agent and multi-device use.
- Summary: Introduced `PROJECT_BRIEF.md` as the long-term project intent layer
  and kept `HANDOFF.md` focused on bounded short-term handoff state.
