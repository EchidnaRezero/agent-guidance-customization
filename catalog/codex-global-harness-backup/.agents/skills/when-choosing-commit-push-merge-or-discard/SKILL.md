---
name: when-choosing-commit-push-merge-or-discard
description: Use when choosing whether to commit, push, create a PR, merge, keep, or discard local Git work.
---

# When Choosing Commit Push Merge Or Discard

## Purpose

Choose the next Git action for completed local work.

## Preflight

Run read-only checks:

```powershell
$repoRoot = git rev-parse --show-toplevel
$gitDir = git rev-parse --git-dir
$gitCommon = git rev-parse --git-common-dir
$branch = git branch --show-current
$status = git status --short
git remote -v
git worktree list
```

If `$branch` is empty, report detached HEAD and avoid merge/push assumptions.

Run the relevant test command before claiming completion. If tests fail, stop and report the failing command and summary.

## Git Account Selection

Before commit or push, follow `references/git-account-selection.md`.

## Present Choices

After tests and identity checks relevant to the requested path, offer only useful choices:

```text
What should I do with this branch?

1. Commit locally
2. Push current branch
3. Create PR
4. Merge locally
5. Keep as-is
6. Discard work
```

If the user already requested one action, execute that action directly after the required checks.

## Actions

- For commit, push, PR, and merge command shapes, read `references/git-finish-actions.md`.
- For discard, branch deletion, or worktree removal, read `references/destructive-git-cleanup.md`.
- Before GitHub SSH push, use `when-pushing-to-github-with-ssh`.

## Critical Stops

- Global Git identity changes
- Commit or push before selecting the repo-local account
- GitHub SSH push without `when-pushing-to-github-with-ssh`
- Worktree removal from inside the worktree being removed
- Branch deletion before merge/test success
