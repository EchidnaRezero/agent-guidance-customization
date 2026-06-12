# Destructive Git Cleanup

Use this only when the chosen action discards work or removes a branch/worktree.

Show what will be deleted and require exact confirmation:

```text
Type discard to delete branch <branch> and worktree <path>.
```

Then:

```powershell
git checkout <base-branch>
git branch -D <branch>
git worktree remove <path>
git worktree prune
```

Rules:

- Do not delete a branch before merge and post-merge tests succeed unless the user explicitly chose discard.
- Do not run `git worktree remove` from inside the worktree being removed.
- Only remove worktrees that are clearly project-local or created by this workflow.
- If the host/app owns the worktree, leave it and report that cleanup should be done by the host.
