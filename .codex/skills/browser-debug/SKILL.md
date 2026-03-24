---
name: browser-debug
metadata:
  version: "1.2.0"
description: Reproduce, diagnose, fix, and verify frontend and browser-facing issues with an evidence-first loop. Use when pages fail to render, interactions break, console errors appear, network requests fail, visual regressions appear, hydration/runtime issues occur, or a user asks to debug a UI flow before claiming the fix works.
---

# Browser Debug

Deprecated compatibility layer: prefer `~/.agents/skills/browser-debug`.
Keep this copy aligned with the primary skill and remove it in the next major version.

## Goal

Turn browser bugs into a repeatable workflow: reproduce first, capture evidence, implement the smallest viable fix, rerun the flow, and report proof plus residual risk.

## Workflow

1. Reproduce the issue before editing.
- Identify the exact route, user action, viewport, and expected behavior.
- If reproduction is ambiguous, narrow it with one concise assumption.

2. Capture evidence.
- Collect console errors, failed network requests, and screenshots when tools are available.
- Record the failing step, observed behavior, and any unstable conditions.
- Prefer structured evidence over intuition.

3. Form a concrete hypothesis.
- State the likely root cause in one or two sentences.
- Tie the hypothesis to specific files, components, requests, or state transitions.

4. Apply the smallest verifiable fix.
- Avoid broad rewrites while the root cause is still unproven.
- Keep changes scoped to the failing behavior unless a wider fix is justified.
- Add or improve logs only when they materially help diagnosis or regression detection.

5. Rerun the exact failing flow.
- Use the same route, inputs, and viewport if possible.
- Confirm the original failure is gone and no adjacent regression is obvious.

6. Deliver with evidence.
- Report:
  - root cause
  - changed files
  - verification steps
  - residual risk or unknowns

## Guardrails

- Do not claim success without a rerun or equivalent evidence.
- Stop blind retries if the same fix pattern fails twice; explain the updated hypothesis.
- If the bug depends on unavailable browser tooling, say what evidence is missing and what remains unverified.
