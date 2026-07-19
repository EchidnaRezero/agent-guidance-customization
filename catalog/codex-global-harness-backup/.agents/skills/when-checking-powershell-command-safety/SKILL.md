---
name: when-checking-powershell-command-safety
description: "Use before running a Windows PowerShell command that can affect the PC or system beyond the current project folder, workspace, sandbox, container, or a similarly isolated environment. Do not use when all effects are confined to the current project, workspace, container, or equivalent isolation boundary."
---

# When Checking PowerShell Command Safety

## Scope

- Trigger only when a planned command can change system-level resources outside the active isolation boundary.
- Do not trigger for effects confined to the current project folder, workspace, sandbox, container, or a similarly isolated environment.

## Approval

1. Identify the exact system scope the command can affect, such as external paths, services, installed software, accounts, system configuration, security controls, or networking.
2. Explain that scope and the concrete risks to the user.
3. Ask for explicit approval to run the command with that scope.
4. Do not execute the command unless the user grants explicit approval.
