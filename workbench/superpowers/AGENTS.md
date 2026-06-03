This bundled superpowers copy is Codex-only.

- Treat this bundled `superpowers/` directory as its own project root for internal paths.
- Inside this bundled item, write relative paths from this root, such as `.codex/INSTALL.md`, `skills/`, `docs/`, `hooks/`, and `worktrees/`.
- The parent package `AGENTS.md` is the top policy layer above this bundled superpowers copy.
- Use `skills/using-superpowers/SKILL.md` as the entry workflow.
- If the current environment is not Windows, load the matching OS environment skill before following shell-heavy skills.
- Use `.codex/INSTALL.md` and `docs/README.codex.md` for Codex setup details.
- Non-Codex assets were removed from this bundled package. See `docs/non-codex-recovery-guide_kr.md` if you need to restore support for another agent.
