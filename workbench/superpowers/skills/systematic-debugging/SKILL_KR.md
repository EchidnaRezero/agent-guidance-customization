---
name: systematic-debugging
description: 버그, 테스트 실패, 예상 밖 동작을 만났을 때 수정안을 제안하기 전에 사용합니다.
---

# 체계적 디버깅

## 개요

무작위 수정은 시간을 낭비하고 새 버그를 만듭니다. 빠른 임시 패치는 근본 문제를 가립니다.

**핵심 원칙:** 수정하기 전에 항상 root cause를 찾습니다. 증상만 고치는 것은 실패입니다.

**절차의 문구를 어기는 것은 디버깅의 정신을 어기는 것입니다.**

## 철칙

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

Phase 1을 완료하지 않았다면 수정안을 제안할 수 없습니다.

## 언제 사용하는가

모든 기술 문제에 사용합니다.

- 테스트 실패
- production bug
- 예상 밖 동작
- 성능 문제
- build failure
- integration issue

**특히 사용해야 하는 경우:**
- 시간 압박이 있을 때
- "빠른 수정 하나"가 뻔해 보일 때
- 이미 여러 수정을 시도했을 때
- 이전 수정이 작동하지 않았을 때
- 문제를 완전히 이해하지 못했을 때

**건너뛰지 말아야 하는 경우:**
- 문제가 단순해 보일 때. 단순한 버그도 root cause가 있습니다.
- 급할 때. 서두르면 재작업이 늘어납니다.
- 즉시 고치라는 압박이 있을 때. 체계적인 방식이 시행착오보다 빠릅니다.

## 네 단계

각 단계를 완료해야 다음 단계로 넘어갑니다.

### Phase 1: Root Cause Investigation

**어떤 수정도 시도하기 전에 수행합니다.**

1. **오류 메시지를 꼼꼼히 읽기**
   - 오류나 경고를 건너뛰지 않습니다.
   - stack trace를 끝까지 읽습니다.
   - line number, file path, error code를 기록합니다.

2. **일관되게 재현하기**
   - 안정적으로 발생시키는 방법이 있습니까?
   - 정확한 단계는 무엇입니까?
   - 매번 발생합니까?
   - 재현되지 않으면 추측하지 말고 데이터를 더 모읍니다.

3. **최근 변경 확인**
   - 무엇이 바뀌었습니까?
   - git diff, 최근 커밋을 확인합니다.
   - 새 dependency, config change, 환경 차이를 봅니다.

4. **여러 컴포넌트 시스템에서 증거 수집**

   시스템이 여러 계층으로 되어 있으면, 수정 제안 전에 diagnostic instrumentation을 추가합니다.

   ```
   For EACH component boundary:
     - Log what data enters component
     - Log what data exits component
     - Verify environment/config propagation
     - Check state at each layer

   Run once to gather evidence showing WHERE it breaks
   THEN analyze evidence to identify failing component
   THEN investigate that specific component
   ```

   예시:

   ```powershell
   # Layer 1: Workflow
   Write-Host "=== Secrets available in workflow: ==="
   if ($env:IDENTITY) { Write-Host "IDENTITY: SET" } else { Write-Host "IDENTITY: UNSET" }

   # Layer 2: Build script
   Write-Host "=== Env vars in build script: ==="
   if ($env:IDENTITY) { Write-Host "IDENTITY=$env:IDENTITY" } else { Write-Host "IDENTITY not in environment" }

   # Layer 3: Signing script
   echo "=== Keychain state: ==="
   security list-keychains
   security find-identity -v

   # Layer 4: Actual signing
   codesign --sign "$IDENTITY" --verbose=4 "$APP"
   ```

   이렇게 하면 어느 계층에서 깨지는지 드러납니다.

5. **데이터 흐름 추적**

   오류가 call stack 깊은 곳에 있으면 이 디렉터리의 `root-cause-tracing.md`를 봅니다.

   빠른 버전:
   - 나쁜 값은 어디서 생겼습니까?
   - 누가 이 나쁜 값을 넘겼습니까?
   - 원인을 찾을 때까지 위로 추적합니다.
   - 증상이 아니라 원천에서 고칩니다.

