---
name: pre-code-research
metadata:
  version: "1.2.0"
description: Perform architecture-first technical research before implementation and persist findings in the repository. Use when coding tasks involve multiple frameworks, unclear technical choices, large refactors, new system design, production-risk decisions, or explicit requests such as "先调研再写代码", "技术方案分析", "架构评估", "best practice", or "pitfalls". Apply to AI and non-AI projects. Re-check existing research docs periodically and treat YAML frontmatter as the first-pass summary.
---

# Pre-Code Research

Deprecated compatibility layer: prefer `~/.agents/skills/pre-code-research`.
Keep this copy aligned with the primary skill and remove it in the next major version.

## Goal

Reduce implementation risk by validating the technical path before coding.
Produce a decision-ready architecture brief with sources, tradeoffs, pitfalls, and concrete implementation guidance.

## Workflow

1. Check repository research artifacts first.
- Look for existing reports in `docs/research/`, `research/`, or `.ai/research/`.
- Read YAML frontmatter first for each candidate report before reading full content.
- Reuse existing conclusions when still valid; avoid duplicating reports.

2. Define scope and constraints before research.
- Capture target problem, delivery scope, runtime constraints, non-functional requirements, and timeline.
- List assumptions explicitly when requirements are incomplete.
- Ask a concise blocking question only when a wrong assumption would invalidate architecture choices.

3. Prioritize official documentation and primary sources.
- Use official docs, official examples, and official design guides as the baseline.
- Pin framework/library versions and date-stamp time-sensitive claims.
- Summarize:
  - official recommended architecture
  - official core design patterns
  - official demo or sample implementation style
  - official best practices
  - officially discouraged or anti-pattern usage

4. Collect community production practices.
- Review mature open-source repositories, issue discussions, and high-quality engineering posts.
- Separate high-consensus practices from niche opinions.
- Extract common production architecture patterns and operational practices.

5. Build a pitfall catalog.
- Identify common bugs, design mistakes, architecture misuse, performance bottlenecks, and maintainability traps.
- For each pitfall, provide cause, impact, early signal, and mitigation.

6. Compare architecture options.
- Provide recommended and non-recommended patterns.
- Explain pros, cons, fit scenarios, and risk boundaries.
- State why non-recommended options are risky under this context.

7. Design data and state boundaries.
- Define ownership boundaries by module.
- Define data flow direction and allowed sharing interfaces.
- Specify consistency rules, mutation boundaries, and anti-coupling constraints.
- Prefer structures that minimize cross-module hidden state and side effects.

8. Analyze 2-3 real-world open-source implementations.
- Explain system structure, module organization, component interactions, and state/data handling.
- State why each architecture works in practice and what tradeoffs it makes.

9. Produce the final recommendation before coding.
- Recommend:
  - overall architecture
  - module decomposition
  - system structure
  - engineering implementation approach
- Then provide implementation guidance:
  - code structure
  - directory layout
  - core component design
- Do not generate full implementation code unless the user explicitly asks for coding.

10. Persist research output back into the repository.
- Prefer `docs/research/` as the default output directory.
- If the repository already uses `research/` or `.ai/research/`, follow the existing convention.
- Name files as `YYYY-MM-DD-<topic>-architecture-brief.md`.
- Update existing report when it is the same scope; create a new file only when scope materially changes.

11. Re-check research periodically.
- Treat a report as stale when any of the following is true:
  - it is older than 30 days
  - core dependency versions changed
  - architecture constraints changed
- On stale reports, add a new "Review Notes" entry and refresh recommendations.
- Before implementation on complex tasks, perform a quick stale-check pass.

## Quality Bar

- Treat official docs as primary authority; use community sources to validate production reality.
- Cite links for key claims when browsing is available.
- Mark inference explicitly when evidence is indirect.
- Resolve source conflicts by explaining decision criteria.
- Keep the report concise, technical, and actionable; avoid generic theory.

## Output Format

Use the template at [references/research-report-template.md](references/research-report-template.md).
Adapt section depth to task size, but keep the same section order unless the user asks otherwise.
Always include YAML frontmatter that summarizes the entire document.
Read YAML frontmatter first when consuming existing reports.

## Execution Notes

- For OpenAI-specific tasks, pair this skill with official OpenAI references and prioritize official OpenAI sources.
- For non-AI stacks, keep the same structure and replace domain-specific sources accordingly.
- Keep YAML frontmatter concise but complete enough that another agent can decide relevance without reading the full file.

## Failure Handling

- If official or primary sources are missing, blocked, or too incomplete to support a confident recommendation, say so explicitly.
- Fall back to the smallest defensible recommendation set and label assumptions clearly.
- If the report is partial, do not present it as final architecture certainty.
- When possible, still persist a partial report so later runs can continue from known gaps instead of restarting from zero.
