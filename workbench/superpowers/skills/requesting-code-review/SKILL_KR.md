---
name: requesting-code-review
description: 작업 완료, 주요 기능 구현 후, 또는 merge 전에 요구사항 충족 여부를 확인하기 위해 사용합니다.
---

# 코드 리뷰 요청하기

`superpowers:code-reviewer` subagent를 보내 문제를 일찍 잡습니다. 리뷰어에게는 현재 세션 기록이 아니라 정확히 구성한 작업 맥락만 전달합니다. 이렇게 하면 리뷰어는 사고 과정이 아니라 결과물에 집중하고, 현재 세션의 context도 보존됩니다.

**핵심 원칙:** 일찍, 자주 리뷰하고, scope와 완료 기준을 확인합니다.

## 언제 리뷰를 요청할까

**필수:**

- subagent-driven development의 각 작업 뒤
- 주요 기능을 완료한 뒤
- main으로 merge하기 전

**선택이지만 유용한 경우:**

- 막혔을 때, 새로운 관점을 얻기 위해
- refactoring 전 baseline 확인을 위해
- 복잡한 bugfix 뒤

## 요청 방법

**1. git SHA를 얻습니다.**

```powershell
$BASE_SHA = git rev-parse HEAD~1  # or origin/main
$HEAD_SHA = git rev-parse HEAD
```

**2. code-reviewer subagent를 보냅니다.**

Task tool에서 `superpowers:code-reviewer` type을 사용하고, `code-reviewer.md` template을 채웁니다.

**Placeholders:**

- `{WHAT_WAS_IMPLEMENTED}` - 방금 만든 것
- `{PLAN_OR_REQUIREMENTS}` - 무엇을 해야 하는지
- `{SCOPE_AND_COMPLETION_CRITERIA}` - 합의한 경계와 완료 기준
- `{BASE_SHA}` - 시작 commit
- `{HEAD_SHA}` - 끝 commit
- `{DESCRIPTION}` - 짧은 요약

**3. feedback에 대응합니다.**

- Critical issue는 즉시 고칩니다.
- Important issue는 진행 전에 고칩니다.
- scope creep이나 completion criteria 미달은 진행 전에 고칩니다.
- Minor issue는 나중을 위해 기록합니다.
- 리뷰어가 틀렸다면 근거를 들어 반박합니다.

## 핵심 예시

```text
[Just completed Task 2: Add verification function]

You: Let me request code review before proceeding.

$BASE_SHA = (git log --oneline | Select-String "Task 1" | Select-Object -First 1).ToString().Split(' ')[0]
$HEAD_SHA = git rev-parse HEAD

[Dispatch superpowers:code-reviewer subagent]
  WHAT_WAS_IMPLEMENTED: Verification and repair functions for conversation index
  PLAN_OR_REQUIREMENTS: Task 2 from docs/superpowers/plans/deployment-plan.md
  BASE_SHA: a7981ec
  HEAD_SHA: 3df7661
  DESCRIPTION: Added verifyIndex() and repairIndex() with 4 issue types

[Subagent returns]:
  Issues:
    Important: Missing progress indicators
    Minor: Magic number (100) for reporting interval
  Assessment: Ready to proceed

You: [Fix progress indicators]
[Continue to Task 3]
```

## Workflow와 통합

**Subagent-Driven Development:**

- 각 작업 뒤에 리뷰합니다.
- 문제가 쌓이기 전에 잡습니다.
- 다음 작업으로 넘어가기 전에 고칩니다.

**Executing Plans:**

- 각 batch, 보통 3개 작업 뒤에 리뷰합니다.
- feedback을 적용한 뒤 계속합니다.

**Ad-Hoc Development:**

- merge 전에 리뷰합니다.
- 막혔을 때 리뷰합니다.

## 금지 신호

**절대 하지 말 것:**

- "간단하다"는 이유로 리뷰 생략하기
- Critical issue 무시하기
- Important issue를 고치지 않고 진행하기
- 유효한 기술 feedback에 억지로 반박하기

**리뷰어가 틀렸다면:**

- 기술적 근거로 반박합니다.
- 동작을 증명하는 code/test를 보여줍니다.
- clarification을 요청합니다.

Template 위치: `requesting-code-review/code-reviewer.md`
