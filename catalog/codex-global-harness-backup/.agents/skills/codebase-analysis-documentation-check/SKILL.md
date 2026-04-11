---
name: codebase-analysis-documentation-check
description: "Apply codebase-analysis rules on top of `when-writes-and-check-md_doc` when writing and checking a document that explains structure, relationships, or behavior."
---

# when-writes-and-check-codebase-analysis_doc

## Writing Criteria

### Scope

- Apply `when-writes-and-check-md_doc` first.
- Use this skill only for documents that explain codebase structure, relationships, or behavior.

### Structure

- Start with one or more overall views first, then move to detail sections tied to those overall views.
- For folder trees and file trees, use ASCII instead of diagrams.
- Use diagrams mainly to show the overall view of structure, order, or behavior. Unless the document is unusually complex or the user explicitly asks for more, keep the number of diagrams to three or fewer per document.
- Use tables for simple mappings or dependency lists.

### Diagram Semantics

- Use only arrow types that represent a single relationship within one diagram. If a different relationship needs a different arrow, split it into a separate diagram.
- State what each arrow means in nearby text, a legend, or a caption.

### Numbering and Mapping

- Use capital letters such as `A`, `B`, `C` for distinct overall views when the document contains multiple different wholes.
- Within one overall view, use numbered detail sections such as `A1`, `A2`, `B1`, `B2`.
- Do not mix detail numbers across different overall views.
- In overall-structure diagrams, use stable node labels that match the detail section family they belong to.
- Reuse the same labels and numbering in later detailed sections so readers can map each detail back to its parent overall view.
- See `references/numbering-and-mapping-example.md` for one complete sample.

### Style

- For numbered detail sections, prefer the title form `## [A1] name: what it does`.
- Keep subtitle explanations short and concrete, centered on observable behavior that a reader can see on screen or directly verify.

## Review Criteria

- If it is ambiguous whether the document is a codebase analysis document, ask the user before applying this skill.
- Check whether the document follows the writing criteria above.
