---
name: writing-skills
description: 새 스킬을 만들거나, 기존 스킬을 편집하거나, 배포 전에 스킬이 제대로 작동하는지 검증할 때 사용합니다.
---

# 스킬 작성

## 개요

스킬 작성은 프로세스 문서에 적용하는 Test-Driven Development입니다.

개인 스킬은 상위 Codex root의 `../skills/`에 둡니다. Home `agents.md`는 최상위 정책이고, 스킬은 그 아래에 있는 참고 자료입니다. 스킬이 정책을 대체하려고 해서는 안 됩니다.

테스트 사례, 즉 pressure scenario를 만들고, 스킬 없이 실패하는 모습을 확인한 다음, 그 실패를 막는 `SKILL.md`를 작성하고, 다시 테스트해 통과하는지 확인합니다.

핵심 원칙: 스킬 없이 agent가 실패하는 모습을 보지 않았다면, 그 스킬이 무엇을 가르쳐야 하는지 아직 모르는 것입니다.

**REQUIRED BACKGROUND:** 이 스킬을 쓰기 전에 `superpowers:test-driven-development`를 이해해야 합니다. 그 스킬의 RED-GREEN-REFACTOR 흐름을 문서 작성에 적용합니다.

## 스킬이란 무엇인가

스킬은 검증된 기법, 패턴, 도구, 참고 정보를 담은 재사용 가능한 안내서입니다.

스킬은 다음입니다.

- 재사용 가능한 기법
- 패턴
- 도구
- 참고 가이드

스킬은 다음이 아닙니다.

- 한 번 문제를 해결한 과정을 적은 이야기
- 프로젝트 전용 규칙
- 자동화로 강제할 수 있는 단순 기계적 제약

운영체제별 동작이 다르면 Windows, macOS, Linux, WSL 안내를 한 스킬에 섞지 말고 별도 OS 스킬을 선호합니다.

## 스킬 TDD 대응표

| TDD 개념 | 스킬 작성 |
| --- | --- |
| **Test case** | subagent를 이용한 pressure scenario |
| **Production code** | 스킬 문서 (`SKILL.md`) |
| **Test fails (RED)** | 스킬 없이 agent가 규칙을 어김 |
| **Test passes (GREEN)** | 스킬을 읽은 agent가 지킴 |
| **Refactor** | 허점을 막으면서 준수를 유지함 |
| **Write test first** | 스킬 작성 전에 baseline scenario 실행 |
| **Watch it fail** | agent가 쓰는 합리화와 실패 양상을 기록 |
| **Minimal code** | 실제 실패를 막는 최소 스킬 작성 |
| **Refactor cycle** | 새 합리화 찾기, 막기, 재검증 |

전체 스킬 작성 과정은 RED-GREEN-REFACTOR를 따릅니다.

## 언제 스킬을 만들지

만들어야 할 때:

- 기법이 직관적으로 obvious하지 않았을 때
- 여러 프로젝트에서 다시 참고할 가능성이 있을 때
- 넓게 적용되는 패턴일 때
- 다른 사람에게도 도움이 될 때

만들지 말아야 할 때:

- 일회성 해결책
- 이미 잘 문서화된 표준 관행
- 프로젝트 전용 규칙 (`AGENTS.md`에 둠)
- regex나 검증 도구로 강제할 수 있는 기계적 제약

## 스킬 유형

### Technique

따라 할 수 있는 구체적 방법입니다. 예: `condition-based-waiting`, `root-cause-tracing`

### Pattern

문제를 바라보는 사고방식입니다. 예: `flatten-with-flags`, `test-invariants`

### Reference

API 문서, 문법 가이드, 도구 문서입니다. 예: office docs

## 디렉터리 구조

```text
skills/
  skill-name/
    SKILL.md              # Main reference (required)
    supporting-file.*     # Only if needed
```

- 모든 스킬은 flat namespace에 둡니다.
- heavy reference나 reusable tool이 필요할 때만 별도 파일을 둡니다.
- 원칙, 개념, 50줄 미만 code pattern은 `SKILL.md` 안에 둡니다.

