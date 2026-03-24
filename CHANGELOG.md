# Changelog

All notable changes to `codex-harness-pack` are documented in this file.

## [1.2.0] — Official Alignment Update

> Reference: [OpenAI Codex Config](https://developers.openai.com/codex/config-basic) · [AGENTS.md guide](https://developers.openai.com/codex/guides/agents-md) · [Skills](https://developers.openai.com/codex/skills)

This release aligns the pack with OpenAI's official Codex layering model. Core directory structure, skill discovery path, and config responsibilities have been updated.

### Breaking changes

- **Skills moved**: All skills relocated from `.codex/skills/` to `.agents/skills/`. This follows the official skills discovery path (`$HOME/.agents/skills` for user-level skills). `.codex/skills/` is retained as a deprecated compatibility layer and may be removed in v2.
- **`developer_instructions` removed from `config.toml`**: The long-form developer instructions have been moved out of `~/.codex/config.toml`. `config.toml` now contains only configuration values, not working agreements.

### New

- **`~/.codex/AGENTS.md`** — New global working agreements file. Contains durable guidance that was previously embedded in `config.toml`: task structuring, plan-first discipline, test/check/review workflow, failure handling, and skill priority references.
- **`agents/openai.yaml` per skill** — Each bundled skill now includes `agents/openai.yaml` metadata with `display_name`, `short_description`, and invocation policy (`allow_implicit_invocation`). This enables explicit control over when Codex may implicitly trigger each skill.
- **`repo-bootstrap` minimum scaffold policy** — Bootstrap now defaults to a minimal set of files instead of a comprehensive docs tree. Heavy directories (`docs/research/`, `docs/debugging/`, `docs/decisions/`, etc.) are generated on demand only.
- **`docs/code_review.md` bootstrap template** — New template for code review scope, risk items, regression checkpoints, dependency policy, and merge readiness criteria. Review is now a first-class workflow item.
- **`docs/PLANS.md` bootstrap template** — Replaces the old `task-brief-template.md` and `exec-plan-template.md`. Uses the official `Goal / Scope / Risks / Steps / Verification / Status` structure.
- **`CONTRIBUTING.md` updated** — Now reflects the new directory structure and references official Codex documentation.

### Changed

- **`config.toml`** slimmed to contain only core config values such as `model`, `approval_policy`, `sandbox_mode`, `web_search`, and `personality`.
- **`repo-bootstrap` SKILL.md** rewritten with minimal scaffold as default behavior, clear template source paths (`.codex/templates/repo/`), and trimmed verbose sections.
- **`README.md`** rewritten with official-aligned positioning. Install command updated to dual-path (`rsync` for both `.codex/` and `.agents/skills/`). Added "what it installs / what it is not / official references" structure.
- **Bootstrap templates and compatibility copies** aligned to the minimal scaffold policy. Repo-level `config.toml` templates no longer carry workflow instructions, deprecated `.codex/skills/*` copies now include redirect notes, and old heavy bootstrap references were removed from compatibility assets.

### Skill invocation policy

| Skill | `allow_implicit_invocation` | Reason |
|---|---|---|
| `repo-bootstrap` | `false` | Heavyweight; explicit invocation preferred |
| `pre-code-research` | `false` | Deep research; explicit invocation preferred |
| `research-before-code` | `true` | Medium complexity; implicit OK |
| `browser-debug` | `true` | Common workflow; implicit OK |
| `ts-refactor-safe` | `true` | Common workflow; implicit OK |

### Bootstrap default output (v1.2)

Minimal by default:

- `AGENTS.md`
- `.codex/config.toml`
- `docs/code_review.md`
- `docs/PLANS.md`
- `.codex/repo-bootstrap.yaml`

On demand (only when explicitly needed):

- `docs/research/`
- `docs/debugging/`
- `docs/decisions/`
- `docs/exec-plans/active/`
- `docs/exec-plans/completed/`
- `docs/STATUS.md`

### Install command (v1.2)

```bash
rsync -a .codex/ ~/.codex/
rsync -a .agents/skills/ ~/.agents/skills/
```

Restart Codex after syncing.

### Official references cited in this release

- [Config Basics](https://developers.openai.com/codex/config-basic)
- [Custom instructions with AGENTS.md](https://developers.openai.com/codex/guides/agents-md)
- [Skills — Where to save skills](https://developers.openai.com/codex/skills)
- [Customization](https://developers.openai.com/codex/concepts/customization)
- [Workflows](https://developers.openai.com/codex/workflows)

---

## [1.1.0] — Previous release

(Previous changelog entries would go here.)
