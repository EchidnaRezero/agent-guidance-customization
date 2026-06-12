---
name: when-checking-powershell-command-safety
description: "위험하거나 파괴적이거나 차단될 수 있는 Windows PowerShell 명령을 실행하기 전 사용합니다."
---

# When Checking PowerShell Command Safety

## 목표

위험하거나 허용되지 않는 Windows PowerShell 명령이 실행되기 전에 막습니다.

## 워크플로

1. 실행하려는 명령이 파괴적인지, 정책상 민감한지, 또는 오용하기 쉬운지 확인합니다.
2. 명령이 위험하면 실행 전에 경고합니다.
3. 더 안전한 명령이 있으면 그것을 우선합니다.
4. 이 환경에서 그 명령 자체를 피해야 한다면 실행하지 않습니다.
