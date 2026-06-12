---
name: when-creating-or-updating-skills
description: Codex skill을 만들거나 업데이트할 때 사용합니다. SKILL.md, resource, metadata, Korean companion을 포함합니다.
metadata:
  short-description: 스킬 만들기 또는 수정하기
---

# When Creating Or Updating Skills

이 스킬은 효과적인 스킬을 만들고 수정하는 방법을 안내합니다.

## 스킬이 하는 일

스킬은 특정 작업을 위해 Codex에 절차, 지식, 도구 사용법을 추가하는 모듈형 폴더입니다.

### 스킬이 제공하는 것

1. 특정 작업용 절차
2. 파일 형식이나 API에 맞는 도구 사용법
3. 도메인 지식
4. 반복 작업을 줄이는 스크립트, 참고자료, 자산

## 핵심 원칙

### 짧게 쓸 것

컨텍스트는 공유 자원입니다. Codex가 이미 알고 있을 가능성이 높은 설명은 줄이고, 실제로 필요한 정보만 넣습니다.

### 자유도를 작업 성격에 맞출 것

- 높은 자유도: 여러 접근이 가능한 작업
- 중간 자유도: 선호 패턴은 있지만 약간의 변형이 가능한 작업
- 낮은 자유도: 순서와 방식이 틀리면 쉽게 망가지는 작업

### 검증은 독립적으로 할 것

필요하면 서브에이전트로 스킬을 실제 작업에 써 보며 검증할 수 있습니다. 이때는 정답이나 의도한 수정 내용을 넘기지 말고, 실제 작업물과 요청만 넘겨서 스킬이 일반화되는지 봅니다.

## 스킬 구조

모든 스킬은 필수 `SKILL.md`와 선택 자원으로 이루어집니다.

```text
skill-name/
|-- SKILL.md
|-- agents/
|   `-- openai.yaml
|-- scripts/
|-- references/
`-- assets/
```

### SKILL.md

- Frontmatter: `name`, `description`
- Body: 실제 사용 지침

이 원본 지침과 다른 로컬 Codex 하네스 예외는 `references/local-skill-creator-exceptions.md`를 읽습니다.

### agents 메타데이터

- UI 표시용 메타데이터
- `references/openai_yaml.md`를 먼저 읽고 값 생성
- `display_name`, `short_description`, `default_prompt`는 스킬 내용을 보고 만든다
- `agents/openai.yaml`이 스킬과 어긋나면 다시 생성한다

### 선택 자원

#### scripts/

- 반복해서 다시 쓰게 되는 코드
- 정확성과 재현성이 중요한 작업용 코드

#### references/

- 작업 중 필요할 때만 읽는 참고자료
- 스키마, 정책, API 문서, 세부 절차

#### assets/

- 결과물에 직접 쓰는 파일
- 템플릿, 이미지, 글꼴, 샘플 문서

### 스킬에 넣지 말 것

다음 같은 보조 문서는 만들지 않습니다.

- `README.md`
- `INSTALLATION_GUIDE.md`
- `QUICK_REFERENCE.md`
- `CHANGELOG.md`

스킬은 에이전트가 일을 하는 데 필요한 내용만 가져야 합니다.

## 점진적 공개 원칙

스킬은 세 단계로 읽힙니다.

1. 메타데이터
2. `SKILL.md` 본문
3. 필요할 때만 읽는 자원 파일

`SKILL.md`는 핵심만 두고, 길어지면 `references/`로 나눕니다.

### 패턴

#### 1. 개요 + 참고자료

핵심 절차는 `SKILL.md`에 두고, 세부 내용은 별도 파일로 뺍니다.

#### 2. 도메인별 분리

여러 분야를 다루면 분야별 참고자료로 나눠서 필요한 것만 읽게 합니다.

#### 3. 조건부 세부사항

기본 흐름은 `SKILL.md`, 고급 기능은 별도 참고자료로 연결합니다.

중요한 규칙:

- 참고자료는 `SKILL.md`에서 직접 연결되게 합니다.
- 긴 참고자료는 상단에 목차를 둡니다.

## 한국어 동반 파일 규칙

스킬을 만들거나 수정할 때는 `SKILL.md`와 함께 한국어 동반 파일도 필수로 취급합니다.

