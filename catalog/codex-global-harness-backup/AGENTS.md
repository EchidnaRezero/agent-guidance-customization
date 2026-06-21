## Policy Scope
- Treat this file as the highest-level global policy for the `.codex` environment.
- Repository-local `AGENTS.md` files may add or override rules for that repository.

## Language
- Always converse with the user in Korean.
- Write harness documents in English by default unless the user asks for another language.
- When creating or updating harness documents, use the `korean_companion_sync` custom agent to keep the canonical English document and `_KR` companion synchronized.

## Environment
- Assume the default execution environment is Windows, not WSL or Linux.
- When suggesting commands that the user may run directly, prefer `cmd` command syntax over PowerShell unless PowerShell-specific behavior is required.
- If the current environment is not Windows, stop applying the default Windows environment guidance and verify the appropriate environment guidance before continuing.

## Workflow
- Follow the most specific applicable repository or harness workflow.

## Security
- Determine the project's security level during planning, normally in `writing-plans`.
- Low: local personal use. Check input handling, path handling, secrets, logs, errors, permissions, and delete or overwrite behavior.
- Medium: external APIs, sensitive stored data, or shared device access. Check the low-level items and also review storage and access control.
- High: multiple users, public server exposure, or highly sensitive data. Design authentication, authorization, transport protection, and storage protection separately.
- If a security design looks heavier than the chosen level, explain why and get confirmation before proceeding.

## Git
- Use the `git_steward` custom agent for Git workflow checks, identity handling, commit and push readiness, and safe cleanup decisions.

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
