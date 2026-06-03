---
name: using-git-worktrees
description: 현재 작업 공간과 분리된 기능 작업을 시작하거나 구현 계획을 실행하기 전에 사용합니다. 안전 확인과 디렉터리 선택 절차를 거쳐 격리된 git worktree를 만듭니다.
---

# Git Worktree 사용하기

## 개요

Git worktree는 같은 저장소를 공유하면서도 서로 분리된 작업 공간을 만드는 기능입니다. 여러 브랜치를 동시에 작업할 때, 현재 작업 폴더를 계속 바꾸지 않아도 됩니다.

**핵심 원칙:** 디렉터리를 체계적으로 고르고, 안전 확인을 한 뒤에 만들어야 안정적으로 격리됩니다.

**시작할 때 알릴 말:** "I'm using the using-git-worktrees skill to set up an isolated workspace."

기본 셸 안내는 Windows 기준입니다. 현재 환경이 Windows가 아니면, 이 스킬의 셸 예시나 전역 경로를 해석하기 전에 해당 OS 환경 스킬을 먼저 불러옵니다.

## 디렉터리 선택 절차

아래 우선순서를 따릅니다.

### 1. 기존 디렉터리 확인

```powershell
Get-ChildItem -Directory -Force .worktrees -ErrorAction SilentlyContinue
Get-ChildItem -Directory -Force worktrees -ErrorAction SilentlyContinue
```

**발견되면:** 그 디렉터리를 사용합니다. 둘 다 있으면 `.worktrees`를 우선합니다.

### 2. 프로젝트 안내 확인

```powershell
Get-Content AGENTS.md -ErrorAction SilentlyContinue | Select-String -Pattern "worktree.*director"
```

**선호 위치가 적혀 있으면:** 사용자에게 다시 묻지 않고 그대로 사용합니다.

### 3. 사용자에게 묻기

기존 디렉터리도 없고 프로젝트 안내도 없으면 묻습니다.

```text
No worktree directory found. Where should I create worktrees?

1. .worktrees/ (project-local, hidden)
2. worktrees\<project-name>\ (shared location inside this bundled superpowers root on Windows)

Which would you prefer?
```

## 안전 확인

### 프로젝트 내부 디렉터리인 경우 (`.worktrees` 또는 `worktrees`)

**worktree를 만들기 전에 반드시 해당 디렉터리가 ignore 되는지 확인합니다.**

```powershell
# Check if directory is ignored (respects local, global, and system gitignore)
git check-ignore -q .worktrees 2>$null
$ignored = $LASTEXITCODE -eq 0
if (-not $ignored) {
  git check-ignore -q worktrees 2>$null
  $ignored = $LASTEXITCODE -eq 0
}
```

**ignore 되지 않으면:**

Jesse의 규칙인 "Fix broken things immediately"에 따라:

1. `.gitignore`에 알맞은 줄을 추가합니다.
2. 그 변경을 commit 합니다.
3. 그다음 worktree 생성을 계속합니다.

**중요한 이유:** worktree 내부 파일이 실수로 저장소에 commit 되는 일을 막기 위해서입니다.

### 공유 디렉터리인 경우 (`worktrees/`)

프로젝트 밖에 있으므로 `.gitignore` 확인이 필요 없습니다.

## 생성 절차

### 1. 프로젝트 이름 감지

```powershell
$project = Split-Path -Leaf (git rev-parse --show-toplevel)
```

### 2. Worktree 생성

```powershell
if ($LOCATION -in @('.worktrees', 'worktrees')) {
  $path = Join-Path $LOCATION $BRANCH_NAME
} else {
  $path = Join-Path "worktrees\\$project" $BRANCH_NAME
}

git worktree add "$path" -b "$BRANCH_NAME"
Set-Location "$path"
```

### 3. 프로젝트 설정 실행

프로젝트 파일을 보고 필요한 설정을 자동으로 실행합니다.

```powershell
if (Test-Path package.json) { npm install }
if (Test-Path Cargo.toml) { cargo build }
if (Test-Path requirements.txt) { pip install -r requirements.txt }
if (Test-Path pyproject.toml) { poetry install }
if (Test-Path go.mod) { go mod download }
```

### 4. 깨끗한 시작 상태 확인

테스트를 실행해 worktree가 깨끗한 상태에서 시작하는지 확인합니다.

```powershell
# Examples - use project-appropriate command
npm test
cargo test
pytest
go test ./...
```

**테스트가 실패하면:** 실패 내용을 보고하고, 계속 진행할지 조사할지 사용자에게 묻습니다.

**테스트가 통과하면:** 준비됐다고 보고합니다.

### 5. 위치 보고

```text
Worktree ready at <full-path>
Tests passing (<N> tests, 0 failures)
Ready to implement <feature-name>
```

## 빠른 참조

| 상황 | 행동 |
|------|------|
| `.worktrees/`가 있음 | 사용합니다. ignore 확인 필요 |
| `worktrees/`가 있음 | 사용합니다. ignore 확인 필요 |
| 둘 다 있음 | `.worktrees/`를 사용합니다 |
| 둘 다 없음 | `AGENTS.md` 확인 후 사용자에게 묻습니다 |
| 디렉터리가 ignore 되지 않음 | `.gitignore`에 추가하고 commit 합니다 |
| baseline 테스트 실패 | 실패를 보고하고 사용자에게 묻습니다 |
| `package.json`/`Cargo.toml` 없음 | dependency 설치를 건너뜁니다 |

## 흔한 실수

### ignore 확인 생략

- **문제:** worktree 내용이 추적되어 git 상태를 더럽힙니다.
- **해결:** 프로젝트 내부 worktree를 만들기 전에 항상 `git check-ignore`를 사용합니다.

### 위치를 추측함

- **문제:** 프로젝트 관례를 어기고 일관성이 깨집니다.
- **해결:** 기존 디렉터리 > `AGENTS.md` > 사용자에게 질문 순서를 따릅니다.

### 실패한 테스트를 무시하고 진행

- **문제:** 새 버그와 기존 문제를 구분할 수 없습니다.
- **해결:** 실패를 보고하고 명시적으로 진행 허락을 받습니다.

### 설정 명령을 하드코딩

- **문제:** 다른 도구를 쓰는 프로젝트에서 깨집니다.
- **해결:** `package.json` 등 프로젝트 파일로 자동 감지합니다.

## 금지 신호

**절대 하지 말 것:**

- 프로젝트 내부 worktree를 만들면서 ignore 확인을 생략하기
- baseline 테스트 확인을 건너뛰기
- 테스트가 실패했는데 묻지 않고 진행하기
- 위치가 애매한데 추측하기
- `AGENTS.md` 확인을 건너뛰기

**항상 할 것:**

- 기존 디렉터리 > `AGENTS.md` > 사용자에게 질문 순서를 따르기
- 프로젝트 내부 디렉터리가 ignore 되는지 확인하기
- 프로젝트 설정을 자동 감지하고 실행하기
- 깨끗한 테스트 baseline 확인하기

## 통합

**호출하는 스킬:**

- **brainstorming** (Phase 4) - 설계 승인 후 구현이 이어질 때 필수
- **subagent-driven-development** - 작업 실행 전 필수
- **executing-plans** - 작업 실행 전 필수
- 격리된 작업 공간이 필요한 모든 스킬

**함께 쓰는 스킬:**

- **finishing-a-development-branch** - 작업 완료 후 cleanup에 필수
