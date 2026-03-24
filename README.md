# codex-harness-pack

[![Repo](https://img.shields.io/badge/repo-codex--harness--pack-24292f)](https://github.com/luminlee-labs/codex-harness-pack)
[![Codex Pack](https://img.shields.io/badge/codex-pack%201.2.0-blue)](https://github.com/luminlee-labs/codex-harness-pack)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

[English](#english) | [简体中文](#简体中文)

## Overview

`codex-harness-pack` is an **official-aligned Codex workflow pack**.

It aligns with OpenAI’s current layering model:

- `~/.codex/config.toml` for user-level configuration
- `~/.codex/AGENTS.md` for durable global guidance
- `.codex/config.toml` for repo-level overrides
- `$HOME/.agents/skills` and `$REPO_ROOT/.agents/skills` for skills discovery
- layered `AGENTS.md` / `AGENTS.override.md` for directory-level rules

References:

- [Codex Config Basics](https://developers.openai.com/codex/config-basic)
- [Custom instructions with AGENTS.md](https://developers.openai.com/codex/guides/agents-md)
- [Codex Skills](https://developers.openai.com/codex/skills)
- [Customization](https://developers.openai.com/codex/concepts/customization)
- [Workflows](https://developers.openai.com/codex/workflows)

## 30-Second Install

```bash
git clone https://github.com/luminlee-labs/codex-harness-pack.git
cd codex-harness-pack
rsync -a .codex/ ~/.codex/
rsync -a .agents/skills/ ~/.agents/skills/
```

Restart Codex after syncing.

## What this pack installs

- `~/.codex/config.toml`
- `~/.codex/AGENTS.md`
- `~/.agents/skills/*`

### Repo bootstrap templates

- `.codex/templates/repo/AGENTS.md`
- `.codex/templates/repo/.codex/config.toml`
- `.codex/templates/repo/docs/code_review.md`
- `.codex/templates/repo/docs/PLANS.md`
- `.codex/templates/bootstrap/repo-bootstrap.yaml`

## What this pack is not

- Not a runtime bundle
- Not an execution engine
- Not a replacement for repo-specific docs

It is a workflow/guidance/skills layer that helps Codex follow:

- plan-first execution
- test/check/review discipline
- minimal bootstrap scaffold by default

## Skills included

- `repo-bootstrap` (heavyweight, explicit by default)
- `pre-code-research` (heavyweight, explicit by default)
- `research-before-code`
- `browser-debug`
- `ts-refactor-safe`

Each skill includes `agents/openai.yaml` metadata and invocation policy.

## Compatibility note

`.codex/skills/` is kept as a **deprecated compatibility layer** for now.
Deprecated copies should mirror the primary `.agents/skills/` behavior and include a visible redirect notice so users do not encounter conflicting instructions.

Primary discovery path is now:

- Repo: `$REPO_ROOT/.agents/skills`
- User: `$HOME/.agents/skills`

In the next major version, deprecated `.codex/skills/` may be removed.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

---

<a id="english"></a>
## English

This repository provides an OpenAI-official-aligned Codex workflow pack.

Core principles:

- Config belongs in `config.toml`
- Durable guidance belongs in `AGENTS.md`
- Reusable workflows belong in skills
- Complex work should be planned first
- Code changes should include test/check/review evidence

For official references, use:

Official references are listed above in the Overview section.

<a id="简体中文"></a>
## 简体中文

这个仓库是一个与 OpenAI 官方分层对齐的 Codex workflow pack。

核心原则：

- 配置放在 `config.toml`
- 持久约定放在 `AGENTS.md`
- 可复用流程放在 skills
- 复杂任务先做计划
- 代码变更必须配测试/检查/review 证据

官方文档：

官方参考链接见上方 Overview 的 References。