- 스킬을 만들 때는 `_KR`가 들어간 한국어 Markdown 파일도 같이 만듭니다.
- 스킬을 수정할 때는 한국어 동반 파일을 현재 `SKILL.md`와 동기화합니다.
- 메인 스킬이 바뀌었는데 한국어판이 빠졌거나 일부만 바뀐 상태로 두지 않습니다.

## 스킬 제작 절차

1. 실제 사용 예를 통해 스킬을 이해한다
2. 재사용할 내용물을 계획한다
3. 스킬을 초기화한다
4. 스킬을 구현하고 문서를 쓴다
5. 기본 검증을 돌린다
6. 실제 사용 결과로 반복 개선한다

한국어 사용자가 함께 읽어야 하는 스킬이라면, `SKILL.md`를 만들거나 수정할 때 같은 폴더에 `SKILL_KR.md`도 같이 만들거나 수정합니다.

### 스킬 이름

- 소문자, 숫자, 하이픈만 사용
- 64자 이하
- 가능하면 짧고 동작이 드러나는 이름
- 폴더 이름은 스킬 이름과 같게

### 1단계: 실제 예로 이해하기

가능하면 사용 예를 먼저 모읍니다.

- 어떤 요청이 이 스킬을 트리거하는지
- 어떤 작업까지 지원해야 하는지
- 어디에 만들지

### 2단계: 재사용할 내용 계획하기

각 예시를 보고 무엇을 스크립트, 참고자료, 자산으로 빼면 좋을지 정합니다.

예:

- PDF 회전 작업이 반복되면 `scripts/rotate_pdf.py`
- 프런트엔드 앱 기본 틀이 반복되면 `assets/hello-world/`
- BigQuery 스키마를 매번 다시 찾게 되면 `references/schema.md`

### 3단계: 스킬 초기화

새 스킬이면 `init_skill.py`를 사용합니다.

기본 위치:

- 현재 Codex 루트의 `skills/`

사용 예:

```bash
scripts/init_skill.py my-skill --path skills
scripts/init_skill.py my-skill --path skills --resources scripts,references
scripts/init_skill.py my-skill --path custom/skills --resources scripts --examples
```

초기화가 끝난 뒤에는 `_KR`가 들어간 한국어 동반 파일도 함께 만들어야 초기화가 완료된 것으로 봅니다.

### 4단계: 스킬 편집

스킬은 다른 Codex가 쓰는 작업 안내서라고 생각하고 씁니다.

#### 재사용 자원부터 만들기

먼저 `scripts/`, `references/`, `assets/`를 채웁니다.

- 스크립트는 실제로 실행해 검증합니다.
- 예시 파일을 만들었다면 필요 없는 placeholder는 지웁니다.

#### SKILL.md 업데이트

작성은 명령형/동사형으로 씁니다.

`SKILL.md`를 바꿀 때는 같은 작업 흐름 안에서 한국어 동반 파일도 함께 바꿔 두 파일이 계속 동기화되게 합니다.

##### Frontmatter

- `name`
- `description`

설명에는 반드시 다음이 들어가야 합니다.

- 무엇을 하는 스킬인지
- 언제 써야 하는지

트리거 정보는 본문이 아니라 description에 넣습니다.

이 규칙과 다른 로컬 Codex 하네스 예외는 `references/local-skill-creator-exceptions.md`를 읽습니다.

##### Body

스킬 사용 절차와 자원 사용법을 적습니다.

### 5단계: 검증

기본 검증:

```bash
scripts/quick_validate.py <path/to/skill-folder>
```

frontmatter 형식, 필수 필드, 이름 규칙 등을 확인합니다.

### 6단계: 반복 개선

실제 사용 후:

1. 써 본다
2. 어디서 막히는지 본다
3. `SKILL.md`나 자원을 고친다
4. 다시 검증한다
5. 필요하면 forward-test 한다

## Forward-testing

서브에이전트에게 실제 사용자처럼 요청해서 스킬을 시험합니다.

좋은 프롬프트:

`Use $skill-x at /path/to/skill-x to solve problem y`

나쁜 프롬프트:

`Review the skill at /path/to/skill-x; pretend a user asks you to...`

주의:

- 새 스레드 사용
- 원본 작업물만 넘길 것
- 기대 정답이나 의도한 수정은 넘기지 말 것
- 반복마다 맥락을 다시 구성할 것
- 남은 테스트 찌꺼기는 정리할 것

만약 숨은 맥락이 있어야만 잘 되면, 스킬이나 검증 방식이 잘못된 것입니다.
