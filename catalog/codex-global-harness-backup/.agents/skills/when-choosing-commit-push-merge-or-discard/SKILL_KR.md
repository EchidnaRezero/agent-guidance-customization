---
name: when-choosing-commit-push-merge-or-discard
description: 로컬 Git 작업을 commit, push, PR 생성, merge, keep, discard 중 무엇으로 처리할지 고를 때 사용합니다.
---

# When Choosing Commit Push Merge Or Discard

## 목적

완료된 로컬 작업의 다음 Git action을 고릅니다.

## 사전 확인

read-only 확인을 실행합니다.

```powershell
$repoRoot = git rev-parse --show-toplevel
$gitDir = git rev-parse --git-dir
$gitCommon = git rev-parse --git-common-dir
$branch = git branch --show-current
$status = git status --short
git remote -v
git worktree list
```

`$branch`가 비어 있으면 detached HEAD 상태를 보고하고 merge/push를 가정하지 않습니다.

완료를 주장하기 전에 관련 test command를 실행합니다. 실패하면 멈추고 실패한 command와 요약을 보고합니다.

## Git 계정 선택

commit 또는 push 전에는 `references/git-account-selection.md`를 따릅니다.

## 선택지 제시

요청한 경로에 필요한 test와 identity 확인이 끝난 뒤, 필요한 선택지만 제시합니다.

```text
What should I do with this branch?

1. Commit locally
2. Push current branch
3. Create PR
4. Merge locally
5. Keep as-is
6. Discard work
```

사용자가 이미 특정 action을 요청했다면 필요한 확인 후 바로 그 action을 실행합니다.

## Action

- commit, push, PR, merge 명령 형태는 `references/git-finish-actions.md`를 읽습니다.
- discard, branch 삭제, worktree 제거는 `references/destructive-git-cleanup.md`를 읽습니다.
- GitHub SSH push 전에는 `when-pushing-to-github-with-ssh`를 사용합니다.

## 중단 조건

- global Git identity 변경
- repo-local 계정을 고르기 전 commit 또는 push
- `when-pushing-to-github-with-ssh` 없이 GitHub SSH push
- 제거할 worktree 안에서 `git worktree remove` 실행
- merge/test 성공 전 branch 삭제
