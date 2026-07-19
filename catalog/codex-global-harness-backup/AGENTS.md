## Policy Scope
- Treat this file as the highest-level global policy for the `.codex` environment.
- Repository-local `AGENTS.md` files may add or override rules for that repository.

## Language
- Always converse with the user in Korean.
- For documents for AI (`AGENTS.md`, `SKILL.md`, or harness documents), write in English by default unless another language is requested. Write all other documents in Korean by default unless otherwise instructed.
- When creating or updating harness documents, use the `korean_companion_sync` custom agent to keep the canonical English document and `_KR` companion synchronized.
- When talking with a user, describe actions and results in terms the user can directly observe or verify before using technical jargon.

## Writing Docs
- Use contrastive negation such as “not A but B” only when rejecting A is necessary because it is a likely misconception, a safety concern, or the core contrast. Otherwise, omit A and state only the intended B.
- Follow a predefined template so the content is structured and cohesive.
- Avoid meta-expressions that describe the document instead of its content.
- Do not mention, restate, or describe the user's chat instructions in the document. They are behavior rules for the agent, not document content.

## Answer Style
- Put the conclusion or summary first.
- Cover one topic in about one paragraph. If a longer answer is unavoidable, keep it focused and end with one brief preview of what comes next.

## Environment
- In Windows environments, when suggesting commands that the user may run directly, prefer `cmd` command syntax over PowerShell unless PowerShell-specific behavior is required.

## Security
- Begin assessing the project's security level when the environment is tracked by Git or when the user discusses deployment or web connectivity.
- Low: local personal use. Check input handling, path handling, secrets, logs, errors, permissions, and delete or overwrite behavior.
- Medium: external APIs, sensitive stored data, or shared device access. Check the low-level items and also review storage and access control.
- High: multiple users, public server exposure, or highly sensitive data. Design authentication, authorization, transport protection, and storage protection separately.
- If a security design looks heavier than the chosen level, explain why and get confirmation before proceeding.

## Git
- Use read-only Git commands directly only when file history or existing changes are necessary evidence for the current request.
- Do not perform Git checks as a default preflight for unrelated work.
- Use the `git_steward` custom agent for Git workflow decisions and state-changing Git operations, including staging, commit, push, pull, merge, branch or worktree changes, discard or cleanup, identity selection, and SSH account checks.

## Plan And Brainstorming
- Use simple visualization tools when they materially help the user understand the result. For UI work, consider an HTML sample page so the user can preview actions and outcomes.

## Code Guide
- Apply KISS: keep code and structure as simple as the task allows. Give each file, class, and function one clear responsibility, use specific names that include a verb whenever practical, and keep related logic together.
- When there is no separate request or the existing design and code are not themselves faulty, follow the existing codebase’s structure, naming conventions, code style, and established patterns when modifying it.
- Prohibit hardcoding except for prototypes or mockups shown to the user, and one-off tests or verification performed before project work begins or while it is underway.
- Before using a library, check the latest version through Context7 when available. Adjust the version when needed for compatibility or stability.
- Prefer project-local environments such as Python `venv`, and use `uv` instead of `pip`.
- If code work for the same request has already failed four times or more, debug the cause first before making more code changes.
