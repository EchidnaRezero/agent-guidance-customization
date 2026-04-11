---
name: using-macos-environment
description: Use when the current environment is macOS and command, path, or filesystem guidance should follow macOS conventions.
---

# Using macOS Environment

## Overview

Apply this when the work is happening on macOS and the guidance should use macOS shell, path, and filesystem conventions.

## Use This Skill For

- macOS shell commands
- macOS filesystem paths
- POSIX scripts or permissions
- macOS-side Codex skill installation or updates
- macOS-specific handling of worktree or temp directories

## Command and Path Conventions

- Use POSIX shell syntax.
- Prefer `$HOME` for user-home paths.
- Typical user paths live under `/Users/<name>/...`.
- Prefer `/tmp` for ephemeral data.
- Use `ln -s` for symlinks and `chmod +x` for executable scripts.
- Quote paths that may contain spaces.

## Superpowers Adaptation

- Codex clone path: `~/.codex/superpowers`
- Codex skills path: `~/.agents/skills/superpowers`
- Global worktree location: `~/.config/superpowers/worktrees/<project-name>`
- Shell-heavy skills such as `using-git-worktrees` and `brainstorming/visual-companion.md` may be interpreted with macOS conventions after this skill is loaded.

## macOS Notes

- Use `open <url-or-path>` when macOS needs to open a browser URL or local file.
- Expect filesystem watcher differences on macOS when reasoning about `fs.watch` behavior.
- Use Homebrew package names when a skill needs a macOS package-manager example.

## Red Flags

- Do not mix Windows `C:\...` paths into macOS shell commands.
- Do not use `cmd` or PowerShell syntax here.
- If the environment is actually Linux or WSL, use the matching skill instead.
