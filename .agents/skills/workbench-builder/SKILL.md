---
name: workbench-builder
description: "Create or update one experimental workbench item that contains harness files and settings. Use when Codex is asked to build a workbench item for another project."
---

# Workbench Builder

## Rules

- Work under `workbench/<name>/`.
- Keep experiment files out of the root.
- Treat `references/` as official grounding, not output.
- Keep skill descriptions short and trigger-focused when the item includes skills.
- Create `manifest.toml` for each workbench item.
- Record `name`, `vendor`, `product`, `model`, `version`, and `updated_on`.
- Mark the item as experimental until promoted to `catalog/`.
