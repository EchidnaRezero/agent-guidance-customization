---
name: when-checking-powershell-command-safety
description: "Use before running a Windows PowerShell command that may be unsafe, destructive, blocked, or better replaced."
---

# When Checking PowerShell Command Safety

## Goal

Prevent risky or disallowed Windows PowerShell commands before execution.

## Workflow

1. Check whether the planned command is destructive, policy-sensitive, or easy to misuse.
2. If the command is risky, warn before running it.
3. Prefer a safer command when one exists.
4. If the command should be avoided entirely in this environment, do not run it.
