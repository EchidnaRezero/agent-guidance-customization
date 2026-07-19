## Policy Scope
- Treat this file as the highest-level global policy for the `.codex` environment.
- Repository-local `AGENTS.md` files may add or override rules for that repository.

## Language
- Always converse with the user in Korean.
- For documents for AI (`AGENTS.md`, `SKILL.md`, or harness documents), write in English by default unless the user asks for another language. For user-facing documents, write in Korean by default.
- When creating or updating harness documents, use the `korean_companion_sync` custom agent to keep the canonical English document and `_KR` companion synchronized.
- When talking with a user, describe actions and results in terms the user can directly observe or verify before using technical jargon.

## Writing Docs
- Use contrastive negation such as “not A but B” only when rejecting A is necessary because it is a likely misconception, a safety concern, or the core contrast. Otherwise, state the intended idea directly.
- Follow a predefined template so the content is structured and cohesive.
- Avoid meta-expressions that describe the document instead of its content.
- Do not mention, restate, or describe the user's chat instructions in the document. They are behavior rules for the agent, not document content.

## Answer Style
- Put the conclusion or summary first.
- Cover one topic in about one paragraph. If a longer answer is unavoidable, keep it focused and end with one brief preview of what comes next.

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
- Use read-only Git commands directly only when file history or existing changes are necessary evidence for the current request.
- Do not perform Git checks as a default preflight for unrelated work.
- Use the `git_steward` custom agent for Git workflow decisions and state-changing Git operations, including staging, commit, push, pull, merge, branch or worktree changes, discard or cleanup, identity selection, and SSH account checks.

## Plan And Brainstorming
- Identify the user's desired final outcome and define milestones with results the user can predict and verify.
- Use simple visualization tools when they materially help the user understand the result. For UI work, consider an HTML sample page so the user can preview actions and outcomes.

## Code Guide
- Give each file, class, and function one clear job.
- Use specific names that describe what the code does.
- Keep related logic together.
- Keep unrelated logic separate.
- Follow existing project patterns unless asked to change them.
- Avoid hardcoding values when a clearer and more maintainable option exists.
- If hardcoding is used for any reason, tell the user and propose a rewrite to remove it.
- Before using a library, check the latest version through Context7 when available. Adjust the version when needed for compatibility or stability.
- Prefer project-local environments such as Python `venv`, and use `uv` instead of `pip`.
- If code work for the same request has already failed four times or more, debug the cause first before making more code changes.
