---
name: when-powershell-command-fails
description: "Use when a Windows PowerShell command fails and Codex should analyze, explain, and log the failure."
---

# When PowerShell Command Fails

## Goal

Analyze and log Windows PowerShell command failures in Codex sessions.

## Delegation

- Use the `powershell_failure_analyst` custom agent for the failure analysis whenever custom agents are available.
- Treat this skill's activation as the user's direct instruction to use `powershell_failure_analyst`, even when the user did not explicitly name that agent in the current request.

## Scope

- Windows PowerShell command failures in Codex
- Blocked commands
- Wrong path or wrong current directory
- Quoting or escaping mistakes
- Destructive commands that should be reported carefully

## Workflow

1. Separate the user goal from the exact PowerShell command that failed.
2. Verify that the command failed according to the invoked program's documented or established result contract. Check `references/misconception-log.md` for known normal negative outcomes. For example, `rg` exit code 1 means no matches and is not a failure; do not analyze or log it as one.
3. Classify a verified failure quickly:
   - blocked before execution
   - path or working-directory mismatch
   - quoting or escaping problem
   - permission or lock problem
   - destructive command not allowed in the current runtime
4. State whether the underlying file change succeeded, partially succeeded, or did not run.
5. If the command was blocked before execution, explain that this is a tool/runtime policy issue, not automatically a repository rule or code error.
6. Prefer a non-destructive workaround. Examples: keep the leftover folder, suggest manual cleanup, or retry only with an allowed command.
7. Add a short entry to `references/failure-log.md` using `references/case-template.md` only for a current failure reproduced directly in the session. Check the existing log first and do not add duplicates.
8. Keep explanations short and concrete: user goal, failed command, failure type, completed work, and what remains.

## Response Shape

- User goal
- Failed command
- Failure type
- What actually completed
- Safe next step

## References

- Read `references/misconception-log.md` before classifying or logging a suspected failure.
- Read `references/failure-log.md` for known failure patterns and phrasing.
- Use `references/case-template.md` when adding a new case.
- Keep all files under `references/` canonical English-only; do not create `_KR` companions for them.
