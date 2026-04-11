---
name: using-linux-environment
description: Use when the current environment is Linux and command, path, or filesystem guidance should follow Linux conventions.
---

# Using Linux Environment

## Overview

Apply this when the work is happening on Linux and the guidance should use Linux shell, path, and filesystem conventions.

## Use This Skill For

- Linux shell commands
- Linux filesystem paths
- POSIX scripts or permissions
- Linux-side Codex skill installation or updates
- Linux-specific handling of worktree or temp directories

## Command and Path Conventions

- Use POSIX shell syntax.
- Prefer `$HOME` for user-home paths.
- Prefer `/tmp` for ephemeral data.
- Use `ln -s` for symlinks and `chmod +x` for executable scripts.
- Quote paths that may contain spaces.

## Superpowers Adaptation

- Codex clone path: `~/.codex/superpowers`
- Codex skills path: `~/.agents/skills/superpowers`
- Global worktree location: `~/.config/superpowers/worktrees/<project-name>`
- Shell-heavy skills such as `using-git-worktrees` and `brainstorming/visual-companion.md` may be interpreted with Linux conventions after this skill is loaded.

## Browser and Server Notes

- Bash scripts may run directly in the Linux shell.
- `--project-dir` keeps brainstorm files under the project instead of `/tmp`.
- If the browser is outside the Linux environment, bind a reachable host and set `--url-host` accordingly.

## Red Flags

- Do not mix Windows `C:\...` paths into Linux shell commands.
- Do not use `cmd` or PowerShell syntax here.
- If the environment is actually WSL, use `using-wsl-environment` instead.
