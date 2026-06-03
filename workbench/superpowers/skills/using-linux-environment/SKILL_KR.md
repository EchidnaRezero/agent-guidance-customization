---
name: using-linux-environment
description: 현재 환경이 Linux이고 명령, 경로, 파일시스템 안내가 Linux 관례를 따라야 할 때 사용합니다.
---

# Linux 환경 사용

## 개요

작업이 Linux에서 진행되고 안내가 Linux 셸, 경로, 파일시스템 관례를 따라야 할 때 이 스킬을 적용합니다.

## 사용 대상

- Linux 셸 명령
- Linux 파일시스템 경로
- POSIX 스크립트 또는 권한
- Linux 쪽 Codex 스킬 설치 또는 업데이트
- Linux 전용 worktree 또는 임시 디렉터리 처리

## 명령 및 경로 규칙

- POSIX 셸 문법을 사용합니다.
- 사용자 홈 경로에는 `$HOME`을 우선 사용합니다.
- 임시 데이터에는 `/tmp`를 우선 사용합니다.
- 심볼릭 링크에는 `ln -s`, 실행 스크립트 권한에는 `chmod +x`를 사용합니다.
- 공백이 들어갈 수 있는 경로는 따옴표로 감쌉니다.

## Superpowers 적용 방식

- Bundled superpowers root: `.`
- Bundled skills path: `skills/`
- 이 bundled item의 공유 worktree 위치: `worktrees/<project-name>/`
- 이 스킬을 로드한 뒤에는 `using-git-worktrees`, `brainstorming/visual-companion.md`처럼 셸 사용이 많은 스킬을 Linux 관례로 해석할 수 있습니다.

## 브라우저 및 서버 참고

- Bash 스크립트는 Linux 셸에서 직접 실행할 수 있습니다.
- `--project-dir`는 brainstorm 파일을 `/tmp` 대신 프로젝트 아래에 둡니다.
- 브라우저가 Linux 환경 밖에 있으면, 접근 가능한 host에 bind하고 `--url-host`를 그에 맞게 설정합니다.

## 위험 신호

- Linux 셸 명령에 Windows `C:\...` 경로를 섞지 않습니다.
- 여기서는 `cmd` 또는 PowerShell 문법을 사용하지 않습니다.
- 실제 환경이 WSL이면 대신 `using-wsl-environment`를 사용합니다.
