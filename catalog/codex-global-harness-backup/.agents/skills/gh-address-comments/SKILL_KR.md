---
name: gh-address-comments
description: 현재 브랜치의 열린 GitHub PR에 달린 리뷰/이슈 코멘트를 gh CLI로 처리하도록 돕습니다. 먼저 gh 인증 상태를 확인하고, 로그인되어 있지 않으면 사용자에게 인증을 요청합니다.
metadata:
  short-description: GitHub PR 리뷰의 코멘트를 처리합니다
---

# PR Comment Handler

현재 브랜치에 열려 있는 PR을 찾아, 그 PR의 코멘트를 gh CLI로 처리하는 가이드입니다.

사전 준비: `gh`가 인증되어 있는지 확인합니다. 예를 들어 한 번 `gh auth login`을 실행한 뒤, `gh auth status`를 실행해 `gh` 명령이 정상 동작하게 합니다.

## 1) 처리할 코멘트 확인
- `scripts/fetch_comments.py`를 실행하여 PR의 모든 코멘트와 리뷰 스레드를 출력합니다.

## 2) 사용자에게 확인 요청
- 모든 리뷰 스레드와 코멘트에 번호를 붙이고, 각 항목을 고치려면 무엇이 필요한지 짧게 요약합니다.
- 사용자에게 어떤 번호의 코멘트를 처리할지 묻습니다.

## 3) 사용자가 코멘트를 고르면
- 선택된 코멘트에 대한 수정 작업을 적용합니다.

참고:
- 실행 도중 `gh`가 인증 문제나 rate limit 문제를 일으키면, 사용자에게 `gh auth login`으로 다시 인증하라고 안내한 뒤 재시도합니다.
