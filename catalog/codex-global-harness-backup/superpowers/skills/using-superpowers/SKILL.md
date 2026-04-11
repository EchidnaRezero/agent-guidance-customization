---
name: using-superpowers
description: Use when entering project work with this bundled superpowers workflow - routes work into the appropriate superpowers workflow after applying higher-level home and local policy
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<IMPORTANT>
Use superpowers as the default workflow for project work in this bundled package.
Do not let superpowers override higher-level home policy or repository-local policy.
</IMPORTANT>

## Instruction Priority

Use this order:

1. the parent harness package `AGENTS.md` policy layer
2. repository-local and project-local `AGENTS.md`
3. direct task requirements from the current request
4. superpowers workflow defaults

Do not add extra meta-priority claims here. This skill only routes work into the workflow.

## How to Access Skills

**In Codex:** Use Codex skill loading and follow the loaded skill directly.

## Environment Gate

Default guidance is Windows-first only for Windows environments.

If the current environment is not Windows, load one of these skills before interpreting shell snippets, filesystem paths, or install steps:

- `using-macos-environment`
- `using-linux-environment`
- `using-wsl-environment`

When a skill uses upstream tool names, translate them using `references/codex-tools.md`.

# Using Skills

## The Rule

For project work, use this flow:

1. `brainstorming` decides what to build.
2. `writing-plans` decides how to build it.
3. `execution` starts only after both are complete.

Check whether a superpowers skill clearly applies before entering project workflow or taking implementation action.

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "Non-Windows environment?" [shape=diamond];
    "Load matching OS skill" [shape=box];
    "About to enter project workflow?" [shape=diamond];
    "Already brainstormed?" [shape=diamond];
    "Need writing-plans?" [shape=diamond];
    "Invoke brainstorming skill" [shape=box];
    "Invoke writing-plans skill" [shape=box];
    "Use matching workflow skill" [shape=box];
    "Respond or continue work" [shape=doublecircle];

    "User message received" -> "Non-Windows environment?";
    "Non-Windows environment?" -> "Load matching OS skill" [label="yes"];
    "Non-Windows environment?" -> "About to enter project workflow?" [label="no"];
    "Load matching OS skill" -> "About to enter project workflow?";
    "About to enter project workflow?" -> "Respond or continue work" [label="no"];
    "About to enter project workflow?" -> "Already brainstormed?" [label="yes"];
    "Already brainstormed?" -> "Invoke brainstorming skill" [label="no"];
    "Already brainstormed?" -> "Need writing-plans?" [label="yes"];
    "Invoke brainstorming skill" -> "Need writing-plans?";
    "Need writing-plans?" -> "Invoke writing-plans skill" [label="no"];
    "Need writing-plans?" -> "Use matching workflow skill" [label="yes"];
    "Invoke writing-plans skill" -> "Use matching workflow skill";
    "Use matching workflow skill" -> "Respond or continue work";
}
```

## Red Flags

These thoughts mean STOP and re-check policy and workflow:

| Thought | Reality |
|---------|---------|
| "Superpowers decides policy" | Home and local `AGENTS.md` decide policy. |
| "The same shell guidance works everywhere" | Windows guidance and non-Windows OS skills are separate. Branch to an OS skill first on non-Windows environments. |
| "I can keep pushing forward even if planning changed the target" | Return to `brainstorming` when the target changes. |
| "This work doesn't need the workflow" | Project work still uses the workflow unless a more specific local workflow replaces it. |

## Skill Priority

When multiple workflow skills could apply, use this order:

1. **Process skills first** (brainstorming, debugging) - these determine HOW to approach the task
2. **Planning skills second** (writing-plans) - these lock implementation decisions
3. **Execution skills third** (executing-plans, subagent-driven-development) - these guide implementation

"Let's build X" → brainstorming first, then writing-plans.
"Fix this bug" → systematic-debugging first if the issue is repeated or unclear.

## Skill Types

**Rigid** (TDD, debugging): Follow exactly. Don't adapt away discipline.

**Flexible** (patterns): Adapt principles to context.

The skill itself tells you which.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.
