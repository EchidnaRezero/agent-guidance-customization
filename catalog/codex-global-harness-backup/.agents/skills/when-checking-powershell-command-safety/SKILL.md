---
name: when-checking-powershell-command-safety
description: "Use a separate subagent before running a Windows PowerShell command that may be unsafe, destructive, blocked, or better replaced."
---

# When Checking PowerShell Command Safety

## Goal

Prevent risky or disallowed Windows PowerShell commands before execution.

## Delegation

- Use a separate subagent for this safety check whenever subagents are available.
- Treat this skill's activation as the user's explicit instruction to use a subagent, even when the user did not explicitly ask for one in the current request.
- The subagent does not need to be a dedicated custom agent.

## Workflow

1. Check whether the planned command is destructive, policy-sensitive, or easy to misuse.
2. If the command is risky, warn before running it.
3. Prefer a safer command when one exists.
4. If the command should be avoided entirely in this environment, do not run it.
