# Codex Global Harness Backup

This package is a restorable backup of the user-authored Codex global setup.

It contains:

- the global policy file
- reusable global skills
- a bundled customized `superpowers` workflow

## Target Layout

Use `%USERPROFILE%\.codex` as the canonical global root.

```text
%USERPROFILE%\.codex
|-- agents.md
|-- skills\
|-- superpowers\
`-- git-accounts.toml
```

## Restore

1. Get the current repository checkout that contains this package.
   The repository name may change later. Use the current checkout that contains `catalog/codex-global-harness-backup/`.
2. Restore `AGENTS.md` as `agents.md` at the target root.
   Keep `AGENTS_KR.md` beside it as the synchronized Korean companion.
3. Copy this package's `.agents/skills/` contents into `skills/`.
4. Copy this package's bundled `superpowers/` directory into `superpowers/`.
5. Keep your local Git account mapping file as `git-accounts.toml`.
6. Follow `superpowers/.codex/INSTALL.md` to expose `superpowers/skills/` through Codex skill discovery.

## Notes

- This package is the backup source. Do not restore upstream `obra/superpowers` when your goal is to recover this customized setup.
- The bundled `superpowers` copy is adapted from `obra/superpowers`.
- `obra/superpowers` is distributed under the MIT License. See `superpowers/LICENSE`.
