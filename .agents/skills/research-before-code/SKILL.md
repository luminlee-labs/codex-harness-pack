---
name: research-before-code
metadata:
  version: "1.2.0"
description: Research the repository, existing docs, prior decisions, and relevant implementation paths before coding. Use when a task is new, ambiguous, spans multiple files or layers, touches unfamiliar modules, requires architecture decisions, or the user explicitly asks to plan, investigate, or "先调研再写代码".
---

# Research Before Code

## Goal

Reduce implementation mistakes by turning ambiguous work into a repo-aware brief, a concrete plan, and a smaller coding surface.

## Workflow

1. Read the repo map first.
- Start with the nearest `AGENTS.md`.
- For non-trivial work, look for the repo's current planning surface before assuming a template path.
- Prefer `docs/PLANS.md` when the repo uses the minimal bootstrap scaffold; otherwise reuse an existing `docs/exec-plans/` layout if the repo already has one.

2. Read durable repo knowledge before code.
- Check `docs/architecture/`, `docs/feature-notes/`, `docs/decisions/`, `docs/debugging/`, and active exec plans when relevant.
- Reuse existing conclusions when they are still valid.

3. Inspect the real implementation path.
- Identify the files, modules, commands, tests, and runtime surfaces likely involved.
- Prefer reading the smallest set of concrete files that explains the problem.

4. Produce a short implementation brief.
- Capture:
  - goal
  - relevant context
  - constraints
  - done criteria
  - likely file set
  - known risks

5. Plan before editing when needed.
- Medium task: write a brief plan in the response.
- Large or ambiguous task: create or update a live plan in the repo's planning surface, typically `docs/PLANS.md` or an existing `docs/exec-plans/active/` location.
- Do not start large work without an explicit plan.

6. Only then start implementation.
- Prefer small, verifiable steps.
- Update docs when a durable decision or debugging insight is discovered.

## Guardrails

- Do not confuse code reading with planning; produce a brief or plan before large changes.
- If repo docs and code disagree, note the mismatch and prefer the verified implementation path.
- If the task repeats across repositories without repo-specific assumptions, consider promoting the workflow to a global skill.

## Failure Handling

- If repo docs are absent, stale, or contradictory, say that explicitly and fall back to verified implementation paths.
- If the repository is too incomplete to support a confident plan, produce the smallest defensible brief, list assumptions, and identify the missing evidence that blocks a stronger plan.
