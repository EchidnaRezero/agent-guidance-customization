# Git Account Selection

Use `%USERPROFILE%\.codex\git-accounts.toml` before the first commit or push in a repository.

Expected account shape:

```toml
default = "github-erika"

[accounts.github_erika]
label = "anonymous community and practice"
ssh_host = "github-erika"
user_name = "InoriNatsume"
user_email = "236425129+InoriNatsume@users.noreply.github.com"
```

Rules:

- If the user named an account, use that account.
- Otherwise show available account ids and labels, with the default first.
- Set repo-local identity before committing.
- Do not use global Git identity.
- If a push may happen, use `when-pushing-to-github-with-ssh` for the SSH host and account checks.

```powershell
git config --local user.name "<user_name>"
git config --local user.email "<user_email>"
git config --local --get user.name
git config --local --get user.email
```
