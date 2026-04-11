# Superpowers for Codex

Guide for using Superpowers with OpenAI Codex via native skill discovery.

This bundled copy is adapted from `obra/superpowers`, which is distributed under the MIT License.

Home `agents.md` remains the highest-level policy. Superpowers is the default workflow layer underneath it.

Parent Codex root relative to this bundled item:

```text
../
|-- agents.md
|-- skills/
`-- superpowers/   # this bundled directory
```

## Quick Install

Use the bundled install guide at `.codex/INSTALL.md`.

## Manual Installation

### Prerequisites

- OpenAI Codex
- Git

### Windows Steps

1. Place this bundled directory at `superpowers/` under the parent Codex root.

2. Create the skills junction:
   ```powershell
   New-Item -ItemType Directory -Force -Path "..\skills"
   cmd /c mklink /J "..\skills\superpowers" ".\skills"
   ```

3. Restart Codex.

4. **For subagent skills** (optional): Skills like `dispatching-parallel-agents` and `subagent-driven-development` require Codex's multi-agent feature. Add to your Codex config:
   ```toml
   [features]
   multi_agent = true
   ```

### Non-Windows Adaptation

If you are rewriting this workflow for macOS, Linux, or WSL, load the matching OS environment skill in this bundled package before changing the shell commands or path rules.

## How It Works

Codex has native skill discovery through `../skills/`. On Windows, Superpowers skills are made visible through a single junction:

```
..\skills\superpowers -> .\skills
```

The `using-superpowers` skill is discovered automatically and enforces skill usage discipline — no additional configuration needed.

## Usage

Skills are discovered automatically. Codex activates them when:
- You mention a skill by name (e.g., "use brainstorming")
- The task matches a skill's description
- The `using-superpowers` skill directs Codex to use one

### Personal Skills

Create your own skills in `../skills/`:

```powershell
New-Item -ItemType Directory -Force -Path "..\skills\my-skill"
```

Create `../skills/my-skill/SKILL.md`:

```markdown
---
name: my-skill
description: Use when [condition] - [what it does]
---

# My Skill

[Your skill content here]
```

The `description` field is how Codex decides when to activate a skill automatically — write it as a clear trigger condition.

## Updating

Replace the bundled `superpowers` directory with a newer packaged copy, then keep the same junction target.

## Uninstalling

```powershell
Remove-Item "..\skills\superpowers"
```

Optionally delete this `superpowers/` directory.

## Troubleshooting

### Skills not showing up

1. Verify the junction: `Get-ChildItem "..\skills\superpowers"`
2. Check the packaged skills path you linked to exists
3. Restart Codex — skills are discovered at startup

### Windows junction issues

Junctions normally work without special permissions. If creation fails, try running PowerShell as administrator.

## Getting Help

- Report issues: https://github.com/obra/superpowers/issues
- Main documentation: https://github.com/obra/superpowers
