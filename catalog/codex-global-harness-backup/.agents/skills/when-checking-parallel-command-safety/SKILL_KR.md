---
name: when-checking-parallel-command-safety
description: "Git, SSH, 파일 편집, 설정 변경, 상태 확인 명령을 병렬 실행하기 전 사용합니다."
---

# When Checking Parallel Command Safety

서로 의존하거나 상태를 바꾸는 작업을 동시에 실행해서 잘못된 결과가 나오는 일을 막습니다.

## 핵심 규칙

1. 실행할 작업 목록을 먼저 적습니다.
2. 각 작업을 읽기 전용, 상태 변경, 인증/네트워크 민감, 사용자 입력 필요 중 무엇인지 표시합니다.
3. 모든 작업이 읽기 전용이고, 서로 독립적이며, 다른 작업이 바꿀 수 있는 상태를 읽지 않을 때만 병렬 실행합니다.
4. 그 외에는 순차적으로 실행하고, 끝난 뒤 새 단계에서 다시 확인합니다.

## 절대 병렬로 돌리지 말 것

- 같은 시스템에 대해 상태를 바꾸는 명령과 그 상태를 확인하는 명령
- 인증 설정과 그 인증에 의존하는 명령
- 파일 편집과 그 파일을 읽는 확인 작업
- 암호문 입력, 로그인, 원격 완료, 사용자 입력을 기다릴 수 있는 명령
- 사용자 중단 후의 불확실한 배치 작업; 이 경우에는 다시 순차적으로 확인합니다

## 흔한 사례

### Git

- `git add`, `git commit`, `git push`, `git pull`, `git fetch`, `git checkout`, `git config` 쓰기 작업은 순차적으로 실행합니다.
- `git status`, `git diff`, `git rev-parse`, `git ls-remote` 확인은 쓰기 작업이 끝난 뒤에만 합니다.

### SSH 및 인증

- 이후 단계가 의존할 경우 `ssh-add`, `gh auth login`, `gh auth status`, `ssh -T`는 순차적으로 실행합니다.

### 병렬로 해도 되는 작업

- 서로 관련 없는 여러 파일 읽기
- 서로 다른 디렉터리 나열하기
- 서로 독립적인 파일들의 diff 비교
- 한 파일의 compile check를 돌리면서 관련 없는 문서를 읽기

## 참고 자료

- 나쁜 조합과 더 안전한 대체 방식은 `references/risky-combinations.md`를 읽습니다.
