# Codex Global Working Agreements

> 参考: [Customization](https://developers.openai.com/codex/concepts/customization) · [AGENTS.md guide](https://developers.openai.com/codex/guides/agents-md) · [Prompting](https://developers.openai.com/codex/prompting) · [Workflows](https://developers.openai.com/codex/workflows) · [Skills](https://developers.openai.com/codex/skills)

## Task Structuring

Structure ambiguous requests into:

- **Goal** — what to achieve
- **Context** — relevant background
- **Constraints** — hard boundaries
- **Done when** — verifiable completion criteria

Source: [Prompting](https://developers.openai.com/codex/prompting)

## Plan First

- **Small task**: implement directly.
- **Medium task**: write a plan before editing.
- **Large or ambiguous task**: use the repo's planning surface, typically `docs/PLANS.md` or an existing `docs/exec-plans/` location. Never start large work without an explicit plan.

Source: [Prompting](https://developers.openai.com/codex/prompting) — "break it into smaller, focused steps"

## After Code Changes

Always run the relevant verification pipeline:

1. **Test** — run the smallest relevant test suite
2. **Check** — lint, typecheck, build
3. **Review** — use `/review` or review the diff manually

Source: [Workflows](https://developers.openai.com/codex/workflows) — `/review` is an official Codex workflow

## Never Finish With Code Only

Report:

- What changed
- How it was verified
- Whether constraints are satisfied
- Remaining risks or unknowns (residual risk)

If verification is not possible, say why explicitly.

## Upgrade Repeated Patterns

Promote repeated guidance by ladder:

```
prompt → AGENTS.md → skill → automation or CI
```

- Same mistake twice → update `AGENTS.md` or docs
- Workflow repeats at least twice with clear steps → suggest creating or updating a skill

Source: [Customization](https://developers.openai.com/codex/concepts/customization) — Skills package repeatable processes

## Skill Priority

When multiple skills match, prefer the most specific one. Never skip `repo-bootstrap` when it is required.

1. `repo-bootstrap` — new or stale repository
2. `browser-debug` — browser-facing runtime failures
3. `ts-refactor-safe` — non-trivial TypeScript/TSX refactors
4. `research-before-code` — unfamiliar repository or ambiguous task
5. `pre-code-research` — external architecture and best-practice research

Invoke skills explicitly (`$skill-name`) for heavyweight flows (`repo-bootstrap`, `pre-code-research`). Implicit invocation is appropriate for lighter workflows.

Source: [Skills](https://developers.openai.com/codex/skills)

## Failure Handling

- Stop blind retries after two failures. Explain the current hypothesis and propose the next step.
- If bootstrap or research fails partway through, report partial state, last successful step, blocking reason, and safest next action. Do not claim success.
- If blocked, explain the situation and propose an alternative path instead of looping.

## AGENTS.md Discovery

Codex loads AGENTS.md files in layered order. More specific (closer to CWD) guidance overrides earlier guidance. Use the closest `AGENTS.md` for directory-specific rules; create `AGENTS.override.md` for temporary overrides.

Source: [AGENTS.md guide](https://developers.openai.com/codex/guides/agents-md)

## What This File Is Not

This file stores durable working agreements. It is not:

- A technical encyclopedia
- A task executor
- A replacement for project documentation

Keep durable facts in `docs/`. Keep reusable workflows in `.agents/skills/`.
