---
name: catalog-builder
description: "Promote one finished workbench item into catalog/. Use when Codex is asked to move an experimental workbench item into reusable catalog output."
---

# Catalog Builder

## Rules

- Promote from `workbench/<name>/` to `catalog/<name>/`.
- Before promotion, confirm which files are final output.
- Create one `manifest.toml` at the promoted item root.
- On the first promotion in a conversation, retrieve the current agent, model, and provider, then ask the user to confirm that environment tuple before writing it.
- Reuse the confirmed environment tuple for later promotions in the same conversation. Confirm it again in each new conversation.
- Record the item version, the UTC calendar date at manifest creation or update time in `YYYY-MM-DD` format, and the confirmed environment tuple:

```toml
version = "1.0.0"
updated_on = "2026-07-19"
environment = { agent = "codex", model = "gpt-5.6-sol", provider = "openai" }
```

- Keep only the files required by the final item.
- Remove notes, drafts, and other intermediate files.
- Delete the original workbench item after promotion.
