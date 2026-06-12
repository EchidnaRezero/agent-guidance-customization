---
name: when-powershell-command-fails
description: "Windows PowerShell 명령이 실패했고 Codex가 원인을 분석, 설명, 기록해야 할 때 사용합니다."
---

# When PowerShell Command Fails

## 목표

Codex 세션에서 발생한 Windows PowerShell 명령 실패를 분석하고 기록합니다.

## 범위

- Codex 안에서 실행한 Windows PowerShell 명령 실패
- 실행 전에 차단된 명령
- 잘못된 경로나 잘못된 현재 디렉터리
- 따옴표나 escaping 실수
- 신중하게 보고해야 하는 파괴적 명령

## 워크플로

1. 사용자의 목표와 실제로 실패한 PowerShell 명령을 분리합니다.
2. 실패 유형을 빠르게 분류합니다.
   - 실행 전에 차단됨
   - 경로 또는 작업 디렉터리 불일치
   - 따옴표 또는 escaping 문제
   - 권한 또는 잠금 문제
   - 현재 런타임에서 허용되지 않는 파괴적 명령
3. 실제 파일 변경이 성공했는지, 일부만 성공했는지, 아예 실행되지 않았는지 명시합니다.
4. 명령이 실행 전에 차단되었다면, 이것이 도구/런타임 정책 문제이지 자동으로 저장소 규칙이나 코드 오류를 뜻하는 것은 아니라고 설명합니다.
5. 가능한 경우 비파괴적 우회 방법을 우선합니다. 예: 남은 폴더를 그대로 두기, 수동 정리 제안, 허용된 명령으로만 재시도.
6. 실패가 발생하면 원인을 분석하고 `references/case-template.md`를 사용해 `references/failure-log.md`에 짧은 항목을 추가합니다.
7. 설명은 짧고 구체적으로 유지합니다: 사용자 목표, 실패한 명령, 실패 유형, 완료된 작업, 남은 작업.

## 응답 형태

- 사용자 목표
- 실패한 명령
- 실패 유형
- 실제로 완료된 것
- 안전한 다음 단계

## 참고 자료

- 알려진 실패 패턴과 표현 방식은 `references/failure-log.md`를 읽습니다.
- 새 사례를 추가할 때는 `references/case-template.md`를 사용합니다.
