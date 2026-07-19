# Misconception Log

## Index

- [Case 031: An expected negative native-command status was misclassified as failure](#case-031-an-expected-negative-native-command-status-was-misclassified-as-failure)

## Case 031: An expected negative native-command status was misclassified as failure

### Outcome type

Expected negative native-command status

### Requested command

```powershell
& <native-command> <arguments>
$status = $LASTEXITCODE
```

### User goal

Determine whether a searched value, optional dependency, or other queried item is absent.

### What happened

- The native command returned its documented status for an empty or absent result.
- The runner treated every nonzero exit code as an execution failure.

### Likely cause

- The PowerShell wrapper did not translate the command's result-specific exit codes before reporting status.

### Reproduction evidence

- PowerShell 7.5.8 with ripgrep 15.1.0 returned exit 1 with zero-length stdout and stderr for an absent fixed-string pattern.

### Meaning

- The negative result can be valid evidence that the queried item is absent.
- Other nonzero statuses still require diagnosis according to the command's exit-code contract.

### Short explanation example

```text
The check completed and returned its documented status for an expected negative result.
```

### Safe next step

- Capture `$LASTEXITCODE`, map the expected negative status to a meaningful result, and preserve unexpected statuses as errors.

### Tool example

Translate exit `1` from an `rg` no-match or an `npm ls` expected absence into a meaningful negative result.
