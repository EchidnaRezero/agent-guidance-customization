# Installing Superpowers for Codex

Enable the bundled superpowers skills in Codex via native skill discovery.

Home `agents.md` remains the highest-level policy. Superpowers is the default workflow layer underneath it.

Parent Codex root relative to this bundled item:

```text
../
|-- agents.md
|-- skills/
`-- superpowers/   # this bundled directory
```

## Prerequisites

- Git

## Installation

1. **Place this bundled directory at `superpowers/` under the parent Codex root.**

2. **From the bundled `superpowers/` directory, create the skills junction (Windows PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "..\skills"
   cmd /c mklink /J "..\skills\superpowers" ".\skills"
   ```

3. **Restart Codex** to discover the skills.

If you are preparing non-Windows guidance, do not reuse these commands directly. Load the matching OS environment skill in this bundled package first and rewrite the shell steps for that OS.

## Migrating from old bootstrap

If you installed superpowers before native skill discovery, you need to:

1. **Create the skills junction** (step 2 above) — this is the discovery mechanism.

2. **Remove the old bootstrap block** from `../agents.md` — any block referencing `superpowers-codex bootstrap` is no longer needed.
   The active policy file should be `../agents.md`.

3. **Restart Codex.**

## Verify

```powershell
Get-ChildItem "..\skills\superpowers"
```

You should see a symlink (or junction on Windows) pointing to the bundled superpowers skills directory.

## Updating

Replace the bundled `superpowers` directory with the newer packaged copy, then keep the same junction target.

## Uninstalling

```powershell
Remove-Item "..\skills\superpowers"
```

Optionally delete this `superpowers/` directory.