## `SKILL.md` 구조

Frontmatter에는 `name`과 `description`이 필요합니다.

- `name`: 글자, 숫자, hyphen만 사용합니다.
- `description`: third-person으로, 언제 사용할지만 설명합니다.
- `description`은 `Use when...`으로 시작하고, workflow 요약을 넣지 않습니다.
- 전체 frontmatter는 1024자 이내를 목표로 합니다.

기본 구조:

```markdown
---
name: Skill-Name-With-Hyphens
description: Use when [specific triggering conditions and symptoms]
---

# Skill Name

## Overview
핵심 원칙을 1-2문장으로 설명합니다.

## When to Use
사용할 상황과 사용하지 않을 상황을 적습니다.

## Core Pattern
기법이나 패턴이면 before/after 비교를 둡니다.

## Quick Reference
빠르게 훑을 수 있는 표나 목록을 둡니다.

## Implementation
간단한 패턴은 inline code, 큰 reference나 tool은 별도 파일로 둡니다.

## Common Mistakes
자주 틀리는 점과 수정법을 적습니다.
```

## Skill Discovery Optimization (SDO)

미래의 Codex가 스킬을 찾아야 하므로, `description`은 "지금 이 스킬을 읽어야 하는가?"에 답해야 합니다.

### `description` 규칙

- `Use when...`으로 시작합니다.
- 문제, 증상, 상황을 설명합니다.
- workflow나 절차를 요약하지 않습니다.
- first-person을 쓰지 않습니다.
- 가능하면 500자 아래로 유지합니다.

좋은 예:

```yaml
description: Use when tests have race conditions, timing dependencies, or pass/fail inconsistently
description: Use when using React Router and handling authentication redirects
```

나쁜 예:

```yaml
description: Use for TDD - write test first, watch it fail, write minimal code, refactor
description: I can help you with async tests when they're flaky
```

### 검색 키워드

Codex가 검색할 단어를 본문에 포함합니다.

- 오류 메시지: `"Hook timed out"`, `"ENOTEMPTY"`, `"race condition"`
- 증상: `"flaky"`, `"hanging"`, `"zombie"`, `"pollution"`
- 동의어: `"timeout/hang/freeze"`, `"cleanup/teardown/afterEach"`
- 도구와 파일 형식: 실제 명령, 라이브러리 이름, 파일명

### 이름 짓기

동작을 드러내는 이름을 선호합니다.

- `creating-skills`가 `skill-creation`보다 낫습니다.
- `condition-based-waiting`이 `async-test-helpers`보다 낫습니다.
- 프로세스에는 `creating-`, `testing-`, `debugging-` 같은 gerund가 잘 맞습니다.

### 토큰 효율

자주 로드되는 스킬은 짧아야 합니다.

- getting-started workflow: 각각 150단어 미만 목표
- 자주 참조되는 스킬: 전체 200단어 미만 목표
- 그 외 스킬: 500단어 미만 목표

세부 옵션을 길게 문서화하기보다 `--help`를 참조하고, 다른 스킬의 workflow를 반복하지 말고 스킬 이름으로 cross-reference합니다.

## 다른 스킬 참조

다른 스킬을 참조할 때는 경로 대신 스킬 이름과 필요성을 명확히 씁니다.

- 좋은 예: `**REQUIRED SUB-SKILL:** Use superpowers:test-driven-development`
- 좋은 예: `**REQUIRED BACKGROUND:** You MUST understand superpowers:systematic-debugging`
- 나쁜 예: `See skills/testing/test-driven-development`
- 나쁜 예: `@skills/testing/test-driven-development/SKILL.md`

`@` 링크는 파일을 즉시 force-load하여 context를 많이 쓰므로 피합니다.

## Flowchart 사용

flowchart는 agent가 잘못 판단할 수 있는 작고 중요한 결정 지점에만 사용합니다.

사용할 때:

