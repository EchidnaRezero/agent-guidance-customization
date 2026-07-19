## Workflow
- If the user names a file such as `AGENTS.md`, `README.md`, or a skill and the target harness is ambiguous, ask which harness they mean before editing or interpreting it.
- Build experimental guidance under `workbench/<name>/` before promoting it elsewhere in this repository.
- Treat an item as approved for catalog promotion when the user explicitly requests promotion or requests a push whose scope includes added or updated files under `catalog/<name>/`.
- Apply `catalog-builder` to each item approved for promotion.
- If more detailed specs, official grounding, or newer information is needed for this repository's guidance work, verify with the relevant official source or Context7 when available.

## Docs
- In this repository, write Markdown documents in English unless the user explicitly asks for Korean.

## Workbench
- Each `workbench/<name>/` folder is one independent workbench item and should contain its own experimental harness files.
- Do not merge or reuse files across workbench items unless the user explicitly asks for it.
- Do not confuse root-level rules(rules for Root Documentation) and skills with workbench-level experimental output.
- Do not write repository-specific terms such as "item", "workbench item", or "catalog item" inside documents that belong to an individual workbench or catalog folder. Those documents may later be used outside this repository, so describe their own harness, files, and restore behavior without relying on this repository's classification terms.
- When working at the repository root level, use skills from the repository-local `.agents/skills/` directory.
- When working inside `workbench/<name>/` (workbench level), use the skills inside that workbench folder first.

## Catalog
- Each `catalog/<name>/` folder is one independent catalog item.
- For a catalog item, treat the directory containing that item's `AGENTS.md` as that item's project root.
- Write all relative paths, file references, and layout references inside that item from that item root.
- Do not prefix item-internal paths with `catalog/` or any other parent path.
- If a path must point outside the item root, ask the user before writing it.
