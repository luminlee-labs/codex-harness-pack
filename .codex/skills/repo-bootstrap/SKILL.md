---
name: repo-bootstrap
metadata:
  version: "1.1.0"
description: Audit, research, and initialize repository workflow scaffolding for Codex-driven development. Use when starting work in a new repository, when `.codex/repo-bootstrap.yaml` is missing or stale, when `.codex/config.toml`, `AGENTS.md`, `.codex/`, docs, or `.agents/skills/` scaffolding is missing, or when a project needs repo-specific planning, verification, and knowledge-management structure before implementation begins.
---

# Repo Bootstrap

## Goal

Make a new repository immediately usable for disciplined Codex work by researching the stack first, creating repo-specific workflow scaffolding, and writing a marker file so later sessions can skip re-initialization unless the bootstrap is stale.

## Marker Logic

Use `.codex/repo-bootstrap.yaml` as the bootstrap marker.

- If the marker exists, the version matches, and the referenced workflow files still exist, do not reinitialize the repo.
- If the marker is missing, initialize the repo.
- If the marker exists but is stale, refresh only the outdated pieces.

Treat the marker as stale when any of the following is true:

- `bootstrap_version` is older than the current template version
- `marker_schema_version` is older than the current supported marker schema
- required files listed in the marker are now missing
- the detected stack or package manager differs materially from the marker
- the repo moved from a simple project to a monorepo or multi-app structure

## Bootstrap State Machine

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

Interpretation rules:

- do not treat `stale` as full re-initialization by default; prefer refresh
- do not treat `partial` as safe to skip; prefer repair
- do not treat `failed` as success on the next run; use retry-safe behavior and re-evaluate the repository state

## Versioning And Migration

- `metadata.version` in this skill frontmatter versions the packaged skill itself.
- `bootstrap_version` versions the bootstrap behavior and baseline expectations.
- `marker_schema_version` versions the shape of `.codex/repo-bootstrap.yaml`.
- `pack_version` records which global pack version wrote the marker.

Migration strategy is `safe-refresh`:

- prefer additive or non-destructive updates
- refresh stale managed files when safe
- never overwrite populated repo files without approval
- if migration cannot be completed safely, stop and report the blocking reason instead of forcing a rewrite

## Idempotency

Repeated bootstrap runs must be safe.

- If the marker is valid and required files still match, bootstrap should no-op and report that it skipped work.
- If bootstrap runs again on a partially initialized repo, it should create only the missing pieces or refresh stale managed files.
- Bootstrap should not duplicate content or rewrite established repo conventions on repeated execution.

## Research Before Initialization

Before creating files, inspect the repository and infer:

- primary languages
- package manager or build system
- frontend, backend, or full-stack shape
- test, lint, build, or typecheck commands if present
- existing docs layout
- whether the repo already has stronger workflow conventions

Use that research to tailor the generated `AGENTS.md`, `.codex/`, docs tree, and repo-level skills. Do not just copy a generic scaffold blindly.

## Bootstrap Audit

Check for these items at the repository root:

- `AGENTS.md`
- `.codex/config.toml`
- `.codex/system-workflow.md`
- `.codex/task-brief-template.md`
- `.codex/exec-plan-template.md`
- `docs/STATUS.md`
- `docs/README.md`
- `docs/architecture/`
- `docs/feature-notes/`
- `docs/research/`
- `docs/exec-plans/active/`
- `docs/exec-plans/completed/`
- `docs/debugging/`
- `docs/decisions/`
- `.agents/skills/` or `skills/`

## Initialization Rules

1. Preserve existing repo conventions.
- If the repository already has a clear structure, extend it instead of overwriting it.
- Do not replace an established docs or tooling layout with the bootstrap layout.

2. Create the minimum viable scaffolding when missing.
- Use the templates under `assets/bootstrap/`.
- Initialize only missing files and directories.
- Prefer `.gitkeep` for empty directories that should persist.
- Add `.codex/repo-bootstrap.yaml` using the template under `assets/bootstrap/.codex/repo-bootstrap.yaml`.

3. Make `AGENTS.md` short.
- Keep it as an operational map.
- Put durable facts in `docs/`.
- Put reusable workflows in `.agents/skills/`.

4. Add planning and verification workflow.
- Ensure the repo has task brief and exec plan templates.
- Ensure the docs tree includes space for architecture, research, decisions, debugging, exec plans, and a current development-status document.
- Prefer generating project `.codex/config.toml` when it is missing, because OpenAI documents project config as an official repository-level surface.
- Create or refresh `docs/STATUS.md` as the fastest human-readable summary of current development state.

5. Generate repo-specific workflow, not only a generic skeleton.
- Tailor wording and examples to the detected stack.
- If the repo contains a frontend app, include browser-debug expectations.
- If the repo uses TypeScript, add or recommend TypeScript refactor safety workflow.
- If the repo is unfamiliar or architecture-heavy, include research-before-code guidance.

6. Write the marker file.
- Record:
  - pack version
  - marker schema version
  - bootstrap version
  - last input state
  - last transition
  - last trigger reason
  - initialized timestamp
  - last migration timestamp when applicable
  - detected stack
  - package manager
  - docs layout
  - initialized skills
  - required workflow files

7. Report the audit result.
- State:
  - why bootstrap triggered
  - input state
  - whether the path was skip, initialize, refresh, migrate, repair, or retry-safe
  - which files already existed
  - which files or directories were created
  - whether bootstrap was skipped, initialized, or refreshed
  - what was intentionally left untouched
  - any repo-specific convention that should override the default bootstrap

## Observability

Do not make bootstrap feel like a black box.

- Report the trigger reason explicitly.
- Report marker status: missing, valid, stale, schema_mismatch, partial, or failed.
- Report the chosen path: skip, initialize, refresh, migrate, repair, or retry-safe.
- Report created, refreshed, skipped, and untouched files separately.
- If assumptions were needed because research was incomplete, state them explicitly.

## Failure Handling

- If repository research is incomplete or blocked, fall back to the smallest neutral scaffold that is still defensible and list the assumptions made.
- If bootstrap fails partway through, do not claim success.
- Report the last successful step, partial files created, the blocking error, and the safest next action.
- If writing a marker after partial failure would be misleading, do not write a valid marker; if a status file is written, mark it as `partial` or `failed`.

## Guardrails

- Do not overwrite populated repo files without explicit user approval.
- If the repo already has a stronger convention, align to it and only fill the missing pieces.
- If the repo is not yet a code repository, initialize only the neutral workflow skeleton.
- Do not treat the marker as truth if the repo clearly drifted away from it; refresh selectively.
