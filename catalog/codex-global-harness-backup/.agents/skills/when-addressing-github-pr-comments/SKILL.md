---
name: when-addressing-github-pr-comments
description: Use when addressing review or issue comments on the current branch's open GitHub PR with gh CLI.
metadata:
  short-description: Address comments in a GitHub PR review
---

# When Addressing GitHub PR Comments

Guide to find the open PR for the current branch and address its comments with gh CLI.

Prereq: ensure `gh` is authenticated (for example, run `gh auth login` once), then run `gh auth status` so `gh` commands succeed.

## 1) Inspect comments needing attention
- Run scripts/fetch_comments.py which will print out all the comments and review threads on the PR

## 2) Ask the user for clarification
- Number all the review threads and comments and provide a short summary of what would be required to apply a fix for it
- Ask the user which numbered comments should be addressed

## 3) If user chooses comments
- Apply fixes for the selected comments

Notes:
- If gh hits auth/rate issues mid-run, prompt the user to re-authenticate with `gh auth login`, then retry.
