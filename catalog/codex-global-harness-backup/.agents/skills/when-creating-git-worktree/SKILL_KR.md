---
name: when-creating-git-worktree
description: Windows에서 Git worktree를 만들거나 확인할 때 사용합니다. worktree 위치, ignore 안전성, setup, baseline test를 포함합니다.
---

# When Creating Git Worktree

## 목적

main checkout을 더럽히거나 worktree 폴더를 실수로 commit하지 않게 격리된 Git worktree를 만듭니다.

## 사전 확인

먼저 read-only 확인을 실행합니다.

```powershell
$repoRoot = git rev-parse --show-toplevel
$gitDir = git rev-parse --git-dir
$gitCommon = git rev-parse --git-common-dir
$branch = git branch --show-current
git rev-parse --show-superproject-working-tree 2>$null
```

- `$gitDir -ne $gitCommon`이고 superproject 확인 결과가 비어 있으면 기존 linked worktree를 보고하고 재사용합니다.
- `$branch`가 비어 있으면 branch 생성이나 마무리 전에 detached HEAD 상태를 보고하세요.

## 위치 선택

다음 순서를 사용합니다.

1. 기존 `.worktrees`
2. 기존 `worktrees`
3. `AGENTS.md`의 project guidance
4. 사용자에게 질문

```powershell
Get-ChildItem -Directory -Force .worktrees -ErrorAction SilentlyContinue
Get-ChildItem -Directory -Force worktrees -ErrorAction SilentlyContinue
Get-Content AGENTS.md -ErrorAction SilentlyContinue | Select-String -Pattern "worktree.*director"
```

질문이 필요하면:

```text
No worktree directory found. Where should I create worktrees?

1. .worktrees/ inside this repo
2. worktrees/<project-name>/ in the chosen shared location
```

## Ignore 안전성

repo-local `.worktrees` 또는 `worktrees`를 쓸 때는 만들기 전에 Git이 해당 디렉터리를 ignore하는지 확인합니다.

```powershell
git check-ignore -q .worktrees 2>$null
$ignored = $LASTEXITCODE -eq 0
if (-not $ignored) {
  git check-ignore -q worktrees 2>$null
  $ignored = $LASTEXITCODE -eq 0
}
```

ignore되지 않으면 선택한 디렉터리를 `.gitignore`에 추가하고, 그 housekeeping 변경을 commit하기 전에 사용자에게 확인합니다.

## 생성

```powershell
$project = Split-Path -Leaf (git rev-parse --show-toplevel)
$path = Join-Path $LOCATION $BRANCH_NAME
git worktree add "$path" -b "$BRANCH_NAME"
Set-Location "$path"
```

host 또는 app이 이미 격리를 관리해서 `git worktree add`가 실패하면, 수동 상태를 억지로 만들지 말고 충돌을 설명하고 멈춥니다.

## Setup과 Baseline

흔한 setup 명령은 자동 감지합니다.

```powershell
if (Test-Path package.json) { npm install }
if (Test-Path Cargo.toml) { cargo build }
if (Test-Path requirements.txt) { pip install -r requirements.txt }
if (Test-Path pyproject.toml) { poetry install }
if (Test-Path go.mod) { go mod download }
```

구현 전에 project test command를 실행합니다. 실패하면 실패 내용을 보고하고, 원인 분석을 할지 known-bad baseline으로 계속할지 사용자에게 묻습니다.

## 보고

```text
Worktree ready at <full-path>
Branch: <branch>
Baseline: <test command/result>
```

## 중단 조건

- nested worktree 만들기
- repo-local worktree 디렉터리에 대해 `.gitignore`/`git check-ignore` 확인 생략하기
- baseline test 실패를 조용히 넘기기
- Codex/App-managed worktree와 싸우고 환경 상태 보고를 생략하기
