## Policy Scope
- Treat this file as the highest-level global policy for the local Codex environment.
- Repository-local `AGENTS.md` files may add or override rules for that repository.
- Treat customized superpowers as the default workflow layer, not as a higher-priority policy layer.
- Do not copy superpowers meta-priority claims such as "skills override policy" or "check skills before any response" into this file.

## Language
- Always converse with the user in Korean.
- Write Markdown documents in Korean by default unless the user asks for another language.
- Write AI-facing instruction documents such as `AGENTS.md`, `SKILL.md`, and similar agent guidance in English unless the user asks for another language.
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
- Use `AGENTS.md` to describe behavior rules.
- Use `README.md` to describe project goals, overall structure, and how to use the project.
- Prefer visual structure such as flowcharts when they make the structure easier to understand.
- Write `README.md` so that a middle-school student can follow it as written.
- By default, project work should follow the customized superpowers workflow unless the current repository defines a more specific local workflow.

## Planning
- Assume the user may describe only a rough goal, without a stable structure or implementation plan.
- Use the customized superpowers workflow to decide when planning questions should be answered:
  - `brainstorming`: lock purpose, intended user, scope, output shape and expected benefit, and completion criteria.
  - `writing-plans`: lock technical stack, security level, debug logging strategy, test strategy, local environment approach, and Git usage plan.
- If the request is vague or the design looks excessive, explain the issue in an easy way with structure, examples, or visualizations, and get the user's confirmation before proceeding.
- Before suggesting a framework, library, or version, verify current information with Context7 when available.
- If code work for the same request has already failed four times or more, debug the cause first before making more code changes.

## Security
- Determine the project's security level during planning, normally in `writing-plans`.
- Low: local personal use. Check input handling, path handling, secrets, logs, errors, permissions, and delete or overwrite behavior.
- Medium: external APIs, sensitive stored data, or shared device access. Check the low-level items and also review storage and access control.
- High: multiple users, public server exposure, or highly sensitive data. Design authentication, authorization, transport protection, and storage protection separately.
- If a security design looks heavier than the chosen level, explain why and get confirmation before proceeding.

## Git
- In `writing-plans`, decide whether the chosen workflow will create a commit or push.
- If Git output will be created, ask which Git account to use during planning.
- Treat `Erika account` and `Inori account` as the same GitHub account choice, meaning `github_erika` in the local Git account mapping document.
- After the user chooses, check the local Git account mapping document in the user's home directory.
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

## Documentation
- Write only the final content.
- Do not explain the document itself instead of its content.
- Do not describe before/after changes; write only the final result.
- Documentation minimization rule: do not write duplicate or near-duplicate content.
- If duplicate or overlapping content appears within a document or across documents, ask the user before merging, consolidating, or absorbing it.
- Do not restate what is already clear from structure, diagrams, or flowcharts.
- Use Mermaid only when it makes the explanation shorter or clearer.
- Do not write parent paths of the project root or other external local paths in documents.
- Do not write names, account information, or anything that looks like personal or account data in documents without the user's confirmation.
- If a document needs a local path or file name outside the user's project, get the user's confirmation before writing it.
