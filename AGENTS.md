## Terminology
- Root harness: this repository's root-level harness, including this `AGENTS.md` and `.agents/skills/`.
- Item harness: one independent harness inside `workbench/<name>/` or `catalog/<name>/`, using that item's own `AGENTS.md` and local files as its boundary.
- Current global harness: the harness currently installed in the user's `%USERPROFILE%\.codex`.
- Potential global harness: a workbench or catalog item that is designed to replace the current global harness later, such as `workbench/<name>/` during editing or `catalog/codex-global-harness-backup/` after promotion.
- If the user names a file such as `AGENTS.md`, `README.md`, or a skill and the target harness is ambiguous, ask which harness they mean before editing or interpreting it.

## References
- When the user asks to create or update an `AGENTS.md` or a skill in this repository, first check `docs/`.
- Build experimental guidance under `workbench/<name>/` before promoting it elsewhere in this repository.
- If more detailed specs, official grounding, or newer information is needed for this repository's guidance work, check `official_references/` or verify with Context7 when available.

## Docs
- In this repository, write Markdown documents in English unless the user explicitly asks for Korean.
- When writing an English document under `docs/`, also create a Korean version with the same name plus `_kr`.
- The `_kr` file is the same document for Korean readers.
- If the same document exists without `_kr`, the agent does not need to read the `_kr` version unless asked.

## Workbench
- Each `workbench/<name>/` folder is one independent workbench item and should contain its own experimental `AGENTS.md` files and skills.
- Do not merge or reuse files across workbench items unless the user explicitly asks for it.
- Do not confuse root-level rules(rules for Root Documentation) and skills with workbench-level experimental output.
- When working at the repository root level, use skills from the repository-local `.agents/skills/` directory.
- When working inside `workbench/<name>/` (workbench level), use the skills inside that workbench folder first.

## Catalog
- Each `catalog/<name>/` folder is one independent catalog item.
- For a catalog item, treat the directory containing that item's `AGENTS.md` as that item's project root.
- Write all relative paths, file references, and layout references inside that item from that item root.
- Do not prefix item-internal paths with `catalog/` or any other parent path.
- If a path must point outside the item root, ask the user before writing it.
