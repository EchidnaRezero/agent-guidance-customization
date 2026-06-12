# Local Skill Creator Exceptions

Use this reference only when editing skills for this local Codex harness. Keep the upstream OpenAI `SKILL.md` guidance intact where possible, and put local deviations here instead of spreading them through the main skill body.

## Frontmatter

Required fields:

- `name`
- `description`

Locally allowed optional fields:

- `metadata`
- `license`
- `allowed-tools`

Do not add unsupported frontmatter fields. If a new field is needed, update the local validator and this reference in the same change.

## Korean Companion

When creating or updating `SKILL.md`, create or update `SKILL_KR.md` in the same pass. Keep the Korean companion aligned with the English canonical instructions, but do not treat the Korean file as the canonical source.

## Agents Metadata

`agents/openai.yaml` may contain local UI metadata such as display names, short descriptions, default prompts, icons, and policy settings. Read `references/openai_yaml.md` before changing it.

The local `policy.allow_implicit_invocation` setting is documented in `references/openai_yaml.md`. Keep that documentation synchronized with any generator or validator behavior that depends on it.
