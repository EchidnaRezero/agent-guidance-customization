---
name: when-after-creating-or-editing-markdown-docs
description: Must trigger immediately after creating or editing Markdown documentation to review and revise user-specified expressions.
---

# When After Creating Or Editing Markdown Docs

## Review Scope

- For existing documents, review only changed portions.
- For newly created documents, review all newly written content.
- If three or more Markdown documents are in scope, ask the user to choose the review scope before modifying them.

## Delete Meta Descriptions About The Document Or Its Components

Delete meta descriptions that explain the document itself or document components (such as sections, tables, figures, diagrams, and Mermaid charts) instead of the content.

Examples:

> This table is for showing the common structure.

> This diagram visually shows the dependency relationship.

> This document describes the restore process.

> This file is a generated output.

> This checklist is an operation guide.

> The following Mermaid diagram visualizes the request flow.

Delete expressions like these completely. Do not paraphrase, compress, or summarize them.

## Delete Tautological Or Redundant Expressions

Examples:

1. Heading repetition:
> ## Say only Y when removing X does not change the meaning
>
> In "not X, but Y" expressions, say only Y when removing X does not change the meaning.

Problem: The heading already states the rule, and the first sentence repeats it.

2. Table readback:
> | Role | Responsibility |
> |---|---|
> | Design orchestrator | Discuss ideas with the user, define verification criteria, write `design.md` |
> | Planning orchestrator | Write `plan.md` from `design.md`, hand off to implementation after user approval |
> | Implementation loop orchestrator | Coordinate implementation, verification, and debugging work |
> | Review orchestrator | Review code, improvements, refactoring, and security checks |
>
> This table summarizes each role and responsibility.

Problem: The sentence reads the table back by saying "This table summarizes each role and responsibility."

3. Formula or visual-structure readback:
> $$
> \begin{array}{l}
> \text{local martingale} \\[1ex]
> \quad \downarrow\ \text{(+ }M_0=0\text{, continuous paths, }\langle M\rangle_t=t\text{)} \\[1ex]
> \text{Wiener process}
> \end{array}
> $$
>
> Being a local martingale is not enough. The conditions $M_0=0$, continuous paths, and $\langle M\rangle_t=t$ are also needed.

Problem: The parenthesized condition already states the required conditions, but the prose repeats them outside the formula. It also has the "Not X, But Y" problem below.

4. Repeating below what was already said above:
> Use this form only when directly comparing X and Y, when the rejected idea already appears in the document, or when the user explicitly requests a caution note.
>
> ...(body)...
>
> Exception:
>
> Use the "not X, but Y" form when the user explicitly requests a caution note or when the rejected idea already appears in the document.

Problem: The exception repeats conditions already stated in the rule.

## Say Only Y When Removing X Does Not Change The Meaning

Use this form only when directly comparing X and Y, when the rejected idea already appears in the document, or when the user explicitly requests a caution note.

Flag these forms:

- Joined contrast: "It's not X - it's Y."
- Comma contrast: "This isn't about X, it's about Y."
- Split-sentence contrast: "The issue is not latency. The root cause is Y."
- Multi-negation countdown: "It's not the price. It's not the features. It's the trust."

The split version should be treated the same as the joined version. Each sentence may look harmless on its own, but the move is still a negative setup followed by a positive reveal.

The multi-negation countdown is the same move inflated across several options. Cut straight to the positive claim.

Examples:

> The bottleneck is not CPU usage but disk I/O. (X) => Disk I/O is the bottleneck. (O)

> The permission is not inherited from the project but assigned per workspace. (X) => The permission is assigned per workspace. (O)

> The failure is not a network issue. The root cause is an expired token. (X) => The root cause is an expired token. (O)

> It is not the price. It is not the feature count. It is the renewal timing. (X) => The renewal timing matters most. (O)

## Etc.

Delete from the reviewed content:

- Unrequested prescriptive or mandatory statements.
- Unrequested explanatory commentary or interpretation.
- Additions unsupported by user instructions or applicable local rules.
- Parent or external local paths, names, account information, or personal-looking data added without user confirmation.
