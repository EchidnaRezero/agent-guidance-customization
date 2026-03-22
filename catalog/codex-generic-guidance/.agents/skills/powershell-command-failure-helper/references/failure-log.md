# Failure Log

## Case 001: Remove-Item was blocked before execution

### Failure type

Blocked before execution by runtime policy

### Requested command

```powershell
Remove-Item docs\format\original -Recurse -Force
```

### User goal

Delete the leftover `docs\format\original` folder after other file changes completed.

### What happened

- File copy or content edits succeeded.
- Folder deletion did not run.
- The command was rejected as `blocked by policy` in the Codex desktop runtime layer.

### Likely cause

- The delete command was blocked before execution by the current runtime policy layer.

### Meaning

- This does not automatically mean the repository forbids the change.
- This does not mean the edited files are broken.
- It means cleanup could not be completed in this session with that command.

### Short explanation example

```text
The requested content changes completed, but the cleanup command did not.
`Remove-Item docs\format\original -Recurse -Force` was blocked before execution by the current Codex runtime policy layer.
This looks like a session/tool restriction rather than a code or document error.
The remaining issue is only the undeleted folder.
```

### Safe next step

- Leave the folder in place and report it clearly.
- Retry only with a command that is allowed in the current runtime.
- Do not describe the blocked delete as a code failure.
