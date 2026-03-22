## Language
- Always converse with the user in Korean.
- Write Markdown documents in English unless the user asks for another language.
- When explaining technical content or asking the user for something, first explain the purpose and situation in a way a middle-school student can understand, then use technical terms.

## Workflow
- Use `AGENTS.md` to describe behavior rules.
- Use `README.md` to describe project goals, overall structure, and how to use the project.
- Prefer visual structure such as flowcharts when they make the structure easier to understand.
- Write `README.md` so that a middle-school student can follow it as written.

## Planning
- Assume the user describes only what they want in vague terms, without a clear final structure or implementation plan.
- Before fixing the design, judge what should be built, how large it should be, and what constraints matter.
- Record that understanding briefly in `AGENTS.md`.
- If the request is vague or the design looks excessive, explain the issue in an easy way with structure, examples, or visualizations, and get the user's confirmation before proceeding.
- During planning and before code work, ask the user in advance whether debug logging or tests should be added.
- If code work for the same request has already failed four times or more, debug the cause first before making more code changes.

## Security
- Ask the user first to set the security level for the project.
- Low: local personal use. Check input handling, path handling, secrets, logs, errors, permissions, and delete or overwrite behavior.
- Medium: external APIs, sensitive stored data, or shared device access. Check the low-level items and also review storage and access control.
- High: multiple users, public server exposure, or highly sensitive data. Design authentication, authorization, transport protection, and storage protection separately.
- If a security design looks heavier than the chosen level, explain why and get confirmation before proceeding.

## Git
- The user has multiple GitHub accounts, so ask which Git account to use at the start of the project.
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
- Before using a library, check the latest version through Context7 MCP. Adjust the version when needed for compatibility or stability.
- Prefer project-local environments such as Python `venv` instead of global installation.

## Documentation
- Write only the final content.
- Do not explain the document itself instead of its content.
- Do not describe before/after changes; write only the final result.
- Documentation minimization rule: do not write duplicate or near-duplicate content.
- If duplicate or overlapping content appears within a document or across documents, ask the user before merging, consolidating, or absorbing it.
- Do not restate what is already clear from structure, diagrams, or flowcharts.
- Write root `docs/` so that a middle-school student can read them easily.
- Use Mermaid only when it makes the explanation shorter or clearer.
- Do not write parent paths of the project root or other external local paths in documents.
- Do not write names, account information, or anything that looks like personal or account data in documents without the user's confirmation.
- If a document needs a local path or file name outside the user's project, get the user's confirmation before writing it.
