---
name: when-create-local-harness
description: "Use this global skill when creating or updating one local repository's harness files such as `AGENTS.md`, `README.md`, and local skills."
---

# when-create-local-harness

## Scope

- This skill itself is installed as a global `.codex` skill.
- The files it edits are local to the current project.
- Its targets are one project's `AGENTS.md`, `README.md`, and local skills.

## Rules

- Check existing `AGENTS.md`, `README.md`, and local skills first.
- Edit existing guidance before creating a new guidance file.
- Keep `AGENTS.md` focused on behavior rules.
- Keep `README.md` focused on human-facing structure and usage.
- Keep docs short, final, and non-duplicative.
- Add a local skill only when the workflow is repeated enough to justify one.
