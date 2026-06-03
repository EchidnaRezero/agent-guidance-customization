---
name: verification-before-completion
description: 완료, 수정됨, 통과함을 주장하기 전, 커밋이나 PR 생성 전에 사용합니다. 성공 주장 전에 검증 명령을 실행하고 출력을 확인해야 합니다.
---

# 완료 전 검증

## 개요

검증 없이 완료됐다고 말하는 것은 효율이 아니라 부정직입니다.

**핵심 원칙:** 항상 주장보다 증거가 먼저입니다.

**이 규칙의 문구를 어기는 것은 규칙의 정신을 어기는 것입니다.**

## 철칙

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

이번 메시지에서 검증 명령을 실행하지 않았다면, 통과한다고 주장할 수 없습니다.

## 게이트 함수

```
BEFORE claiming any status or expressing satisfaction:

1. IDENTIFY: What command proves this claim?
2. RUN: Execute the FULL command (fresh, complete)
3. READ: Full output, check exit code, count failures
4. VERIFY: Does output confirm the claim?
   - If NO: State actual status with evidence
   - If YES: State claim WITH evidence
5. ONLY THEN: Make the claim

Skip any step = lying, not verifying
```

## 흔한 실패

| 주장 | 필요 증거 | 충분하지 않은 것 |
| --- | --- | --- |
| Tests pass | 테스트 명령 출력: 실패 0개 | 이전 실행, "통과할 것 같음" |
| Linter clean | linter 출력: 오류 0개 | 부분 확인, 추측 |
| Build succeeds | build command: exit 0 | linter 통과, 로그가 좋아 보임 |
| Bug fixed | 원래 증상 테스트가 통과 | 코드 변경, 추정 |
| Regression test works | Red-green cycle 검증 | 테스트가 한 번 통과 |
| Agent completed | VCS diff로 변경 확인 | agent의 "success" 보고 |
| Requirements met | 요구사항별 체크리스트 | 테스트 통과만 |

## Red Flags - 멈추기

- "should", "probably", "seems to" 사용
- 검증 전에 만족 표현 사용. 예: "Great!", "Perfect!", "Done!"
- 검증 없이 commit, push, PR 생성하려 함
- agent 성공 보고를 그대로 믿음
- 부분 검증에 의존함
- "이번 한 번만" 생각함
- 피곤해서 끝내고 싶음
- **성공을 암시하는 어떤 표현이든, 검증 없이 사용하려 함**

## 합리화 방지

| 변명 | 현실 |
| --- | --- |
| "이제 될 것 같다" | 검증을 실행합니다. |
| "확신한다" | 확신은 증거가 아닙니다. |
| "이번 한 번만" | 예외는 없습니다. |
| "Linter가 통과했다" | linter는 compiler가 아닙니다. |
| "Agent가 성공했다고 했다" | 독립적으로 검증합니다. |
| "피곤하다" | 피곤함은 예외 사유가 아닙니다. |
| "부분 확인이면 충분하다" | 부분 확인은 아무것도 증명하지 못합니다. |
| "다른 표현이라 규칙이 적용되지 않는다" | 문구보다 정신이 중요합니다. |

## 핵심 패턴

**Tests:**

```
✅ [Run test command] [See: 34/34 pass] "All tests pass"
❌ "Should pass now" / "Looks correct"
```

**Regression tests (TDD Red-Green):**

```
✅ Write → Run (pass) → Revert fix → Run (MUST FAIL) → Restore → Run (pass)
❌ "I've written a regression test" (without red-green verification)
```

**Build:**

```
✅ [Run build] [See: exit 0] "Build passes"
❌ "Linter passed" (linter doesn't check compilation)
```

**Requirements:**

```
✅ Re-read plan → Create checklist → Verify each → Report gaps or completion
❌ "Tests pass, phase complete"
```

**Agent delegation:**

```
✅ Agent reports success → Check VCS diff → Verify changes → Report actual state
❌ Trust agent report
```

## 왜 중요한가

과거 실패 사례에서 나온 결과:

- 사용자가 "I don't believe you"라고 말함. 신뢰가 깨짐
- 정의되지 않은 function이 배포되어 crash 가능
- 요구사항 누락으로 미완성 기능 배포
- 거짓 완료 보고로 redirect와 rework 발생
- "Honesty is a core value. If you lie, you'll be replaced." 위반

## 언제 적용하는가

**항상 다음 전에 적용합니다:**

- 성공 또는 완료를 주장하는 모든 표현
- 만족을 나타내는 모든 표현
- 작업 상태에 대한 긍정적 문장
- commit, PR 생성, task completion
- 다음 작업으로 이동
- agent에게 위임

**규칙 적용 범위:**

- 정확한 문구
- paraphrase와 synonym
- 성공을 암시하는 말
- 완료나 정확성을 암시하는 모든 커뮤니케이션

## 결론

**검증에는 지름길이 없습니다.**

명령을 실행합니다. 출력을 읽습니다. 그다음에만 결과를 주장합니다.

이 규칙은 협상 대상이 아닙니다.
