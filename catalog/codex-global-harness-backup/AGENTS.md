## Policy Scope
- Treat this file as the highest-level global policy for the `.codex` environment.
- Repository-local `AGENTS.md` files may add or override rules for that repository.

## Terminology
- Global harness: the reusable `.codex`-level harness installed under `%USERPROFILE%\.codex`.
- Local harness: one repository's own harness files for that project.
- Harness: `AGENTS.md` and skill files, and `README.md` when that README is used as part of the project's harness.

## Language
- Always converse with the user in Korean.
- Write Markdown documents in Korean by default unless the user asks for another language.
- Write AI-facing instruction documents such as `AGENTS.md`, `SKILL.md`, and similar agent guidance in English unless the user asks for another language.
- Treat `AGENTS.md` as the canonical agent-facing policy file.
- Treat `AGENTS_KR.md` as the Korean companion version for users.
- When creating or updating one of them, create or update the other one as well and keep them synchronized.
- For skills, treat `SKILL.md` as the canonical agent-facing file.
- Treat files such as `SKILL_KR.md` as Korean companion versions for users.
- Do not read `_KR` skill companions unless the current task is to create, update, or synchronize them.
- When explaining technical content or asking the user for something, first explain the purpose and situation in a way a middle-school student can understand, then use technical terms.

## Environment
- Assume the default execution environment is Windows, not WSL or Linux.
- When suggesting commands that the user may run directly, prefer `cmd` command syntax over PowerShell unless PowerShell-specific behavior is required.
- If the current environment is not Windows, stop applying the default Windows environment guidance and load the matching OS-specific environment skill instead.
- Expected non-Windows environment skills:
  - `using-macos-environment`
  - `using-linux-environment`
  - `using-wsl-environment`

## Workflow
- By default, project work should follow the customized superpowers workflow unless the current repository defines a more specific local workflow.

## Security
- Determine the project's security level during planning, normally in `writing-plans`.
- Low: local personal use. Check input handling, path handling, secrets, logs, errors, permissions, and delete or overwrite behavior.
- Medium: external APIs, sensitive stored data, or shared device access. Check the low-level items and also review storage and access control.
- High: multiple users, public server exposure, or highly sensitive data. Design authentication, authorization, transport protection, and storage protection separately.
- If a security design looks heavier than the chosen level, explain why and get confirmation before proceeding.

## Git
- In `writing-plans`, decide whether the chosen workflow will create a commit or push.
- If Git output will be created, ask which Git account to use during planning.
- After the user chooses, check the local Git account mapping document such as `git-accounts.toml`.
- Before the first commit or push, set repo-local `user.name` and `user.email` to match that account.
- Do not use global Git identity.

## Code Design
- Give each file, class, and function one clear job.
- Use specific names that describe what the code does.
- Keep related logic together.
- Keep unrelated logic separate.
- Follow existing project patterns unless asked to change them.
- Avoid hardcoding values when a clearer and more maintainable option exists.
- If hardcoding is used for any reason, tell the user and propose a rewrite to remove it.
- Before using a library, check the latest version through Context7 when available. Adjust the version when needed for compatibility or stability.
- Prefer project-local environments such as Python `venv` instead of global installation.
- If code work for the same request has already failed four times or more, debug the cause first before making more code changes.
