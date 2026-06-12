---
name: when-pushing-to-github-with-ssh
description: GitHub SSH push 또는 push가 필요한 PR 전, ssh -T, ssh_host, repo-local identity, git-accounts.toml을 확인할 때 사용합니다.
---

# When Pushing To GitHub With SSH

## 규칙

GitHub remote, SSH key, 계정 identity가 관련되면 추측하지 않습니다. 기존 계정 매핑과 SSH 설정을 확인하기 전에는 새 GitHub key 등록, remote 변경, SSH config 수정을 제안하지 않습니다.

## 필수 확인

`git push` 또는 push가 필요한 PR 작업 전에는 다음을 확인합니다.

```powershell
$accountsPath = Join-Path $env:USERPROFILE ".codex\git-accounts.toml"
git config --local --get-regexp "^user\."
git remote -v
ssh -G <ssh_host> | Select-String -Pattern "hostname|user|identityfile|identitiesonly"
ssh -T git@<ssh_host>
```

다음 값이 같은 의도된 계정을 가리키는지 대조합니다.

- `git-accounts.toml`에서 선택한 계정
- repo-local `user.name`과 `user.email`
- remote host alias
- SSH config `IdentityFile`
- `ssh -T`가 보고한 GitHub 계정

모두 같은 의도된 계정을 가리킬 때만 push합니다.

## 인증이 실패할 때

`ssh -T`가 실패하면 SSH config의 기존 `IdentityFile`을 사용합니다. 기존 매핑과 key file이 틀렸다는 것이 확인되기 전에는 새 key를 만들거나 등록하지 않습니다.

```powershell
ssh-add -l
ssh-add <IdentityFile>
ssh -T git@<ssh_host>
```

`ssh-add`에 interactive unlock이 필요하고 현재 runtime이 입력을 제공할 수 없으면, 사용자가 interactive terminal에서 기존 key를 unlock하도록 요청합니다. 이를 GitHub key 누락으로 다시 이름 붙이지 않습니다.

## 중단 조건

- push할 Git 계정이 선택되지 않음
- repo-local identity가 선택한 계정과 맞지 않음
- remote host가 선택한 계정의 `ssh_host`와 맞지 않음
- `ssh -T`가 다른 GitHub 계정으로 인증됨
- `git-accounts.toml`, SSH config, `ssh -T` 확인 전 key 등록 제안
