# Reference Log

참고 문서의 날짜나 버전을 남길 때는 아래 순서로 기록한다.

1. 문서에 버전 표기가 있으면 그 값을 적는다.
2. 문서에 버전이 없으면 페이지에 보이는 `Last updated`를 적는다.
3. Context7에서 조회했다면 `Context7 checked on` 날짜를 같이 적는다.
4. 그것도 없으면 HTTP `Last-Modified` 헤더를 적는다.
5. 그것도 없으면 `확인일(checked on)`을 적는다.
6. GitHub 저장소면 릴리스 태그가 없을 경우 기본 브랜치 최신 커밋 SHA와 날짜를 적는다.

## 기록 형식

| 항목 | 의미 |
| --- | --- |
| Source | 문서나 저장소 이름 |
| URL | 원문 링크 |
| Type | doc, spec, repo 중 하나 |
| Version or marker | 버전, Last updated, 커밋 SHA 같은 식별값 |
| Context7 checked on | Context7로 최신 내용을 조회한 날짜 |
| Observed on | 이 값을 확인한 날짜 |
| Notes | 애매한 점이나 보충 설명 |

## 현재 참고 자료 기록

2026-03-21 기준으로 확인.

| Source | URL | Type | Version or marker | Context7 checked on | Observed on | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| AGENTS.md | https://agents.md/ | spec | 버전 표기 없음 | - | 2026-03-21 | `ETag`는 보였지만 사람이 읽는 버전 표기는 확인하지 못함 |
| Agent Skills Specification | https://agentskills.io/specification | spec | 버전 표기 없음 | - | 2026-03-21 | 페이지에서 명시적 버전/업데이트 표기를 확인하지 못함 |
| Codex AGENTS.md guide | https://developers.openai.com/codex/guides/agents-md | doc | HTTP `Last-Modified: 2026-03-20 19:56:28 GMT` | 2026-03-21 | 2026-03-21 | Context7 또는 공식 문서 재조회 시 날짜를 갱신 |
| Codex Skills guide | https://developers.openai.com/codex/skills | doc | HTTP `Last-Modified: 2026-03-20 18:39:47 GMT` | 2026-03-21 | 2026-03-21 | Context7 또는 공식 문서 재조회 시 날짜를 갱신 |
| Claude Code memory / CLAUDE.md | https://docs.anthropic.com/en/docs/claude-code/memory | doc | 버전 표기 없음 | 2026-03-21 | 2026-03-21 | 페이지는 열람했지만 헤더 기반 날짜는 안정적으로 얻지 못함 |
| openai/skills | https://github.com/openai/skills | repo | `main@82d2c5b44ac234ec0f204f647562a4349d90ef43` | - | 2026-03-21 | GitHub API 기준 latest main commit date: `2026-03-20T17:49:50Z`, releases 0개 |

## 다시 확인할 때 쓰는 명령

Windows PowerShell 기준:

```powershell
Invoke-WebRequest -Uri "https://developers.openai.com/codex/skills" -Method Head |
  Select-Object -ExpandProperty Headers
```

GitHub 저장소 최신 커밋 확인:

```powershell
Invoke-RestMethod -Uri "https://api.github.com/repos/openai/skills/commits/main"
```

## 실무 팁

- 문서 본문에 버전이 없으면 `확인일`을 꼭 같이 적는다.
- Context7로 다시 확인했으면 `Context7 checked on`도 같이 적는다.
- 화면에 보이는 날짜와 HTTP 헤더 날짜는 다를 수 있으니, 무엇을 기록했는지 Notes에 적는다.
- 중요한 연구 문서는 인용할 때 `URL`만 말하지 말고 `확인일`도 같이 적는다.
