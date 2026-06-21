# Codex Global Harness Backup

This package is a restorable backup of the user-authored Codex global setup.

## Terminology

- Harness: a Codex-scoped harness package aligned with OpenAI's current Codex customization surfaces.
- Codex customization surfaces: the OpenAI-documented Codex surfaces used to shape or extend Codex behavior, including `AGENTS.md`, skills, custom agents, MCP, configuration, hooks, plugins, and related features.

Use the current OpenAI Codex documentation as the reference for supported Codex surfaces:

- [Customization](https://developers.openai.com/codex/concepts/customization)
- [Agent Skills](https://developers.openai.com/codex/skills)
- [Subagents](https://developers.openai.com/codex/subagents)

It contains:

- the global policy file
- reusable global skills
- Codex custom agent profiles
- Git and GitHub SSH push workflow skills

## Target Layout

Use `%USERPROFILE%\.codex` as the canonical global root.

```text
%USERPROFILE%\.codex
|-- AGENTS.md
|-- agents\
|-- skills\
`-- git-accounts.toml
```

## Restore

1. Get the current repository checkout that contains this package.
   The repository name may change later. Use the current checkout that contains this package.
2. Restore `AGENTS.md` at the target root.
   Keep `AGENTS_KR.md` beside it as the synchronized Korean companion.
3. Copy this package's `.agents/skills/` contents into `skills/`.
4. Copy this package's `.codex/agents/` contents into `agents/`.
5. Keep your local Git account mapping file as `git-accounts.toml`.

## Notes

- This package is the backup source for the user-authored global harness.
- The former bundled `superpowers` workflow has been split into regular global skills under `.agents/skills/`.
- Custom agent TOML files follow the Codex `agents/*.toml` custom agent format.
