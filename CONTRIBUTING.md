# Contributing

## Scope

This repository is an official-aligned Codex workflow pack. Keep contributions focused on:

- `~/.codex/config.toml` — user-level configuration
- `~/.codex/AGENTS.md` — durable global working agreements
- `.agents/skills/` — reusable workflow skills (official discovery path)
- `.codex/templates/` — bootstrap scaffold templates
- documentation that explains how the pack works

Do not add runtime-only files such as auth state, session history, logs, sqlite state, or caches.

## Directory Structure

```
~/.codex/
  config.toml       user-level config
  AGENTS.md         global working agreements

~/.agents/skills/  official skills discovery path (primary)
.codex/skills/     deprecated compatibility layer (may be removed in v2)

.codex/templates/  bootstrap scaffold templates
```

## Workflow

1. Fork or branch from `main`.
2. Keep changes small and reviewable.
3. Update documentation when behavior changes.
4. Validate any changed skill before opening a PR.
5. Describe what changed, how it was verified, and any remaining risks.

## Skill Changes

When editing a skill:

- keep the trigger boundary clear and concise
- prefer stable, reusable workflows over project-specific instructions
- preserve `metadata.version` in the SKILL.md frontmatter
- add or update `agents/openai.yaml` for invocation policy if needed
- update related bootstrap templates if behavior changes

## OpenAI Official References

When adding or changing behavior, cite official Codex documentation:

- Config: https://developers.openai.com/codex/config-basic
- AGENTS.md: https://developers.openai.com/codex/guides/agents-md
- Skills: https://developers.openai.com/codex/skills
- Customization: https://developers.openai.com/codex/concepts/customization
- Workflows: https://developers.openai.com/codex/workflows

## Pull Requests

A good PR should include:

- goal
- context
- constraints
- done when
- verification steps
- link to relevant official documentation

## Style

- Prefer concise documentation.
- Keep `README.md` English-first with a Chinese section.
- Keep the repository minimal; avoid adding extra scaffolding without a clear reason.