- non-obvious decision point
- 너무 일찍 멈출 수 있는 process loop
- "A와 B 중 무엇을 쓸지" 결정

사용하지 말아야 할 때:

- reference material: 표나 목록 사용
- code example: Markdown code block 사용
- 선형 절차: numbered list 사용
- `step1`, `helper2` 같은 의미 없는 label

사용자에게 보여줄 그래프는 이 디렉터리의 `render-graphs.js`로 SVG를 렌더링합니다.

```bash
./render-graphs.js ../some-skill
./render-graphs.js ../some-skill --combine
```

## Code Example

좋은 예제 하나가 여러 개의 평범한 예제보다 낫습니다.

- testing technique: TypeScript/JavaScript 선호
- system debugging: Shell/Python 선호
- data processing: Python 선호

예제는 실행 가능하고, 왜 그렇게 하는지 주석이 있으며, 실제 상황에서 가져온 패턴이어야 합니다. 여러 언어로 얇게 나누거나 빈칸 채우기 template를 만들지 않습니다.

## 파일 구성

Self-contained skill:

```text
defense-in-depth/
  SKILL.md
```

Reusable tool이 있는 skill:

```text
condition-based-waiting/
  SKILL.md
  example.ts
```

Heavy reference가 있는 skill:

```text
pptx/
  SKILL.md
  pptxgenjs.md
  ooxml.md
  scripts/
```

## Iron Law

```text
NO SKILL WITHOUT A FAILING TEST FIRST
```

새 스킬과 기존 스킬 편집 모두에 적용됩니다.

스킬을 쓰기 전에 테스트하지 않았다면 삭제하고 다시 시작합니다. "간단한 추가", "문서 업데이트", "나중에 테스트" 같은 예외는 없습니다. 작성한 내용을 참고로 남겨 두거나 테스트 작성에 맞춰 "adapt"하지 않습니다. 삭제는 삭제입니다.

**REQUIRED BACKGROUND:** `superpowers:test-driven-development`가 이 원칙의 이유를 설명합니다.

## 스킬 테스트 방식

### Discipline-Enforcing Skills

예: TDD, verification-before-completion, designing-before-coding

- 규칙 이해 질문
- 시간 압박, sunk cost, authority, exhaustion을 섞은 pressure scenario
- agent가 쓰는 합리화를 찾아 명시적으로 막기

성공 기준: 강한 압박에서도 규칙을 지킵니다.

### Technique Skills

예: condition-based-waiting, root-cause-tracing, defensive-programming

- 실제 적용 scenario
- edge case variation
- 빠진 정보가 있는지 확인

성공 기준: 새 상황에도 기법을 올바르게 적용합니다.

### Pattern Skills

예: reducing-complexity, information-hiding concepts

- 언제 패턴이 맞는지 인식하는 scenario
- 적용 scenario
- 반례

성공 기준: 언제 어떻게 적용할지 올바르게 판단합니다.

### Reference Skills

예: API documentation, command references, library guides

- 필요한 정보를 찾는 scenario
- 찾은 정보를 올바르게 적용하는 scenario
- 흔한 사용 사례의 gap 확인

성공 기준: 필요한 reference를 찾아 정확히 적용합니다.

## 테스트 생략 합리화

| 변명 | 실제 의미 |
| --- | --- |
| "Skill is obviously clear" | 나에게 명확한 것과 다른 agent에게 명확한 것은 다릅니다. |
| "It's just a reference" | reference도 빠진 부분이나 모호한 부분이 있을 수 있습니다. |
| "Testing is overkill" | 테스트하지 않은 스킬에는 문제가 생깁니다. |
| "I'll test if problems emerge" | 문제가 생긴 뒤에는 이미 agent가 스킬을 못 쓰고 있는 상태입니다. |
| "No time to test" | 테스트 없는 배포가 나중에 더 많은 시간을 씁니다. |

이런 말이 나오면 테스트해야 합니다.

## 합리화 막기

규율을 강제하는 스킬은 agent가 압박 속에서 규칙을 피할 구멍을 찾지 못하게 해야 합니다.

