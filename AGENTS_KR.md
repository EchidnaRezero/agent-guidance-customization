## Terminology
- 루트 하네스: 이 저장소의 루트 수준 하네스이며, 이 `AGENTS.md`와 `.agents/skills/`를 포함합니다.
- 아이템 하네스: `workbench/<name>/` 또는 `catalog/<name>/` 안에 있는 하나의 독립된 하네스이며, 그 아이템의 `AGENTS.md`와 로컬 파일을 경계로 봅니다.
- 현재 전역 하네스: 사용자의 `%USERPROFILE%\.codex`에 현재 설치되어 있는 하네스입니다.
- 잠재적 전역 하네스: 나중에 현재 전역 하네스를 대체하도록 설계된 workbench 또는 catalog 아이템입니다. 편집 중에는 `workbench/<name>/`에 있을 수 있고, 승격 후에는 `catalog/codex-global-harness-backup/` 같은 형태가 될 수 있습니다.
- 사용자가 `AGENTS.md`, `README.md`, 스킬 같은 파일을 지칭했는데 어느 하네스를 말하는지 애매하거나 혼선의 여지가 있으면, 수정하거나 해석하기 전에 어느 하네스를 말하는지 먼저 확인합니다.

## References
- 이 저장소에서 `AGENTS.md`나 스킬의 생성 또는 수정을 요청받으면 먼저 `docs/`를 확인합니다.
- 이 저장소 안의 실험용 guidance는 먼저 `workbench/<name>/` 아래에서 만듭니다.
- 이 저장소의 guidance 작업에 더 자세한 사양, 공식 근거, 최신 정보가 필요하면 `official_references/`를 확인하거나 가능할 때 Context7로 검증합니다.

## Docs
- 이 저장소에서는 사용자가 명시적으로 한국어를 요청하지 않는 한 Markdown 문서를 영어로 작성합니다.
- `docs/` 아래에 영어 문서를 쓸 때는 같은 이름에 `_kr`를 붙인 한국어 버전도 같이 만듭니다.
- `_kr` 파일은 한국어 독자를 위한 같은 문서입니다.
- 같은 문서가 `_kr` 없이 존재하면, 요청받지 않은 한 에이전트는 `_kr` 버전을 읽지 않아도 됩니다.

## Workbench
- 각 `workbench/<name>/` 폴더는 하나의 독립된 workbench 아이템이며, 그 안에 자체적인 실험용 `AGENTS.md`와 스킬을 포함해야 합니다.
- 사용자가 명시적으로 요청하지 않은 한 workbench 아이템 사이에서 파일을 합치거나 재사용하지 않습니다.
- 루트 수준 규칙(Root Documentation용 규칙)과 스킬을 workbench 수준의 실험 결과와 혼동하지 않습니다.
- 저장소 루트 수준에서 작업할 때는 저장소 로컬 `.agents/skills/` 디렉터리의 스킬을 사용합니다.
- `workbench/<name>/` 안에서 작업할 때는 그 workbench 폴더 안의 스킬을 먼저 사용합니다.

## Catalog
- 각 `catalog/<name>/` 폴더는 하나의 독립된 catalog 아이템입니다.
- catalog 아이템에서는 그 아이템의 `AGENTS.md`가 있는 디렉터리를 그 아이템의 프로젝트 루트로 취급합니다.
- 그 아이템 안의 모든 상대경로, 파일 참조, 레이아웃 참조는 그 아이템 루트를 기준으로 씁니다.
- 아이템 내부 경로에 `catalog/`나 다른 상위 경로를 붙이지 않습니다.
- 경로가 아이템 루트 밖을 가리켜야 한다면, 문서에 쓰기 전에 사용자에게 확인합니다.
