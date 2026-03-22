# OpenAI AGENTS.md

## OpenAI AGENTS.md 구조

```text
home/
├── AGENTS.md : 넓게 적용되는 개인 규칙
└── repo-root/
    ├── AGENTS.md : 하나의 GitHub 저장소에 적용되는 규칙
    ├── feature-a/
    │   ├── AGENTS.md : 폴더별 규칙
    │   └── child/
    │       └── AGENTS.override.md : 더 강한 로컬 규칙
    └── feature-b/
```

## Codex가 읽는 방식

- Codex는 작업 시작 전에 `AGENTS.md` 체인을 읽는다.
- 체인은 홈 레벨 규칙에서 시작해 프로젝트 루트를 거쳐 현재 폴더까지 내려온다.
- 같은 폴더에서는 `AGENTS.override.md`가 `AGENTS.md`보다 우선한다.

## 실전 규칙

- 한 폴더에 더 강한 로컬 규칙이 필요할 때만 `AGENTS.override.md`를 쓴다.
- `AGENTS.md`는 지침 파일답게 짧게 유지한다.

## 제한

- 합쳐진 지침 크기는 보통 32 KiB 제한을 가진다.
- 체인이 너무 길어지면 더 가까운 폴더로 규칙을 나누거나 설정을 조정한다.
- `project_doc_fallback_filenames`가 설정되어 있으면 Codex는 대체 프로젝트 문서 이름도 쓸 수 있다.

## 출처

- Codex AGENTS.md guide: <https://developers.openai.com/codex/guides/agents-md> — 확인일 2026-03-21