- 규칙만 말하지 말고 구체적 우회 방법을 금지합니다.
- "letter vs spirit" 논리를 막습니다.
- baseline testing에서 나온 합리화를 표로 정리합니다.
- red flag 목록을 만들어 agent가 스스로 멈추게 합니다.

예:

```markdown
Write code before test? Delete it. Start over.

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete
```

## RED-GREEN-REFACTOR for Skills

### RED: 실패하는 테스트 작성

스킬 없이 pressure scenario를 실행하고 baseline behavior를 기록합니다.

- 어떤 선택을 했는가
- 어떤 합리화를 했는가
- 어떤 압박이 위반을 만들었는가

### GREEN: 최소 스킬 작성

baseline에서 확인한 합리화를 막는 최소 스킬을 작성합니다. 가상의 문제를 위해 내용을 늘리지 않습니다. 같은 scenario를 스킬과 함께 다시 실행해 지키는지 확인합니다.

### REFACTOR: 허점 막기

agent가 새 합리화를 찾으면 명시적으로 막고 다시 테스트합니다. 필요하면 `testing-skills-with-subagents.md`의 방법론을 따릅니다.

## Anti-Patterns

### Narrative Example

특정 날짜나 세션에서 한 번 해결한 이야기를 쓰지 않습니다. 재사용 가능한 패턴이 아닙니다.

### Multi-Language Dilution

여러 언어로 얇은 예제를 많이 만들지 않습니다. 유지보수 부담만 늘어납니다.

### Code in Flowcharts

flowchart 안에 code를 넣지 않습니다. 복사하기 어렵고 읽기도 어렵습니다.

### Generic Labels

`helper1`, `step3` 같은 의미 없는 label을 쓰지 않습니다.

## 다음 스킬로 넘어가기 전

스킬을 하나 작성한 뒤에는 멈추고 deployment process를 끝냅니다.

- 여러 스킬을 한꺼번에 만들고 각 스킬 테스트를 건너뛰지 않습니다.
- 현재 스킬 검증 전 다음 스킬로 넘어가지 않습니다.
- batch가 효율적이라는 이유로 테스트를 생략하지 않습니다.

## 스킬 작성 체크리스트

RED:

- pressure scenario 작성
- 스킬 없이 scenario 실행, baseline behavior 기록
- 합리화와 실패 패턴 식별

GREEN:

- `name`은 글자, 숫자, hyphen만 사용
- YAML frontmatter에 `name`, `description` 포함
- `description`은 `Use when...`으로 시작하고 third-person으로 작성
- 검색 keyword 포함
- 핵심 원칙이 있는 overview 작성
- baseline failure를 직접 막는 내용 작성
- code는 inline 또는 별도 파일로 배치
- 좋은 예제 하나 작성
- 스킬과 함께 scenario 재실행

REFACTOR:

- 새 합리화 식별
- 규율 스킬이면 명시적 counter 추가
- rationalization table과 red flags 작성
- 통과할 때까지 재검증

Quality:

- flowchart는 필요한 작은 결정에만 사용
- quick reference table 제공
- common mistakes section 제공
- narrative storytelling 금지
- supporting files는 tool 또는 heavy reference에만 사용

Deployment:

- 필요하면 git commit 및 push
- 넓게 유용하면 PR 고려

## Discovery Workflow

미래 Codex가 스킬을 찾는 흐름:

1. 문제를 만납니다. 예: `"tests are flaky"`
2. `description`으로 SKILL을 찾습니다.
3. overview로 관련성을 확인합니다.
4. quick reference와 pattern을 읽습니다.
5. 필요할 때만 example을 로드합니다.

## 핵심 정리

스킬 작성은 문서에 적용하는 TDD입니다.

같은 Iron Law를 따릅니다: 실패하는 테스트 없는 스킬은 없습니다.

같은 cycle을 따릅니다: RED, GREEN, REFACTOR.

코드에 TDD를 적용한다면, 스킬에도 같은 규율을 적용합니다.
