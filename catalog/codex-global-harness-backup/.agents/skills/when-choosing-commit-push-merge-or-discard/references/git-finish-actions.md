# Git Finish Actions

Use these command shapes after preflight, tests, and any required Git account checks.

## Commit Locally

```powershell
git status --short
git add <paths>
git commit -m "<message>"
```

Use the selected repo-local identity before committing.

## Push Current Branch

```powershell
git push -u origin <branch>
```

Use `when-pushing-to-github-with-ssh` before pushing to GitHub over SSH.

## Create PR

```powershell
$prBody = @"
## Summary
- <change>

## Test Plan
- <command/result>
"@
gh pr create --title "<title>" --body $prBody
```

Keep the worktree after opening a PR.

## Merge Locally

```powershell
git checkout <base-branch>
git pull
git merge <branch>
<test command>
```

Delete the branch or remove a worktree only after merge and post-merge tests succeed.