### Phase 2: Pattern Analysis

수정 전에 패턴을 찾습니다.

1. **작동하는 예 찾기**
   - 같은 코드베이스에서 비슷하게 작동하는 코드를 찾습니다.
   - 깨진 코드와 비슷하지만 정상인 부분을 확인합니다.

2. **참조와 비교**
   - 패턴을 구현 중이면 reference implementation을 완전히 읽습니다.
   - 훑어보지 말고 모든 줄을 읽습니다.
   - 패턴을 이해한 뒤 적용합니다.

3. **차이 식별**
   - 정상 코드와 깨진 코드의 차이를 찾습니다.
   - 작아 보여도 모든 차이를 나열합니다.
   - "이건 상관없다"고 가정하지 않습니다.

4. **의존성 이해**
   - 어떤 컴포넌트가 필요합니까?
   - 어떤 설정, config, environment가 필요합니까?
   - 어떤 가정을 하고 있습니까?

### Phase 3: Hypothesis and Testing

과학적 방법을 따릅니다.

1. **단일 가설 세우기**
   - "나는 Y 때문에 X가 root cause라고 생각한다"처럼 명확히 말합니다.
   - 적어 둡니다.
   - 모호하지 않게 구체적으로 씁니다.

2. **최소한으로 테스트**
   - 가설을 테스트할 가장 작은 변경만 합니다.
   - 한 번에 하나의 변수만 바꿉니다.
   - 여러 수정을 한꺼번에 하지 않습니다.

3. **계속하기 전 검증**
   - 작동하면 Phase 4로 갑니다.
   - 작동하지 않으면 새 가설을 세웁니다.
   - 실패한 수정 위에 다른 수정을 쌓지 않습니다.

4. **모를 때**
   - "I don't understand X"라고 말합니다.
   - 아는 척하지 않습니다.
   - 도움을 요청하거나 더 조사합니다.

### Phase 4: Implementation

증상이 아니라 root cause를 고칩니다.

1. **실패 테스트 케이스 만들기**
   - 가능한 가장 단순한 재현
   - 가능하면 automated test
   - test framework가 없으면 one-off test script
   - 수정 전 반드시 있어야 합니다.
   - 올바른 실패 테스트 작성에는 `superpowers:test-driven-development` 스킬을 사용합니다.

2. **단일 수정 구현**
   - 식별한 root cause를 해결합니다.
   - 한 번에 하나의 변경만 합니다.
   - "하는 김에" 개선하지 않습니다.
   - 리팩터링을 묶지 않습니다.

3. **수정 검증**
   - 테스트가 이제 통과합니까?
   - 다른 테스트가 깨지지 않았습니까?
   - 실제 문제가 해결됐습니까?

4. **수정이 작동하지 않을 때**
   - 멈춥니다.
   - 지금까지 시도한 수정 수를 셉니다.
   - 3개 미만이면 Phase 1로 돌아가 새 정보로 다시 분석합니다.
   - **3개 이상이면 멈추고 아키텍처를 의심합니다.**
   - Fix #4를 시도하기 전에 아키텍처를 논의합니다.

   반복 실패는 아직 root-cause analysis 중이라는 뜻입니다.

5. **3개 이상 수정 실패: 아키텍처 질문**

   아키텍처 문제를 암시하는 패턴:
   - 각 수정이 다른 위치의 shared state, coupling, 문제를 드러냄
   - 수정하려면 massive refactoring이 필요함
   - 각 수정이 다른 증상을 새로 만듦

   멈추고 기본을 질문합니다.
   - 이 패턴은 근본적으로 타당합니까?
   - 관성 때문에 붙잡고 있습니까?
   - 증상 수정 대신 아키텍처를 리팩터링해야 합니까?
   - 목표나 경계가 틀렸다면 brainstorming으로 돌아가 문제를 다시 정의합니다.

   더 수정하기 전에 사용자와 논의합니다. 이것은 가설 실패가 아니라 잘못된 아키텍처일 수 있습니다.

## Red Flags - 멈추고 절차 따르기

다음 생각이 들면 Phase 1로 돌아갑니다.

