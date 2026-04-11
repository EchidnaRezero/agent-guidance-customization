---
name: powershell-command-failure-helper
description: "Log failed Windows PowerShell commands. Use when a PowerShell command fails and Codex should analyze the cause and record it."
---

# PowerShell Command Failure Helper

## Goal

Analyze and log Windows PowerShell command failures in Codex sessions.

## Scope

- Windows PowerShell command failures in Codex
- Blocked commands
- Wrong path or wrong current directory
- Quoting or escaping mistakes
- Destructive commands that should be reported carefully

## Workflow

1. Separate the user goal from the exact PowerShell command that failed.
2. Classify the failure quickly:
   - blocked before execution
   - path or working-directory mismatch
   - quoting or escaping problem
   - permission or lock problem
   - destructive command not allowed in the current runtime
3. State whether the underlying file change succeeded, partially succeeded, or did not run.
4. If the command was blocked before execution, explain that this is a tool/runtime policy issue, not automatically a repository rule or code error.
5. Prefer a non-destructive workaround. Examples: keep the leftover folder, suggest manual cleanup, or retry only with an allowed command.
6. When a failure happens, analyze the likely cause and add a short entry to `references/failure-log.md` using `references/case-template.md`.
7. Keep explanations short and concrete: user goal, failed command, failure type, completed work, and what remains.

## Response Shape

- User goal
- Failed command
- Failure type
- What actually completed
- Safe next step

## References

- Read `references/failure-log.md` for known failure patterns and phrasing.
- Use `references/case-template.md` when adding a new case.
