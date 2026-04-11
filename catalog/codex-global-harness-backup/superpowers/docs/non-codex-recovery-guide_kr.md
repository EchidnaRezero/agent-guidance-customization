# 비Codex 자산 복구 가이드

이 workbench는 현재 Codex 전용으로 정리되어 있다. 다른 에이전트를 다시 지원하려면 아래 표의 자산을 upstream `superpowers`에서 복원하고, 해당 설치 문서와 테스트도 함께 되돌려야 한다.

이번 정리에서 지운 비Codex 자산 종류는 아래와 같다.

- 에이전트별 설치 자산
- 에이전트별 플러그인 폴더와 hook 설정
- 에이전트별 도구 매핑 문서
- 에이전트별 테스트와 호환성 설계 문서

| 대상 | 원래 있던 자산 | 다시 지원하려면 복원하거나 대체할 것 |
| --- | --- | --- |
| `Claude Code` | `.claude-plugin/`, `CLAUDE.md`, `hooks/`, `docs/windows/polyglot-hooks.md`, `tests/claude-code/`, Claude 기반 설계/테스트 문서 | upstream `superpowers`에서 위 자산을 다시 가져오고, `README.md`에 Claude 설치 안내를 복원 |
| `Cursor` | `.cursor-plugin/`, `hooks/hooks-cursor.json` | upstream에서 Cursor 플러그인과 Cursor hook 설정을 복원하고, `README.md`에 Cursor 설치 안내를 복원 |
| `OpenCode` | `.opencode/`, `docs/README.opencode.md`, `tests/opencode/`, `package.json`의 OpenCode plugin entry | upstream에서 OpenCode 플러그인 자산과 문서, 테스트, `package.json`을 함께 복원 |
| `Gemini CLI` | `GEMINI.md`, `gemini-extension.json`, `skills/using-superpowers/references/gemini-tools.md` | upstream에서 위 세 파일을 복원하고, `README.md`에 Gemini 설치 안내를 복원 |
| `Copilot CLI` | `skills/using-superpowers/references/copilot-tools.md`, `README.md`의 Copilot 섹션 | upstream에서 Copilot 도구 매핑과 README 설치 섹션을 복원 |
| `Legacy agent-behavior tests` | `tests/explicit-skill-requests/`, `tests/skill-triggering/`, `tests/subagent-driven-dev/` | 다시 지원할 에이전트의 실행 방식에 맞게 upstream 테스트 자산을 복원하거나, 해당 에이전트용 테스트로 새로 작성 |

Codex-only 상태를 유지하려면 다음 두 파일은 Codex 기준으로 계속 관리해야 한다.

- `README.md`
- `skills/using-superpowers/SKILL.md`
