---
name: workbench-builder
description: "Create or update one experimental AGENTS.md or skill under workbench/. Use when Codex is asked to build a workbench item for another project."
---

# Workbench Builder

## Rules

- Work under `workbench/<name>/`.
- Keep experiment files out of the root.
- Treat `references/` as official grounding, not output.
- Keep skill descriptions short and trigger-focused.
- Create `manifest.toml` for each workbench item.
- Record `name`, `vendor`, `product`, `model`, `version`, and `updated_on`.
- Mark the item as experimental until promoted to `catalog/`.
