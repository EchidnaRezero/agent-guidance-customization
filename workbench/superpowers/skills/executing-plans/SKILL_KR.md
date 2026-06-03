---
name: executing-plans
description: review checkpoint가 있는 별도 세션에서 작성된 구현 계획을 실행할 때 사용합니다.
---

# Executing Plans

## 개요

계획을 불러오고, 비판적으로 검토하고, 모든 작업을 실행한 뒤 완료를 보고합니다.

**시작할 때 알림:** "I'm using the executing-plans skill to implement this plan."

**참고:** 사람 파트너에게 Superpowers는 subagent 접근 권한이 있을 때 훨씬 더 잘 작동한다고 알려주세요. 현재 Codex 환경에서 subagent를 사용할 수 있으면 이 스킬 대신 `superpowers:subagent-driven-development`를 사용하세요.

## 프로세스

### Step 1: 계획 불러오기와 검토

1. 계획 파일을 읽습니다.
2. 비판적으로 검토해 질문이나 우려가 있는지 확인합니다.
3. 우려가 있으면 시작하기 전에 사람 파트너에게 제기합니다.
4. 우려가 없으면 TodoWrite를 만들고 진행합니다.

### Step 2: 작업 실행

범위 경계:

- 계획에 적힌 것만 구현하세요.
- 실행 중 기능을 확장하지 마세요.
- 방향이 바뀌면 계속하기 전에 멈추고 계획이나 `brainstorming`으로 돌아가세요.

각 작업에서:

1. `in_progress`로 표시합니다.
2. 각 단계를 정확히 따릅니다. 계획은 작은 단위의 단계로 되어 있습니다.
3. 지정된 검증을 실행합니다.
4. `completed`로 표시합니다.

### Step 3: 개발 완료

모든 작업이 완료되고 검증되면:

- 알림: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** `superpowers:finishing-a-development-branch`를 사용하세요.
- 해당 스킬에 따라 테스트를 검증하고, 선택지를 제시하고, 선택된 작업을 실행하세요.

## 멈추고 도움을 요청해야 할 때

**다음 상황에서는 즉시 실행을 멈추세요.**

- blocker를 만남: dependency 누락, test 실패, 지시 불명확
- 계획에 시작을 막는 중요한 공백이 있음
- 지시를 이해하지 못함
- 검증이 반복해서 실패함

**추측하지 말고 명확히 물어보세요.**

## 이전 단계로 돌아가야 할 때

**다음 상황에서는 Review (Step 1)로 돌아가세요.**

- 파트너가 피드백을 바탕으로 계획을 업데이트함
- 근본 접근 방식을 다시 생각해야 함
- 대상이나 경계가 바뀜
- 새 아이디어가 계획 범위를 넘어서게 됨

**blocker를 억지로 뚫고 가지 마세요.** 멈추고 물어보세요.

## 기억할 것

- 먼저 계획을 비판적으로 검토하세요.
- 계획 단계를 정확히 따르세요.
- 검증을 건너뛰지 마세요.
- 계획이 말하는 스킬을 참조하세요.
- 막히면 멈추고, 추측하지 마세요.
- 명시적인 사용자 동의 없이 `main`/`master` 브랜치에서 구현을 시작하지 마세요.

## 통합

**필수 워크플로 스킬:**

- **superpowers:using-git-worktrees** - REQUIRED: 시작 전에 격리된 workspace를 설정하세요. Windows가 아니면 먼저 OS 환경 스킬을 불러오세요.
- **superpowers:writing-plans** - 이 스킬이 실행할 계획을 만듭니다.
- **superpowers:finishing-a-development-branch** - 모든 작업 후 개발을 완료합니다.
