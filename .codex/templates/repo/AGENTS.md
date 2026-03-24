# Repository expectations

- Keep this file practical, short, and repo-specific.
- Prefer explicit commands and concrete constraints over abstract guidance.

## Commands

- Run the repo's lint, typecheck, test, and build commands before merge.
- When unsure, use the smallest relevant verification command first.

## Engineering rules

- Keep changes scoped and incremental.
- Avoid broad rewrites when a minimal fix is sufficient.
- Ask before adding new production dependencies or changing public APIs.

## Done means

- Code changes include relevant tests or explicit test rationale.
- Checks pass (or failures are documented with mitigation).
- Review performed (`/review` or equivalent diff review).

## Read next

- Complex work plans: `docs/PLANS.md`
- Review checklist: `docs/code_review.md`
- Directory-specific rules: nearest `AGENTS.md` / `AGENTS.override.md`
