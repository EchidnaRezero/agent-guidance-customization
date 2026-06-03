이 번들 `superpowers` 사본은 Codex 전용입니다.

- 이 번들 `superpowers/` 디렉터리를 내부 경로의 프로젝트 루트로 취급하세요.
- 이 번들 item 안에서는 `.codex/INSTALL.md`, `skills/`, `docs/`, `hooks/`, `worktrees/`처럼 이 루트 기준의 상대 경로를 쓰세요.
- 상위 패키지 `AGENTS.md`가 이 번들 `superpowers` 사본보다 위에 있는 정책 계층입니다.
- 진입 워크플로로 `skills/using-superpowers/SKILL.md`를 사용하세요.
- 현재 환경이 Windows가 아니면, 셸 작업이 많은 스킬을 따르기 전에 해당 OS 환경 스킬을 먼저 불러오세요.
- Codex 설정 세부 사항은 `.codex/INSTALL.md`와 `docs/README.codex.md`를 사용하세요.
- Codex가 아닌 agent용 asset은 이 번들 패키지에서 제거되었습니다. 다른 agent 지원을 복구해야 하면 `docs/non-codex-recovery-guide_kr.md`를 보세요.
