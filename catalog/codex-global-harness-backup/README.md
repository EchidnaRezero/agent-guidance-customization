# Codex Global Harness Backup

This package is a restorable backup of the user-authored Codex global setup.

It contains:

- the global policy file
- reusable global skills
- Git and GitHub SSH push workflow skills

## Target Layout

Use `%USERPROFILE%\.codex` as the canonical global root.

```text
%USERPROFILE%\.codex
|-- agents.md
|-- skills\
`-- git-accounts.toml
```

## Restore

1. Get the current repository checkout that contains this package.
   The repository name may change later. Use the current checkout that contains this item.
2. Restore `AGENTS.md` as `agents.md` at the target root.
   Keep `AGENTS_KR.md` beside it as the synchronized Korean companion.
3. Copy this package's `.agents/skills/` contents into `skills/`.
4. Keep your local Git account mapping file as `git-accounts.toml`.

## Notes

- This package is the backup source for the user-authored global harness.
- The former bundled `superpowers` workflow has been split into regular global skills under `.agents/skills/`.
