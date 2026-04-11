---
name: powershell-command-guard
description: "Check Windows PowerShell commands before execution. Use when Codex should warn about commands that may be unsafe or disallowed."
---

# PowerShell Command Guard

## Goal

Prevent risky or disallowed Windows PowerShell commands before execution.

## Workflow

1. Check whether the planned command is destructive, policy-sensitive, or easy to misuse.
2. If the command is risky, warn before running it.
3. Prefer a safer command when one exists.
4. If the command should be avoided entirely in this environment, do not run it.
