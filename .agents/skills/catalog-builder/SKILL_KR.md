---
name: catalog-builder
description: "workbench에서 완성된 항목을 catalog/로 승격한다. 실험용 workbench 아이템을 재사용 가능한 catalog 출력으로 옮길 때 사용한다."
---

# 카탈로그 빌더

## 규칙

- `workbench/<name>/`에서 `catalog/<name>/`으로 승격한다.
- 승격하기 전에 어떤 파일이 최종 산출물인지 확인한다.
- 승격된 항목 루트에 `manifest.toml` 하나를 만든다.
- 대화의 첫 승격에서는 현재 agent, model, provider를 조회한 뒤, 해당 환경 튜플을 기록하기 전에 사용자에게 확인을 요청한다.
- 같은 대화에서 이후 승격할 때는 확인된 환경 튜플을 재사용한다. 새 대화마다 다시 확인한다.
- 항목 버전, manifest 생성 또는 업데이트 시점의 UTC 달력 날짜를 `YYYY-MM-DD` 형식으로 기록하고, 확인된 환경 튜플을 기록한다.

```toml
version = "1.0.0"
updated_on = "2026-07-19"
environment = { agent = "codex", model = "gpt-5.6-sol", provider = "openai" }
```

- 최종 항목에 필요한 파일만 남긴다.
- 노트, 초안, 기타 중간 산출물은 제거한다.
- 승격 후 원래 workbench 항목을 삭제한다.
