---
name: finishing-a-development-branch
description: 구현이 끝나고 모든 테스트가 통과한 뒤, 작업을 어떻게 합칠지 결정해야 할 때 사용합니다. merge, PR, cleanup 선택지를 구조적으로 안내합니다.
---

# 개발 브랜치 마무리하기

## 개요

개발 작업이 끝났을 때는 먼저 테스트가 통과하는지 확인하고, 사용자에게 명확한 선택지를 제시한 뒤, 선택한 흐름을 실행합니다.

**핵심 원칙:** 테스트 확인 → 선택지 제시 → 선택 실행 → 정리.

commit 또는 push가 생기는 완료 경로를 선택했을 때만 Git 계정이나 repo-local identity를 묻습니다.

**시작할 때 알릴 말:** "I'm using the finishing-a-development-branch skill to complete this work."

기본 셸 안내는 Windows 기준입니다. 현재 환경이 Windows가 아니면, 이 스킬의 셸 예시나 cleanup 예시를 해석하기 전에 해당 OS 환경 스킬을 먼저 불러옵니다.

## 절차

### Step 1: 테스트 확인

**선택지를 제시하기 전에 테스트 통과를 확인합니다.**

```powershell
# Run project's test suite
npm test / cargo test / pytest / go test ./...
```

**테스트가 실패하면:**

```text
Tests failing (<N> failures). Must fix before completing:

[Show failures]

Cannot proceed with merge/PR until tests pass.
```

여기서 멈춥니다. Step 2로 가지 않습니다.

**테스트가 통과하면:** Step 2로 갑니다.

### Step 2: 기준 브랜치 확인

```powershell
# Try common base branches
git merge-base HEAD main 2>$null
if ($LASTEXITCODE -ne 0) {
  git merge-base HEAD master 2>$null
}
```

또는 이렇게 묻습니다: "This branch split from main - is that correct?"

### Step 3: 선택지 제시

정확히 아래 4개 선택지를 제시합니다.

```text
Implementation complete. What would you like to do?

1. Merge back to <base-branch> locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

**추가 설명을 붙이지 말고** 짧게 유지합니다.

### Step 4: 선택 실행

선택한 경로가 commit 또는 push를 만들면:

- Git 계정을 한 번 확인합니다.
- 첫 commit 또는 push 전에 repo-local `user.name`과 `user.email`을 설정합니다.

#### Option 1: 로컬에서 merge

```powershell
# Switch to base branch
git checkout <base-branch>

# Pull latest
git pull

# Merge feature branch
git merge <feature-branch>

# Verify tests on merged result
<test command>

# If tests pass
git branch -d <feature-branch>
```

그다음 worktree cleanup으로 갑니다. Step 5를 따릅니다.

#### Option 2: Push 후 PR 생성

```powershell
# Push branch
git push -u origin <feature-branch>

# Create PR
$prBody = @"
## Summary
<2-3 bullets of what changed>

## Test Plan
- [ ] <verification steps>
"@

gh pr create --title "<title>" --body $prBody
```

그다음 worktree cleanup으로 갑니다. Step 5를 따릅니다.

#### Option 3: 그대로 유지

보고합니다: "Keeping branch <name>. Worktree preserved at <path>."

**worktree를 정리하지 않습니다.**

#### Option 4: 폐기

**먼저 확인합니다.**

```text
This will permanently delete:
- Branch <name>
- All commits: <commit-list>
- Worktree at <path>

Type 'discard' to confirm.
```

정확히 `discard`라고 입력할 때까지 기다립니다.

확인되면:

```powershell
git checkout <base-branch>
git branch -D <feature-branch>
```

그다음 worktree cleanup으로 갑니다. Step 5를 따릅니다.

### Step 5: Worktree 정리

**Options 1, 2, 4에서 사용합니다.**

worktree 안에 있는지 확인합니다.

```powershell
$branch = git branch --show-current
git worktree list | Select-String $branch
```

worktree라면:

```powershell
git worktree remove <worktree-path>
```

**Option 3:** worktree를 유지합니다.

## 빠른 참조

| 선택지 | Merge | Push | Worktree 유지 | Branch 정리 |
|--------|-------|------|---------------|-------------|
| 1. Merge locally | ✓ | - | - | ✓ |
| 2. Create PR | - | ✓ | ✓ | - |
| 3. Keep as-is | - | - | ✓ | - |
| 4. Discard | - | - | - | ✓ (force) |

## 흔한 실수

**테스트 확인 생략**

- **문제:** 깨진 코드를 merge하거나 실패하는 PR을 만듭니다.
- **해결:** 선택지를 제시하기 전에 항상 테스트를 확인합니다.

**열린 질문 사용**

- **문제:** "다음에 뭘 할까요?"처럼 애매해집니다.
- **해결:** 정확히 4개의 구조화된 선택지를 제시합니다.

**자동 worktree cleanup**

- **문제:** 아직 필요할 수 있는 worktree를 지웁니다. 특히 Option 2, 3.
- **해결:** Option 1과 4에서만 cleanup 합니다.

**폐기 확인 없음**

- **문제:** 작업을 실수로 삭제할 수 있습니다.
- **해결:** `discard` 입력 확인을 반드시 받습니다.

## 금지 신호

**절대 하지 말 것:**

- 테스트가 실패했는데 진행하기
- merge 결과에서 테스트를 확인하지 않고 merge 완료 처리하기
- 확인 없이 작업 삭제하기
- 명시 요청 없이 force-push 하기

**항상 할 것:**

- 선택지 제시 전에 테스트 확인하기
- 정확히 4개 선택지를 제시하기
- Option 4에서는 입력 확인 받기
- Option 1과 4에서만 worktree 정리하기

## 통합

**호출하는 스킬:**

- **subagent-driven-development** (Step 7) - 모든 작업 완료 후
- **executing-plans** (Step 5) - 모든 batch 완료 후

**함께 쓰는 스킬:**

- **using-git-worktrees** - 그 스킬이 만든 worktree를 정리합니다
