---
name: item-internal-path-check
description: "Check whether paths written inside one workbench or catalog item are expressed relative to that item's root instead of using parent paths such as `workbench/` or `catalog/`."
---

# item-internal-path-check

## Writing Criteria

- In one `workbench/<name>/` item, treat that `workbench/<name>/` folder as the item root.
- In one `catalog/<name>/` item, treat the directory containing that item's `AGENTS.md` as the item root.
- Write item-internal paths relative to that item root.
- Do not prefix item-internal paths with `workbench/`, `catalog/`, or any repository-root segment.
- If a subdirectory inside the item has its own `AGENTS.md`, treat that subdirectory as its own local root for paths written inside it.
- If a path must point outside the item root, ask the user before writing it.

## Review Criteria

- Check whether every path in the current item follows the writing criteria above.
- Rewrite item-internal paths into item-root-relative form when needed.
