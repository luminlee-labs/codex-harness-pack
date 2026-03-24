---
title: "<topic> Architecture Brief"
slug: "<topic-kebab-case>"
status: "active" # draft | active | stale | superseded
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
review_due: "YYYY-MM-DD"
owner: "codex"
scope: "<what this report covers>"
tech_stack:
  - "<framework-or-platform>"
decision_summary: "<3-5 sentence summary of the final recommendation>"
recommended_architecture:
  - "<pattern A>"
  - "<pattern B>"
non_recommended:
  - "<pattern X>"
key_risks:
  - "<risk and mitigation in one line>"
source_of_truth: "official-first"
source_count: 0
---

# Technical Research and Architecture Brief Template

## 0. Context

- Problem statement:
- Target stack or candidate technologies:
- Constraints (time, team, infra, compliance, cost):
- Assumptions:
- Out-of-scope:

## 1. Official Architecture

- Officially recommended architecture pattern:
- Officially recommended core design patterns:
- Official sample/demo implementation style:
- Official best practices:
- Officially discouraged patterns:
- Official architecture structure (text diagram):

## 2. Community Best Practices

- Common implementation patterns in the community:
- Production engineering practices:
- Common production architecture structure:

## 3. Common Pitfalls

| Pitfall | Root Cause | Impact | Early Signal | Mitigation |
|---|---|---|---|---|
| | | | | |

## 4. Architecture Comparison

### Recommended Patterns

- Pattern A:
- Pattern B:

### Non-Recommended Patterns

- Pattern X:
- Pattern Y:

### Comparison Notes

| Pattern | Pros | Cons | Best Fit | Why Not (if non-recommended) |
|---|---|---|---|---|
| | | | | |

## 5. Data and State Design

- Data/state design principles:
- Module ownership boundaries:
- Data sharing mechanism between modules:
- State boundary and mutation rules:
- Anti-coupling and anti-state-pollution strategy:
- Recommended data structures:

## 6. Open-Source Case Studies (2-3)

### Project 1

- Repo/link:
- High-level structure:
- Module organization:
- Core component interactions:
- Data/state management:
- Why this works:

### Project 2

- Repo/link:
- High-level structure:
- Module organization:
- Core component interactions:
- Data/state management:
- Why this works:

### Project 3 (optional)

- Repo/link:
- High-level structure:
- Module organization:
- Core component interactions:
- Data/state management:
- Why this works:

## 7. Architecture Recommendation

- Recommended overall architecture:
- Recommended module decomposition:
- Recommended system structure:
- Recommended engineering implementation approach:
- Key tradeoffs and risks:

## 8. Implementation Guidance (Post-Research)

- Recommended code structure:
- Recommended module boundaries:
- Recommended directory layout:
- Recommended core component design:
- Suggested implementation order:

## 9. Review Notes

- Last review date:
- Why review was needed (age/version/constraints change):
- What changed since previous version:
- Impact on implementation plan:

## Sources

- Official references:
- Community references:
- Notes on recency/version/date:
