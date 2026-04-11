# Installing Superpowers for Codex

Enable superpowers skills in Codex via native skill discovery. Just clone and symlink.

Home `agents.md` remains the highest-level policy. Superpowers is the default workflow layer underneath it.

## Prerequisites

- Git

## Installation

1. **Clone the superpowers repository (Windows PowerShell):**
   ```powershell
   git clone https://github.com/obra/superpowers.git "$env:USERPROFILE\.codex\superpowers"
   ```

2. **Create the skills junction (Windows PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
   cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers" "$env:USERPROFILE\.codex\superpowers\skills"
   ```

3. **Restart Codex** to discover the skills.

If you are preparing non-Windows guidance, do not reuse these commands directly. Load the matching OS environment skill in this workbench first and rewrite the shell steps for that OS.

## Migrating from old bootstrap

If you installed superpowers before native skill discovery, you need to:

1. **Update the repo:**
   ```bash
   cd ~/.codex/superpowers && git pull
   ```

2. **Create the skills symlink** (step 2 above) — this is the new discovery mechanism.

3. **Remove the old bootstrap block** from `~/.codex/AGENTS.md` — any block referencing `superpowers-codex bootstrap` is no longer needed.

4. **Restart Codex.**

## Verify

```powershell
Get-ChildItem "$env:USERPROFILE\.agents\skills\superpowers"
```

You should see a symlink (or junction on Windows) pointing to your superpowers skills directory.

## Updating

```powershell
Set-Location "$env:USERPROFILE\.codex\superpowers"
git pull
```

Skills update instantly through the symlink.

## Uninstalling

```powershell
Remove-Item "$env:USERPROFILE\.agents\skills\superpowers"
```

Optionally delete the clone: `Remove-Item -Recurse -Force "$env:USERPROFILE\.codex\superpowers"`.
