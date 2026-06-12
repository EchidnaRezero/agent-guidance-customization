---
name: when-writing-or-checking-markdown-docs
description: "Use when writing, editing, or checking Markdown docs; review the whole document after writing."
---

# When Writing Or Checking Markdown Docs

## Writing Criteria

### Default Content

- By default, write only these kinds of content unless a more specific local rule or the user's direct instruction requires more:
  - purpose: one line that says what it is for and what it is used to do; omit it when the title already makes that clear enough
  - structure
  - execution or behavior order
  - usage

### Do Not Add Without Explicit Need

- Unless the user explicitly asks for it, do not add:
  - prescriptive statements about what should or must be done
  - explanatory commentary or interpretation
  - statements about what the document does not cover or does not do, unless the document exists to forbid a specific action, command, or behavior
  - duplicate or near-duplicate content
  - content that repeats what is already said elsewhere in similar words
  - content that a reader can already understand immediately from the structure, diagrams, or flowcharts
  - change descriptions
  - self-referential meta text that explains the document itself unless it is necessary to the content
  - descriptions of content that is no longer present in the current document because it was changed or removed
- If the document needs content beyond the default content above and there is no local rule or user instruction that covers it, ask the user first.

### Local and Personal Data

- Do not write parent paths of the project root or external local paths without the user's confirmation.
- Do not write names, account information, or personal-looking data without the user's confirmation.

### Expression

- For documents that describe structure and behavior, present the overall view first with an appropriate visualization tool. Split it into separate visuals when the number of items is too large.
- If a prose block grows beyond five lines, split it into itemized sections.

## Review Criteria

- Check whether the document follows the writing criteria above.
- After writing or changing a document, read the whole document and check whether it contains duplicate or similar content. Ask the user before merging, consolidating, or absorbing overlapping content within one document or across documents.
