---
name: catalog-builder
description: "workbench에서 완성된 항목을 catalog/로 승격한다. 실험용 workbench 아이템을 재사용 가능한 catalog 출력으로 옮길 때 사용한다."
---

# 카탈로그 빌더

## 규칙

- `workbench/<name>/`에서 `catalog/<name>/`으로 승격한다.
- 승격하기 전에 어떤 파일이 최종 산출물인지 확인한다.
- `manifest.toml`과 최종 항목에 꼭 필요한 파일만 남긴다.
- 노트, 초안, 기타 중간 산출물은 제거한다.
- 승격이 끝나고 push까지 마친 뒤 원래 workbench 항목을 삭제한다.
