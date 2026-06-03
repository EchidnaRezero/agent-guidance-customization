---
name: using-wsl-environment
description: 현재 환경이 WSL이고 명령, 경로, 파일시스템 안내가 WSL 관례를 따라야 할 때 사용합니다.
---

# WSL 환경 사용

## 개요

WSL 안에서 명령, 경로, 파일시스템 동작을 제안하기 전에 이 스킬을 적용합니다. WSL은 Linux와 비슷하지만 Windows 경로, 브라우저, Git 상태와 자주 맞닿아 있습니다.

## 사용 대상

- WSL 셸 명령
- WSL 경로 변환
- WSL 쪽 Codex 또는 프로젝트 작업
- WSL과 Windows 경계를 넘는 브라우저 또는 서버 워크플로

## 명령 및 경로 규칙

- WSL 안에서는 POSIX 셸 문법을 사용합니다.
- WSL 안에서는 Linux 경로를 우선 사용합니다: `$HOME`, `/tmp`, 프로젝트 로컬 경로.
- Windows 쪽 파일이나 도구가 명시적으로 필요한 경우에만 Windows 경로로 변환합니다.
- 가능하면 하나의 명령 안에서는 하나의 경로 스타일만 사용합니다.

## Superpowers 적용 방식

- 셸 사용이 많은 Linux 안내는 작업이 WSL 안에 머무는지 확인한 뒤에만 유효한 것으로 봅니다.
- brainstorm 서버나 로컬 URL의 경우, 서버는 WSL 안에서 실행되어도 브라우저는 Windows 쪽에서 열릴 수 있다고 가정합니다.
- 서비스가 WSL 경계를 넘어 접근 가능해야 하면 명시적인 host binding과 `--url-host localhost`를 우선 사용합니다.

## 위험 신호

- 변환이 의도된 경우가 아니라면 같은 명령 안에서 `/mnt/c/...`와 `C:\...` 형식을 섞지 않습니다.
- WSL 안에 있으면서 Windows `cmd` 또는 PowerShell 의미론을 가정하지 않습니다.
- 작업이 Windows와 상호작용하지 않고 완전히 Linux 안에 머문다면 `using-linux-environment`가 더 단순할 수 있습니다.
