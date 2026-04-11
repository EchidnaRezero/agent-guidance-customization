---
name: using-wsl-environment
description: Use when the current environment is WSL and command, path, or filesystem guidance should follow WSL conventions.
---

# Using WSL Environment

## Overview

Apply this before suggesting commands, paths, or filesystem behavior inside WSL. WSL is Linux-like, but it frequently interacts with Windows paths, browsers, and Git state.

## Use This Skill For

- WSL shell commands
- WSL path translation
- WSL-side Codex or project operations
- Browser/server workflows that cross between WSL and Windows

## Command and Path Conventions

- Use POSIX shell syntax inside WSL.
- Prefer Linux paths inside WSL (`$HOME`, `/tmp`, project-local paths).
- Only translate to Windows paths when the task explicitly needs Windows-side files or tools.
- Keep one command in one path style whenever possible.

## Superpowers Adaptation

- Treat shell-heavy Linux guidance as valid only after checking whether the task stays inside WSL.
- For brainstorm servers or local URLs, assume the browser may open on the Windows side even when the server runs inside WSL.
- If a service must be reachable across the WSL boundary, prefer an explicit host binding and `--url-host localhost`.

## Red Flags

- Do not mix `/mnt/c/...` and `C:\...` forms in the same command unless the conversion is intentional.
- Do not assume Windows `cmd` or PowerShell semantics while still inside WSL.
- If the task stays entirely on Linux without Windows interaction, `using-linux-environment` may be simpler.
