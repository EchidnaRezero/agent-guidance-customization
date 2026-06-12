# Risky Combinations

## Git State Changes

- `git add` + `git status`
  - Risk: `git status` may read before staging completes.
  - Safe order: run `git add`, then run `git status`.

- `git push` + `git status -sb`
  - Risk: `status` may report `ahead 1` before push finishes.
  - Safe order: run `git push`, then run `git status -sb`.

- `git config user.name ...` + `git config --get user.name`
  - Risk: the read may show the previous value.
  - Safe order: run the write, then run the read.

- `git config user.name ...` + `git config user.email ...`
  - Risk: both commands write `.git/config`, so one may fail with a config lock error.
  - Safe order: run the first config write, wait for it to finish, then run the second.

- `git push` + `git ls-remote --heads origin main`
  - Risk: remote verification may happen before the new commit is visible.
  - Safe order: run `git push`, then run `git ls-remote`.

## SSH and Authentication

- `ssh-add <key>` + `git push`
  - Risk: `git push` may start before the key is loaded or before the passphrase is entered.
  - Safe order: run `ssh-add`, confirm success, then run `git push`.

- `ssh -T git@github.com` + `git push`
  - Risk: auth verification and push compete for the same unresolved credential state.
  - Safe order: verify auth first, then push.

## File Edits

- `apply_patch` or formatter writes + test/lint on the same files
  - Risk: checks may read old or partially updated content.
  - Safe order: finish edits, then run checks.

- in-place generator + `git diff`
  - Risk: diff may run before generation completes.
  - Safe order: finish generation, then inspect diff.

## User Interruptions

- long-running remote command + status read in the same batch
  - Risk: if the user interrupts, one command may finish and the other may not.
  - Safe order: run the long command alone, then recheck state in a fresh step.
