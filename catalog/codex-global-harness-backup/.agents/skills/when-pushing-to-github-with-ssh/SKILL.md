---
name: when-pushing-to-github-with-ssh
description: Use before GitHub SSH push or PR push; check ssh -T, ssh_host, repo-local identity, and git-accounts.toml.
---

# When Pushing To GitHub With SSH

## Rule

Do not guess when GitHub remotes, SSH keys, or account identity are involved. Do not suggest new GitHub key registration, remote rewrites, or SSH config edits before checking the existing account mapping and SSH setup.

On Windows, prefer checking and using Windows OpenSSH (`C:/Windows/System32/OpenSSH/ssh.exe`) over Git for Windows SSH (`C:/Program Files/Git/usr/bin/ssh.exe`) for GitHub SSH operations.

```text
Git for Windows `git.exe`
→ Windows OpenSSH through global `core.sshCommand`
→ Windows `ssh-agent`
→ stored key for the selected account
→ verify the account with `ssh -T`
→ `push`
```

## Required Checks

Before `git push` or any PR action that needs push:

```powershell
$accountsPath = Join-Path $env:USERPROFILE ".codex\git-accounts.toml"
git config --local --get-regexp "^user\."
git remote -v
ssh -G <ssh_host> | Select-String -Pattern "hostname|user|identityfile|identitiesonly"
ssh -T git@<ssh_host>
```

Compare these values:

- selected account in `git-accounts.toml`
- repo-local `user.name` and `user.email`
- remote host alias
- SSH config `IdentityFile`
- GitHub account reported by `ssh -T`

Push only when they describe the same intended account.

## If Authentication Fails

If `ssh-add` reports an agent connection failure, do not treat it as a key problem. Check the Windows `ssh-agent` service, start it if it is stopped, then check again.

If `ssh -T` fails, use the existing `IdentityFile` from SSH config. Do not create or register a new key unless the existing mapping and key file are proven wrong.

```powershell
ssh-add -l
ssh-add <IdentityFile>
ssh -T git@<ssh_host>
```

If `ssh-add` needs interactive unlock and the current runtime cannot provide it, ask the user to unlock the existing key in an interactive terminal. Do not relabel that as a missing GitHub key.

## Stop Conditions

- No selected Git account for a push.
- Repo-local identity does not match the selected account.
- Remote host does not match the selected account `ssh_host`.
- `ssh -T` authenticates as a different GitHub account.
- Suggesting key registration before checking `git-accounts.toml`, SSH config, and `ssh -T`.
