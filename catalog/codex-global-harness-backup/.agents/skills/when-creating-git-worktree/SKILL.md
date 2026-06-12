---
name: when-creating-git-worktree
description: Use when creating or checking a Windows Git worktree, including location, ignore safety, setup, and baseline tests.
---

# When Creating Git Worktree

## Purpose

Create an isolated Git worktree without polluting the main checkout or committing worktree folders by mistake.

## Preflight

Run read-only checks first:

```powershell
$repoRoot = git rev-parse --show-toplevel
$gitDir = git rev-parse --git-dir
$gitCommon = git rev-parse --git-common-dir
$branch = git branch --show-current
git rev-parse --show-superproject-working-tree 2>$null
```

- If `$gitDir -ne $gitCommon` and the superproject check is empty, report the existing linked worktree and reuse it.
- If `$branch` is empty, report detached HEAD before creating or finishing branch work.

## Choose Location

Use this order:

1. Existing `.worktrees`
2. Existing `worktrees`
3. Project guidance in `AGENTS.md`
4. Ask the user

```powershell
Get-ChildItem -Directory -Force .worktrees -ErrorAction SilentlyContinue
Get-ChildItem -Directory -Force worktrees -ErrorAction SilentlyContinue
Get-Content AGENTS.md -ErrorAction SilentlyContinue | Select-String -Pattern "worktree.*director"
```

If asking:

```text
No worktree directory found. Where should I create worktrees?

1. .worktrees/ inside this repo
2. worktrees/<project-name>/ in the chosen shared location
```

## Ignore Safety

For repo-local `.worktrees` or `worktrees`, verify Git ignores the directory before creation:

```powershell
git check-ignore -q .worktrees 2>$null
$ignored = $LASTEXITCODE -eq 0
if (-not $ignored) {
  git check-ignore -q worktrees 2>$null
  $ignored = $LASTEXITCODE -eq 0
}
```

If not ignored, add the chosen directory to `.gitignore` and ask before committing that housekeeping change.

## Create

```powershell
$project = Split-Path -Leaf (git rev-parse --show-toplevel)
$path = Join-Path $LOCATION $BRANCH_NAME
git worktree add "$path" -b "$BRANCH_NAME"
Set-Location "$path"
```

If `git worktree add` fails because the host or app already manages isolation, stop and explain the conflict instead of forcing manual state.

## Setup And Baseline

Auto-detect common setup commands:

```powershell
if (Test-Path package.json) { npm install }
if (Test-Path Cargo.toml) { cargo build }
if (Test-Path requirements.txt) { pip install -r requirements.txt }
if (Test-Path pyproject.toml) { poetry install }
if (Test-Path go.mod) { go mod download }
```

Run the project test command before implementation. If tests fail, report the failures and ask whether to investigate or continue with a known-bad baseline.

## Report

```text
Worktree ready at <full-path>
Branch: <branch>
Baseline: <test command/result>
```

## Critical Stops

- Creating nested worktrees
- Skipping `.gitignore`/`git check-ignore` for repo-local worktree directories
- Proceeding silently from failing baseline tests
- Fighting a Codex/App-managed worktree instead of reporting the environment state
