---
name: repo-bootstrap
metadata:
  version: "1.1.0"
description: Audit, research, and initialize minimal repository workflow scaffolding for Codex-driven development. Use when starting work in a new repository, when `.codex/repo-bootstrap.yaml` is missing or stale, or when repo-level Codex baseline files are missing.
---

# Repo Bootstrap

## Goal

Make a repository immediately usable for Codex with a minimal, practical scaffold that follows official layering and avoids over-initialization.

## Trigger Conditions

Use this skill when:

- `.codex/repo-bootstrap.yaml` is missing or stale
- baseline repo workflow files are missing
- repository conventions are unclear and need a minimal normalized baseline

This skill is heavyweight. Prefer explicit invocation.

## Marker Logic

Use `.codex/repo-bootstrap.yaml` as the bootstrap marker.

- valid marker + required files still exist -> `skip`
- missing marker -> `initialize`
- stale marker or drifted repo files -> `refresh`
- partially initialized repo -> `repair`

## State Machine

Input state:

- `missing`
- `valid`
- `stale`
- `schema_mismatch`
- `partial`
- `failed`

Transitions:

- `missing -> initialize`
- `valid -> skip`
- `stale -> refresh`
- `schema_mismatch -> migrate`
- `partial -> repair`
- `failed -> retry_safe`

Output status:

- `success`
- `partial`
- `failed`

## Bootstrap Audit

Before writing files, inspect:

- languages and stack shape
- package/build/test/lint/typecheck commands
- existing docs/layout conventions
- existing AGENTS/config/skills
- whether stronger repo conventions already exist

If stronger conventions exist, align with them and only fill missing pieces.

## Minimal Scaffold Policy (Default)

By default, create only this minimal set when missing:

- `AGENTS.md`
- `.codex/config.toml`
- `docs/code_review.md`
- `docs/PLANS.md`
- `.codex/repo-bootstrap.yaml`

Do not create heavy docs trees by default.

## On-Demand Scaffold (Only When Needed)

Generate these only when the task explicitly needs them or the repo already uses them:

- `docs/research/`
- `docs/debugging/`
- `docs/decisions/`
- `docs/exec-plans/active/`
- `docs/exec-plans/completed/`
- `docs/STATUS.md`

## Template Sources

Use templates under:

- `.codex/templates/repo/`
- `.codex/templates/bootstrap/`

Do not use old heavy bootstrap templates as default behavior.

## Initialization Rules

1. Preserve existing conventions first.
2. Apply minimal scaffold only to missing parts.
3. Do not overwrite populated files without explicit approval.
4. Keep generated `AGENTS.md` short, practical, and repo-specific.
5. Prefer additive, non-destructive refreshes.

## Observability Requirements

Always report:

- trigger reason
- marker input state
- chosen path (`skip` / `initialize` / `refresh` / `repair`)
- created files
- refreshed files
- skipped files
- intentionally untouched files
- assumptions made when audit evidence is incomplete

## Failure Handling

If bootstrap cannot complete safely:

- do not claim success
- report last successful step
- report partial artifacts created
- report blocking reason
- propose safest next action

If marker would be misleading after partial failure, do not write a valid marker.

## Guardrails

- never force full rewrite when refresh/repair is sufficient
- never replace stronger repo conventions with generic scaffold
- prioritize minimal viable bootstrap over comprehensive boilerplate
- keep process idempotent and safe for reruns
