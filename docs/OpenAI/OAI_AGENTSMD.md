# OpenAI AGENTS.md

## OpenAI AGENTS.md structure

```text
home/
├── AGENTS.md : broad personal rules
└── repo-root/
    ├── AGENTS.md : rules for one GitHub repository
    ├── feature-a/
    │   ├── AGENTS.md : folder-specific rules
    │   └── child/
    │       └── AGENTS.override.md : stronger local override
    └── feature-b/
```

## How Codex reads it

- Codex reads the `AGENTS.md` chain before work starts.
- The chain runs from home-level rules to the project root and then down to the current folder.
- In the same folder, `AGENTS.override.md` wins over `AGENTS.md`.

## Practical rule

- Use `AGENTS.override.md` only when one folder needs a stronger local override.
- Keep `AGENTS.md` short enough to work as an instruction file.

## Limits

- The combined instruction size is normally limited to 32 KiB.
- If the chain grows too large, split rules into closer folders or adjust configuration.
- Codex can also use fallback project document names when `project_doc_fallback_filenames` is configured.

## Sources

- Codex AGENTS.md guide: <https://developers.openai.com/codex/guides/agents-md> — checked on 2026-03-21
