# Contributing

## Scope

This repository is a minimal global Codex pack. Keep contributions focused on:

- `~/.codex/config.toml` global behavior
- `~/.codex/skills/` reusable workflow skills
- documentation that explains how the pack works

Do not add runtime-only user files such as auth state, session history, logs, or caches.

## Workflow

1. Fork or branch from `main`.
2. Keep changes small and reviewable.
3. Update documentation when behavior changes.
4. Validate any changed skill before opening a PR.
5. Describe what changed, how it was verified, and any remaining risks.

## Skill Changes

When editing a skill:

- keep the trigger boundary clear
- prefer stable, reusable workflows over project-specific instructions
- preserve version metadata
- update related bootstrap assets if behavior changes

## Pull Requests

A good PR should include:

- goal
- context
- constraints
- done when
- verification steps

## Style

- Prefer concise documentation.
- Keep `README.md` English-first with a Chinese section.
- Keep the repository minimal; avoid adding extra scaffolding without a clear reason.