- "일단 빠르게 고치고 나중에 조사하자"
- "X를 바꿔보고 되는지 보자"
- "여러 변경을 넣고 테스트하자"
- "테스트는 건너뛰고 수동으로 확인하자"
- "아마 X니까 고치자"
- "완전히 이해하지 못했지만 이게 될 것 같다"
- "패턴은 X지만 다르게 적용하자"
- 조사 없이 해결책 목록을 제시함
- data flow를 추적하기 전에 해결책을 제안함
- 이미 2번 이상 실패했는데 "한 번만 더" 시도함
- 각 수정이 다른 곳의 새 문제를 드러냄

**3개 이상 수정이 실패했다면:** Phase 4.5처럼 아키텍처를 의심합니다.

## 사용자의 신호

다음 말을 들으면 잘못된 방향일 수 있습니다.

- "Is that not happening?" - 확인 없이 가정했습니다.
- "Will it show us...?" - 증거 수집을 추가했어야 합니다.
- "Stop guessing" - 이해 없이 수정을 제안 중입니다.
- "Ultrathink this" - 증상보다 기본 구조를 의심해야 합니다.
- "We're stuck?" - 접근이 작동하지 않습니다.

이런 신호가 보이면 멈추고 Phase 1로 돌아갑니다.

## 흔한 합리화

| 변명 | 현실 |
| --- | --- |
| "단순해서 절차가 필요 없다" | 단순한 문제도 root cause가 있습니다. |
| "긴급해서 시간이 없다" | 체계적 디버깅이 시행착오보다 빠릅니다. |
| "일단 이걸 시도하고 조사하자" | 첫 수정부터 제대로 해야 합니다. |
| "수정이 되는지 본 뒤 테스트하겠다" | 테스트 없는 수정은 유지되지 않습니다. |
| "여러 수정을 한 번에 하면 빠르다" | 무엇이 효과가 있었는지 알 수 없습니다. |
| "reference가 길어서 대충 적용하겠다" | 부분 이해는 버그를 만듭니다. |
| "문제를 봤으니 고치자" | 증상을 본 것이 root cause 이해는 아닙니다. |
| "한 번만 더 시도" | 3개 이상 실패는 아키텍처 문제 신호입니다. |

## 빠른 참조

| Phase | 핵심 활동 | 성공 기준 |
| --- | --- | --- |
| **1. Root Cause** | 오류 읽기, 재현, 변경 확인, 증거 수집 | WHAT과 WHY를 이해 |
| **2. Pattern** | 작동 예 찾기, 비교 | 차이 식별 |
| **3. Hypothesis** | 이론 세우기, 최소 테스트 | 확인된 가설 또는 새 가설 |
| **4. Implementation** | 테스트 만들기, 수정, 검증 | 버그 해결, 테스트 통과 |

## "Root Cause 없음"처럼 보일 때

체계적 조사 결과 문제가 정말 환경, timing, 외부 요인이라면:

1. 절차를 완료한 것입니다.
2. 무엇을 조사했는지 문서화합니다.
3. 적절한 처리(retry, timeout, error message)를 구현합니다.
4. 향후 조사를 위해 monitoring/logging을 추가합니다.

하지만 "root cause 없음" 사례의 95%는 조사가 불완전한 경우입니다.

## 지원 기법

이 기법들은 systematic debugging의 일부이며 같은 디렉터리에 있습니다.

- **`root-cause-tracing.md`** - call stack을 거슬러 올라가 최초 트리거를 찾습니다.
- **`defense-in-depth.md`** - root cause를 찾은 뒤 여러 계층에 validation을 추가합니다.
- **`condition-based-waiting.md`** - 임의 timeout을 condition polling으로 바꿉니다.

**관련 스킬:**
- **superpowers:test-driven-development** - Phase 4 Step 1의 실패 테스트 케이스 작성
- **superpowers:verification-before-completion** - 성공을 주장하기 전 수정 검증

## 실제 영향

디버깅 세션 기준:

- 체계적 접근: 15-30분에 수정
- 무작위 수정: 2-3시간 시행착오
- 첫 수정 성공률: 95% 대 40%
- 새 버그 도입: 거의 없음 대 흔함
