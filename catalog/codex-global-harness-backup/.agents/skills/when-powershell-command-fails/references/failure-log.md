# Failure Log

## Index

- [Case 002: A statement was piped without expression wrapping](#case-002-a-statement-was-piped-without-expression-wrapping)
- [Case 005: A native executable was unavailable on PATH](#case-005-a-native-executable-was-unavailable-on-path)
- [Case 008: A recursive text search encountered a live file lock](#case-008-a-recursive-text-search-encountered-a-live-file-lock)
- [Case 011: An unexpected quote terminated a PowerShell string](#case-011-an-unexpected-quote-terminated-a-powershell-string)
- [Case 013: A comma changed scalar path construction into an array argument](#case-013-a-comma-changed-scalar-path-construction-into-an-array-argument)
- [Case 014: An undefined cmdlet parameter caused binding to fail](#case-014-an-undefined-cmdlet-parameter-caused-binding-to-fail)
- [Case 017: LiteralPath prevented wildcard expansion](#case-017-literalpath-prevented-wildcard-expansion)
- [Case 018: The split count argument changed newline matching behavior](#case-018-the-split-count-argument-changed-newline-matching-behavior)
- [Case 021: A variable name collided with an automatic variable](#case-021-a-variable-name-collided-with-an-automatic-variable)
- [Case 024: A cmdlet call was not grouped before a boolean operator](#case-024-a-cmdlet-call-was-not-grouped-before-a-boolean-operator)
- [Case 032: Nested path pairs were flattened during array enumeration](#case-032-nested-path-pairs-were-flattened-during-array-enumeration)
- [Case 033: A colon after an interpolated variable caused a parser error](#case-033-a-colon-after-an-interpolated-variable-caused-a-parser-error)
- [Case 037: A doubled percent sign invalidated the modulo operator](#case-037-a-doubled-percent-sign-invalidated-the-modulo-operator)
- [Case 043: An unverified candidate path was used as input](#case-043-an-unverified-candidate-path-was-used-as-input)
- [Case 046: A relative path used the wrong working-directory base](#case-046-a-relative-path-used-the-wrong-working-directory-base)
- [Case 049: PowerShell passed a positional wildcard literally to a native command](#case-049-powershell-passed-a-positional-wildcard-literally-to-a-native-command)
- [Case 056: Child-process output encoding could not represent emitted Unicode](#case-056-child-process-output-encoding-could-not-represent-emitted-unicode)
- [Case 068: A compound command exposed only its final native exit status](#case-068-a-compound-command-exposed-only-its-final-native-exit-status)
- [Case 073: Filesystem state changed after earlier validation](#case-073-filesystem-state-changed-after-earlier-validation)
- [Case 107: An instance method was called on a null pipeline value](#case-107-an-instance-method-was-called-on-a-null-pipeline-value)
- [Case 109: A broad file filter violated an exact-one result check](#case-109-a-broad-file-filter-violated-an-exact-one-result-check)
- [Case 116: Nested cross-shell quoting changed child argument boundaries](#case-116-nested-cross-shell-quoting-changed-child-argument-boundaries)
- [Case 119: A native regex feature required an explicit engine](#case-119-a-native-regex-feature-required-an-explicit-engine)

## Case 002: A statement was piped without expression wrapping

### Failure type

PowerShell parser error

### Requested command

```powershell
foreach ($p in $projects) { [pscustomobject]@{ Project = $p } } | Format-Table
```

### User goal

Produce objects in a loop and format them as a table.

### What happened

Parsing stopped before the loop ran because the statement could not feed the pipeline in that position.

### Likely cause

`foreach (...) { ... }` was used as a statement where PowerShell required a pipeline-producing expression.

### Reproduction evidence

- PowerShell 7.5.8 Parser API returned `EmptyPipeElement`: "An empty pipe element is not allowed."

### Meaning

The command shape failed before any data or filesystem conclusion could be drawn.

### Short explanation example

```text
PowerShell rejected the direct pipeline from the foreach statement. Wrap the statement output before piping it.
```

### Safe next step

Use `@(foreach (...) { ... }) | ...` or `& { foreach (...) { ... } } | ...` and rerun the read-only operation.

## Case 005: A native executable was unavailable on PATH

### Failure type

Native-command discovery failure

### Requested command

```powershell
jar tf target\artifact.jar
```

### User goal

Inspect an existing artifact with a native utility.

### What happened

PowerShell could not resolve the executable, so the utility never opened the input.

### Likely cause

The program was absent or its installation directory was missing from the current process `PATH`.

### Reproduction evidence

- PowerShell 7.5.8 returned exit 1 with `CommandNotFoundException`; the missing executable was not resolved.

### Meaning

Executable lookup failed before the requested inspection, leaving the input's validity untested.

### Short explanation example

```text
PowerShell could not find the executable on PATH, so the inspection did not start.
```

### Safe next step

Confirm discovery with `Get-Command` or `where.exe`, then use a verified explicit executable path or correct `PATH`.

### Tool example

The same lookup principle applies when `jar` or `magick` cannot be resolved.

## Case 008: A recursive text search encountered a live file lock

### Failure type

Read failure caused by a locked file

### Requested command

```powershell
rg -n 'old-name' . -S
```

### User goal

Search a workspace recursively for stale references.

### What happened

The search traversed a runtime-owned file that another process held open.

### Likely cause

The search root included live state in addition to source and documentation files.

### Reproduction evidence

- PowerShell 7.5.8 with ripgrep 15.1.0 returned exit 2 and OS error 32 for the locked file while emitting the unlocked match.

### Meaning

A locked-file diagnostic makes the broad search incomplete; it does not invalidate matches already emitted.

### Short explanation example

```text
The recursive search reached a live lock file and could not read the full scope.
```

### Safe next step

Restrict the search to intended source roots or exclude runtime and lock-file directories.

### Tool example

An `rg` workspace search can encounter project lock files held by an active application.

## Case 011: An unexpected quote terminated a PowerShell string

### Failure type

PowerShell string-delimiter error

### Requested command

```powershell
rg -n "log \"example" script.sh
```

### User goal

Search a file for text containing double quotes.

### What happened

PowerShell treated an unexpected quote as a delimiter, ended the outer string, and parsed the remainder as command syntax.

### Likely cause

Backslash is not PowerShell's escape character.

### Reproduction evidence

- PowerShell 7.5.8 Parser API returned `TerminatorExpectedAtEndOfString`: the string was missing its `"` terminator.

### Meaning

The requested child command did not receive the intended pattern.

### Short explanation example

```text
An unexpected quote ended the PowerShell string before the intended pattern was complete.
```

### Safe next step

Use PowerShell's backtick or doubled-quote rules where appropriate, or move complex source into a here-string or script file; verify typographic punctuation before execution.

### Tool example

This commonly affects quoted `rg` patterns and inline Python source.

## Case 013: A comma changed scalar path construction into an array argument

### Failure type

PowerShell argument-binding error

### Requested command

```powershell
$roots = @(Join-Path $root 'one', Join-Path $root 'two')
```

### User goal

Build a collection of paths for later file operations.

### What happened

The comma became part of the first command's argument list, producing an array where a scalar child path was required.

### Likely cause

Command expressions inside the array were not individually grouped or placed on unambiguous lines.

### Reproduction evidence

- PowerShell 7.5.8 returned `CannotConvertArgument,Microsoft.PowerShell.Commands.JoinPathCommand`; `System.Object[]` could not bind to `AdditionalChildPath` as `System.String`.

### Meaning

Path collection construction failed before the dependent operation could be assessed.

### Short explanation example

```text
PowerShell bound the comma-separated values to one Join-Path call instead of creating two array elements.
```

### Safe next step

Group each command expression or assign each result separately before forming the array.

### Tool example

The same parsing boundary matters for `Join-Path`, multiple `-Filter` values, and comma-separated range expressions.

## Case 014: An undefined cmdlet parameter caused binding to fail

### Failure type

Undefined parameter binding

### Requested command

```powershell
New-Item -ItemType Directory -LiteralPath $destDir
```

### User goal

Create destination folders before moving files.

### What happened

Parameter binding failed because the active cmdlet did not expose the requested parameter.

### Likely cause

The active `New-Item` cmdlet has no `LiteralPath` parameter.

### Reproduction evidence

- PowerShell 7.5.8 returned `NamedParameterNotFound,Microsoft.PowerShell.Commands.NewItemCommand`; the destination was not created.

### Meaning

The directory-creation step did not run; later steps require independent verification.

### Short explanation example

```text
The active New-Item implementation does not accept that parameter, so creation stopped at binding.
```

### Safe next step

Inspect `Get-Command New-Item -Syntax` and use the supported `-Path` parameter.

## Case 017: LiteralPath prevented wildcard expansion

### Failure type

Literal-path and wildcard semantic mismatch

### Requested command

```powershell
Copy-Item -LiteralPath (Join-Path $src '*') -Destination $dst -Recurse
```

### User goal

Copy all children of one directory into another.

### What happened

PowerShell searched for a child literally named `*` instead of expanding the wildcard.

### Likely cause

`-LiteralPath` intentionally disables wildcard interpretation.

### Reproduction evidence

- PowerShell 7.5.8 returned `PathNotFound,Microsoft.PowerShell.Commands.CopyItemCommand`; the literal `*` path was not found and the file was not copied.

### Meaning

The requested set was never enumerated.

### Short explanation example

```text
LiteralPath treated the asterisk as a filename character, so no wildcard expansion occurred.
```

### Safe next step

Use `-Path` when wildcard expansion is intended, or enumerate children with `Get-ChildItem -LiteralPath` and copy exact paths.

## Case 018: The split count argument changed newline matching behavior

### Failure type

Operator-argument semantics mismatch

### Requested command

```powershell
$lines = $text -split "`r?`n", -1
```

### User goal

Split Markdown text into lines before transforming headings.

### What happened

The result did not have the expected line boundaries.

### Likely cause

The negative count selected a different `-split` mode instead of meaning unlimited splits.

### Reproduction evidence

- PowerShell 7.5.8 returned exit 0 with `Count=1`; the CRLF-containing value remained unsplit.

### Meaning

The transformation input was malformed, so write steps should remain paused.

### Short explanation example

```text
The negative split count changed operator behavior and prevented the expected newline split.
```

### Safe next step

Omit the count for ordinary regex line splitting and inspect the resulting array before writing.

## Case 021: A variable name collided with an automatic variable

### Failure type

Case-insensitive variable collision

### Requested command

```powershell
$matches = @()
$matches += $index
```

### User goal

Collect matching section indexes during a Markdown transformation.

### What happened

The assignment targeted PowerShell's automatic `$Matches` variable and destabilized later matching logic.

### Likely cause

PowerShell variable names are case-insensitive.

### Reproduction evidence

- PowerShell 7.5.8 changed the value from an object array containing `7` to the automatic `$Matches` hashtable after regex matching.

### Meaning

The script's collection and regex state shared one variable slot.

### Short explanation example

```text
$matches and the automatic $Matches variable are the same PowerShell variable.
```

### Safe next step

Rename the collection to a specific name such as `$matchedIndexes` and revalidate before writing.

## Case 024: A cmdlet call was not grouped before a boolean operator

### Failure type

Expression parsing error

### Requested command

```powershell
Where-Object { Test-Path -LiteralPath $_ -and (Split-Path $_ -Leaf) -notlike '_*NOTES.md' }
```

### User goal

Keep existing paths while excluding note files.

### What happened

PowerShell tried to bind `-and` as part of the `Test-Path` command instead of evaluating a Boolean expression.

### Likely cause

The cmdlet invocation was not enclosed in parentheses.

### Reproduction evidence

- PowerShell 7.5.8 returned `NamedParameterNotFound,Microsoft.PowerShell.Commands.WhereObjectCommand`: no parameter matched `and`.

### Meaning

Filtering stopped before producing a trustworthy candidate set.

### Short explanation example

```text
Group the Test-Path call so PowerShell can evaluate its result with -and.
```

### Safe next step

Use `(Test-Path -LiteralPath $_) -and (...)` and inspect the filtered paths before edits.

## Case 032: Nested path pairs were flattened during array enumeration

### Failure type

PowerShell collection-shape error

### Requested command

```powershell
$pairs = @(@($path1, $path2))
foreach ($pair in $pairs) { [IO.File]::ReadAllText($pair[0]) }
```

### User goal

Read each intended pair of file paths during verification.

### What happened

Enumeration removed the intended nesting, so indexing addressed string characters or single paths instead of pairs.

### Likely cause

PowerShell automatically unrolled nested arrays at a pipeline or assignment boundary.

### Reproduction evidence

- PowerShell 7.5.8 produced `PairCount=2` with a `System.String` first item, then `ReadAllText` failed with `FileNotFoundException` after indexing the first character.

### Meaning

The verification used the wrong data shape and its reads are not trustworthy.

### Short explanation example

```text
PowerShell flattened the nested arrays, so each loop item was not the expected two-path pair.
```

### Safe next step

Use objects with named path properties or unary-comma wrapping, then print the shape before reading files.

## Case 033: A colon after an interpolated variable caused a parser error

### Failure type

Expandable-string parser ambiguity

### Requested command

```powershell
throw "Broken link in $pre: $relative"
```

### User goal

Emit a clear validation error containing a variable value followed by a colon.

### What happened

PowerShell interpreted the colon as part of a scoped variable reference and stopped parsing.

### Likely cause

The variable boundary was not delimited inside the expandable string.

### Reproduction evidence

- PowerShell 7.5.8 Parser API returned `InvalidVariableReferenceWithDrive` for the `$pre:` extent.

### Meaning

Validation failed while formatting its diagnostic rather than while evaluating the target condition.

### Short explanation example

```text
The colon made the variable reference ambiguous inside the double-quoted string.
```

### Safe next step

Use `${pre}:`, `$($pre):`, or the format operator to make the boundary explicit.

## Case 037: A doubled percent sign invalidated the modulo operator

### Failure type

PowerShell operator syntax error

### Requested command

```powershell
$count %% 2 -eq 0
```

### User goal

Confirm that a Markdown code-fence count is even.

### What happened

PowerShell rejected the doubled percent token before evaluating the check.

### Likely cause

Escaping rules from another formatting or shell context were carried into PowerShell source.

### Reproduction evidence

- PowerShell 7.5.8 Parser API returned `ExpectedValueExpression`: a value expression was required after `%`.

### Meaning

Fence parity remains unverified.

### Short explanation example

```text
PowerShell uses a single percent sign for modulo; the doubled token could not be evaluated.
```

### Safe next step

Use `$count % 2 -eq 0` and report the count alongside the Boolean result.

## Case 043: An unverified candidate path was used as input

### Failure type

Candidate-path validation failure

### Requested command

```powershell
Get-Content -LiteralPath '<guessed-path>'
```

### User goal

Read likely documentation or source files during inspection.

### What happened

The reader received a guessed path that did not exist in the current tree.

### Likely cause

A filename, extension, directory level, or singular/plural spelling was inferred before discovery.

### Reproduction evidence

- PowerShell 7.5.8 `Get-Content` returned exit 1 with `ItemNotFoundException`; the guessed path was never opened.

### Meaning

Only that candidate read failed; the target's actual location remains unknown.

### Short explanation example

```text
The read used an unverified candidate path, so discover the actual file before drawing a content conclusion.
```

### Safe next step

Enumerate candidates, validate the selected literal path, then read it in a separate command.

### Tool example

Use `rg --files` or `Test-Path` before reading; this also catches placeholder text such as `Import-Csv -Path doesnotexist` before it reaches the reader.

## Case 046: A relative path used the wrong working-directory base

### Failure type

Working-directory path mismatch

### Requested command

```powershell
Get-Item -LiteralPath $relativePath
```

### User goal

Inspect metadata or content for files named relative to a project root.

### What happened

PowerShell resolved the relative input against a different current directory.

### Likely cause

The command assumed an implicit base directory that the runner had not selected.

### Reproduction evidence

- PowerShell 7.5.8 `Get-Item` returned exit 1 with `ItemNotFoundException`; the relative lookup used the wrong base while the fixture remained under the intended base.

### Meaning

The failed lookup says nothing about the same relative path under the intended base.

### Short explanation example

```text
The relative path was resolved from the wrong working directory.
```

### Safe next step

Set and print the working directory or construct a verified absolute path before the read.

### Tool example

Relative `Get-Item` and `Select-String` inputs commonly expose a wrong runner working directory.

## Case 049: PowerShell passed a positional wildcard literally to a native command

### Failure type

Native-command wildcard mismatch

### Requested command

```powershell
rg -n 'pattern' requirements*.txt
```

### User goal

Search every matching file with a native text-search command.

### What happened

The native program received the wildcard text as a positional path and reported it missing.

### Likely cause

PowerShell did not expand the positional wildcard for that native invocation.

### Reproduction evidence

- PowerShell 7.5.8 with ripgrep 15.1.0 returned exit 2 and OS error 123 after receiving `requirements*.txt` literally.

### Meaning

Valid explicit inputs may still have produced results, while the wildcard-selected scope remains incomplete.

### Short explanation example

```text
PowerShell passed the wildcard literally, so the native command could not enumerate the intended files.
```

### Safe next step

Use the native program's glob option or enumerate exact paths in PowerShell before invocation.

### Tool example

For `rg`, prefer `-g 'requirements*.txt'` over a positional Windows wildcard.

## Case 056: Child-process output encoding could not represent emitted Unicode

### Failure type

Console encoding failure

### Requested command

```powershell
@'
# script that prints extracted text
'@ | python -
```

### User goal

Print extracted text from a saved document for read-only analysis.

### What happened

The child process encountered a Unicode character that its active output encoding could not encode.

### Likely cause

The Windows console or redirected stream used a legacy code page with a smaller character repertoire.

### Reproduction evidence

- PowerShell 7.5.8 with Python 3.13.5 returned exit 1 and `UnicodeEncodeError`; CP949 could not encode U+200B after emitting the prefix.

### Meaning

Extraction may have progressed before output serialization failed; the unseen remainder is unverified.

### Short explanation example

```text
The child produced Unicode that the active output encoding could not represent, so output stopped early.
```

### Safe next step

Select UTF-8 explicitly for the child output channel and rerun the read-only extraction.

### Tool example

Python output redirected through PowerShell can fail on zero-width or other Unicode characters under `cp949`.

## Case 068: A compound command exposed only its final native exit status

### Failure type

Compound-command exit-code masking

### Requested command

```powershell
npm run lint; npm run test; npm run build
```

### User goal

Run several independent validation stages in one pass.

### What happened

An earlier native stage failed, while a later stage overwrote `$LASTEXITCODE` with its own result.

### Likely cause

The compound invocation did not capture or enforce each native exit status immediately.

### Reproduction evidence

- PowerShell 7.5.8 with `cmd.exe` observed `first=7 final=0`; the compound process exited 0 after the later command replaced `$LASTEXITCODE`.

### Meaning

The final exit code describes only the last native command and cannot certify earlier stages.

### Short explanation example

```text
A later command replaced the earlier failure code, so the compound command's final status is insufficient.
```

### Safe next step

Run stages separately or capture `$LASTEXITCODE` after every native invocation and stop or summarize explicitly.

### Tool example

An `npm` failure can be hidden by a later successful stage.
An earlier success can be hidden by an expected empty `Get-Process` or `Get-Command` probe.

## Case 073: Filesystem state changed after earlier validation

### Failure type

Time-of-check/time-of-use mismatch

### Requested command

```powershell
Get-Item -LiteralPath <previously-validated-file>
```

### User goal

Open a file that an earlier step had created or verified.

### What happened

The later read could not find the path that earlier evidence showed as present.

### Likely cause

The file was deleted between validation and use.

### Reproduction evidence

- PowerShell 7.5.8 observed `validated=True after_delete=False`; the later `Get-Item` returned exit 1 with `ItemNotFoundException`.

### Meaning

Earlier evidence remains historical; the current operation must use current filesystem state.

### Short explanation example

```text
The path existed at the earlier check but was absent when the later read ran.
```

### Safe next step

Revalidate the exact path and working directory immediately before use, then identify any intervening state change.

## Case 107: An instance method was called on a null pipeline value

### Failure type

Null-value method invocation

### Requested command

```powershell
$_.Value.GetType()
```

### User goal

Inspect the runtime types of values loaded from structured data.

### What happened

One pipeline object's `Value` was null, so the instance method call emitted an error.

### Likely cause

The input schema permitted null while the inspection assumed every value was an object.

### Reproduction evidence

- PowerShell 7.5.8 returned `InvokeMethodOnNull,Microsoft.PowerShell.Commands.ForEachObjectCommand`: a method was called on a null-valued expression.

### Meaning

Other pipeline items may still have been inspected; the null item's type remains intentionally absent.

### Short explanation example

```text
The pipeline value was null, so it had no instance on which GetType could run.
```

### Safe next step

Test for `$null` first and emit a distinct null marker before invoking instance methods.

## Case 109: A broad file filter violated an exact-one result check

### Failure type

Discovery cardinality mismatch

### Requested command

```powershell
$files = @(Get-ChildItem -File -Filter '*inputs*')
if ($files.Count -ne 1) { throw 'Expected exactly one inputs file' }
```

### User goal

Discover and read one intended include file.

### What happened

The broad filter matched the intended file plus a companion metadata file.

### Likely cause

The discovery predicate did not constrain the expected extension or file role.

### Reproduction evidence

- PowerShell 7.5.8 `Get-ChildItem` found two candidates and returned exit 1 with `RuntimeException` when the exact-one guard rejected them.

### Meaning

The exact-one guard worked correctly and prevented an ambiguous selection.

### Short explanation example

```text
The filter returned more than one candidate, so the exact-one check rejected the ambiguous set.
```

### Safe next step

Refine discovery by extension and naming rule, print all candidates, then enforce cardinality again.

### Tool example

A Unity asset and its `.meta` companion can both match a broad `*inputs*` filter.

## Case 116: Nested cross-shell quoting changed child argument boundaries

### Failure type

Cross-shell quoting and argument-boundary failure

### Requested command

```powershell
powershell -Command "<native command with nested quotes>"
```

### User goal

Run a dependency audit or validation through multiple shell layers.

### What happened

An outer shell consumed quotes intended for a child, splitting or reshaping its arguments.

### Likely cause

Inline source crossed PowerShell, native, and child-shell parsers with incompatible escape rules.

### Reproduction evidence

- PowerShell 7.5.8 returned exit 1 with `NativeCommandError`; the child `Get-Content` treated `with` as an unexpected positional argument.

### Meaning

Child diagnostics and final exit codes describe malformed fragments rather than the intended validation.

### Short explanation example

```text
Nested shell parsing changed the child's argument boundaries before the intended command ran.
```

### Safe next step

Remove unnecessary shell layers and pass complex source through a script file, stdin, or a single verified argument array.

### Tool example

Nested WSL Windows-path conversion can reshape the path for the same argument-boundary reason; split cross-shell stages and pass verified arguments.

## Case 119: A native regex feature required an explicit engine

### Failure type

Native-command regex-engine mismatch

### Requested command

```powershell
rg -n --glob '*.md' '(?<![A-Za-z0-9_.-])(alpha|beta)/' <search-root>
```

### User goal

Find Markdown path references that start with selected folder names outside a larger path token.

### What happened

The native command rejected the lookbehind expression before searching the requested files.

### Likely cause

The selected regex engine did not support the pattern's negative lookbehind construct.

### Reproduction evidence

- ripgrep 15.1.0 returned exit 2 with `regex parse error`: look-around, including look-ahead and look-behind, is not supported; the diagnostic suggested `--pcre2`.

### Meaning

The search produced no trustworthy match result, while the inspected files remained unchanged.

### Short explanation example

```text
The selected regex engine could not parse the lookbehind expression, so the search did not start.
```

### Safe next step

Enable PCRE2 explicitly with `--pcre2`, or replace the lookbehind with the compatible prefix pattern `(^|[^A-Za-z0-9_.-])` when including that prefix in each match is acceptable.

### Tool example

Use `rg --pcre2 ... '(?<![A-Za-z0-9_.-])...'` or `rg ... '(^|[^A-Za-z0-9_.-])...'`.
