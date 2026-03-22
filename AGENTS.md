## Language
- Always converse with the user in Korean.
- Write Markdown documents in English unless the user explicitly asks for Korean.

## References
- When the user asks to create or update an `AGENTS.md` or a skill, first check `docs/` for shared basics and short notes on company-specific `AGENTS.md` and skill structure differences, including company/model/version differences.
- Then create a working folder under `workbench/<name>/`.
- If more detailed specs, official grounding, or newer information is needed, check `official_references/` or verify with Context7 when available.

## Docs
- When writing an English document under `docs/`, also create a Korean version with the same name plus `_kr`.
- The `_kr` file is the same document for Korean readers.
- If the same document exists without `_kr`, the agent does not need to read the `_kr` version unless asked.

## Workbench
- Each `workbench/<name>/` folder is one independent workbench item and should contain its own experimental `AGENTS.md` files and skills.
- Do not merge or reuse files across workbench items unless the user explicitly asks for it.
- Do not confuse root-level rules(rules for Root Documentation) and skills with workbench-level experimental output.
- When working in `C:\Projects\skills` (root level), use skills from `C:\Projects\skills\.agents\skills`.
- When working inside `workbench/<name>/` (workbench level), use the skills inside that workbench folder first.

## Root Documentation
- These rules apply to project documentation outside `workbench/` and `catalog/`.
- Do not explain the document itself instead of its content.
- Do not describe before/after changes; write only the final result.
- Documentation minimization rule: do not write duplicate or near-duplicate content.
- If duplicate or overlapping content appears within a document or across documents, ask the user before merging, consolidating, or absorbing it.
- Do not restate what is already clear from structure, diagrams, or flowcharts.
- Write root `docs/` so that a middle-school student can read them easily.
- Use Mermaid only when it makes the explanation shorter or clearer.

## Environment
- Assume PowerShell on Windows first.
- Call out clearly when a command is WSL-only or Linux-only.
- As of March 2026, assume the default agent environment is the OpenAI Codex Windows app using GPT-5.4.
