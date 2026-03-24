---
name: ts-refactor-safe
metadata:
  version: "1.2.0"
description: Perform TypeScript and TSX refactors with explicit boundary mapping, scoped edits, and post-change verification. Use when renaming or moving modules, tightening or reshaping types, changing public interfaces, updating imports, splitting files, or refactoring code where breakage risk is non-trivial.
---

# Ts Refactor Safe

## Goal

Reduce refactor risk by identifying type boundaries first, limiting blast radius, and proving the code still compiles and behaves as expected.

## Workflow

1. Map the boundary before editing.
- Identify the source files, exported types, public functions, props, API contracts, and runtime entry points affected by the refactor.
- Call out likely breakage zones before making changes.

2. Reduce the blast radius.
- Prefer one bounded refactor step at a time.
- Avoid mixing renames, structural moves, and behavior changes unless they are tightly coupled.
- Keep unrelated cleanup out of the same change.

3. Refactor with explicit type intent.
- Preserve or intentionally redefine type boundaries.
- Prefer compiler-visible changes over hidden runtime assumptions.
- When changing a public type, note downstream callers that must move with it.

4. Verify immediately after the scoped edit.
- Run the narrowest useful verification first, then broader checks as needed.
- Prefer `tsc`, lint, and targeted tests before broader test suites when time matters.
- If verification fails, stop and explain the breakage instead of stacking more edits.

5. Report refactor risk clearly.
- Include:
  - changed files
  - type or interface changes
  - verification run
  - remaining breaking-risk areas

## Guardrails

- Do not start a broad refactor without understanding the exported boundaries.
- Avoid all-at-once edits across many files when smaller passes can prove safety.
- If unrelated changes appear in touched files, stop and separate the risk before continuing.
