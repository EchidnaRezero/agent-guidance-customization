---
name: workbench-builder
description: "하네스 파일과 설정이 들어 있는 실험용 workbench 아이템을 만들거나 업데이트한다. 다른 프로젝트용 workbench 항목을 만들 때 사용한다."
---

# 워크벤치 빌더

## 규칙

- `workbench/<name>/` 아래에서 작업한다.
- 실험 파일은 저장소 루트 출력물에 섞지 말고 `workbench/<name>/` 아래에 둔다.
- `references/`는 출력물이 아니라 공식 근거로 본다.
- 항목에 skill이 포함되면 skill 설명은 짧고, 트리거 중심으로 쓴다.
- 각 workbench 항목마다 `manifest.toml`을 만든다.
- `name`, `vendor`, `product`, `model`, `version`, `updated_on`을 기록한다.
- 항목은 `catalog/`로 승격되기 전까지 실험용으로 표시한다.
