---
name: safe-parallel-tool-use
description: "Decide whether planned commands are safe to run in parallel. Use when mixing Git, SSH, file edits, config changes, or status checks to avoid stale reads and race conditions."
---

# Safe Parallel Tool Use

Prevent misleading results caused by running dependent or stateful operations at the same time.

## Core Rule

1. List the planned operations.
2. Mark each one as read-only, state-changing, auth/network-sensitive, or interactive.
3. Run operations in parallel only if every operation is read-only, independent, and not reading state another operation may change.
4. Otherwise run sequentially, then verify in a fresh step.

## Never Parallelize

- a state-changing command with a status check for the same system
- auth setup with commands that depend on that auth
- file edits with checks that read the edited files
- commands that may block on passphrase, login, remote completion, or user input
- any uncertain batch after a user interrupt; recheck sequentially instead

## Common Cases

### Git

- Run `git add`, `git commit`, `git push`, `git pull`, `git fetch`, `git checkout`, and `git config` writes sequentially.
- Check `git status`, `git diff`, `git rev-parse`, or `git ls-remote` only after the write finishes.

### SSH and Auth

- Run `ssh-add`, `gh auth login`, `gh auth status`, and `ssh -T` sequentially when later steps depend on them.

### Safe Parallel Work

- reading multiple unrelated files
- listing different directories
- comparing diffs from independent files
- running a compile check on one file while reading unrelated documentation

## Reference

- Read `references/risky-combinations.md` for concrete bad pairings and safer replacements.
