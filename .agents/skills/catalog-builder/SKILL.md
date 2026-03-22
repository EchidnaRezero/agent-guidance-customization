---
name: catalog-builder
description: "Promote one finished workbench item into catalog/. Use when Codex is asked to move an experimental AGENTS.md or skill from workbench/ into reusable catalog output."
---

# Catalog Builder

## Rules

- Promote from `workbench/<name>/` to `catalog/<name>/`.
- Before promotion, confirm which files are final output.
- Keep `AGENTS.md`, `SKILL.md`, `manifest.toml`, and only the files required by that final item.
- Remove notes, drafts, and other intermediate files.
- Delete the original workbench item after promotion.
