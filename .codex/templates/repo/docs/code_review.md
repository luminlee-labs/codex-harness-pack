# Code Review Checklist

## Scope

- Review all changed files in the PR diff.
- Verify behavior changes, not just code style.

## Common risk items

- Regression risk in adjacent modules
- Error handling gaps and edge cases
- Security and permission assumptions
- Performance degradation on hot paths

## Regression checkpoints

- Critical user flows still work
- Existing tests still pass
- Changed behavior has targeted test coverage

## Dependency policy

- New production dependencies require explicit justification
- Prefer existing libraries when equivalent

## Public API change policy

- Public API changes require compatibility notes
- Breaking changes need migration guidance

## Merge readiness

A change is merge-ready only when:

- review comments are resolved
- verification results are attached
- residual risks are explicitly documented
