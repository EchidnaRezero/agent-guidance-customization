---
name: item-internal-path-check
description: "하나의 workbench 또는 catalog 아이템 안에서 쓰는 경로가 `workbench/`나 `catalog/` 같은 상위 경로가 아니라 그 아이템 루트 기준 상대경로인지 점검합니다."
---

# item-internal-path-check

## 작성 기준

- 하나의 `workbench/<name>/` 아이템에서는 그 `workbench/<name>/` 폴더를 아이템 루트로 봅니다.
- 하나의 `catalog/<name>/` 아이템에서는 그 아이템의 `AGENTS.md`가 있는 디렉터리를 아이템 루트로 봅니다.
- 아이템 내부 경로는 그 아이템 루트를 기준으로 상대경로로 씁니다.
- 아이템 내부 경로에 `workbench/`, `catalog/`, 또는 저장소 루트 경로를 붙이지 않습니다.
- 아이템 안의 하위 디렉터리에 별도 `AGENTS.md`가 있으면, 그 하위 디렉터리를 그 안에서 쓰는 경로의 로컬 루트로 봅니다.
- 경로가 아이템 루트 밖을 가리켜야 하면, 쓰기 전에 사용자에게 확인합니다.

## 검토 기준

- 현재 아이템 안의 모든 경로가 위 작성 기준을 따르는지 확인합니다.
- 필요하면 아이템 내부 경로를 아이템 루트 기준 상대경로로 고칩니다.
