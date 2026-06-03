---
name: using-macos-environment
description: 현재 환경이 macOS이고 명령, 경로, 파일시스템 안내가 macOS 관례를 따라야 할 때 사용합니다.
---

# macOS 환경 사용

## 개요

작업이 macOS에서 진행되고 안내가 macOS 셸, 경로, 파일시스템 관례를 따라야 할 때 이 스킬을 적용합니다.

## 사용 대상

- macOS 셸 명령
- macOS 파일시스템 경로
- POSIX 스크립트 또는 권한
- macOS 쪽 Codex 스킬 설치 또는 업데이트
- macOS 전용 worktree 또는 임시 디렉터리 처리

## 명령 및 경로 규칙

- POSIX 셸 문법을 사용합니다.
- 사용자 홈 경로에는 `$HOME`을 우선 사용합니다.
- 일반적인 사용자 경로는 `/Users/<name>/...` 아래에 있습니다.
- 임시 데이터에는 `/tmp`를 우선 사용합니다.
- 심볼릭 링크에는 `ln -s`, 실행 스크립트 권한에는 `chmod +x`를 사용합니다.
- 공백이 들어갈 수 있는 경로는 따옴표로 감쌉니다.

## Superpowers 적용 방식

- Bundled superpowers root: `.`
- Bundled skills path: `skills/`
- 이 bundled item의 공유 worktree 위치: `worktrees/<project-name>/`
- 이 스킬을 로드한 뒤에는 `using-git-worktrees`, `brainstorming/visual-companion.md`처럼 셸 사용이 많은 스킬을 macOS 관례로 해석할 수 있습니다.

## macOS 참고

- macOS에서 브라우저 URL이나 로컬 파일을 열어야 하면 `open <url-or-path>`를 사용합니다.
- `fs.watch` 동작을 판단할 때 macOS의 파일시스템 watcher 차이를 고려합니다.
- 스킬에서 macOS 패키지 관리자 예시가 필요하면 Homebrew 패키지 이름을 사용합니다.

## 위험 신호

- macOS 셸 명령에 Windows `C:\...` 경로를 섞지 않습니다.
- 여기서는 `cmd` 또는 PowerShell 문법을 사용하지 않습니다.
- 실제 환경이 Linux 또는 WSL이면 그에 맞는 스킬을 사용합니다.
