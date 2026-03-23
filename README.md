# codex-harness-pack

[![Repo](https://img.shields.io/badge/repo-codex--harness--pack-24292f)](https://github.com/luminlee-labs/codex-harness-pack)
[![Codex Pack](https://img.shields.io/badge/codex-pack%201.1.0-blue)](https://github.com/luminlee-labs/codex-harness-pack)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

[English](#english) | [简体中文](#简体中文)

<a id="english"></a>
## English

`codex-harness-pack` is a minimal global Codex pack.

It intentionally ships only the portable pieces that belong in the user-level `~/.codex/` directory:

- `.codex/config.toml`
- `.codex/skills/`

The goal is to make Codex behave like a reusable engineering runtime instead of a one-off prompt assistant.

### Quick Start

```bash
git clone https://github.com/luminlee-labs/codex-harness-pack.git
cd codex-harness-pack
rsync -a .codex/ ~/.codex/
```

Restart Codex after syncing.

### Install

```bash
rsync -a .codex/ ~/.codex/
```

Restart Codex after syncing.

### What This Pack Installs

This pack is intentionally small:

- `~/.codex/config.toml`
  - global routing and completion rules
- `~/.codex/skills/`
  - reusable workflow skills

It does not manage runtime-only files such as auth, session history, logs, sqlite state, or cache files.

### Core Model

This pack uses a two-layer model:

1. `~/.codex/config.toml`
   - acts as the global dispatcher
   - decides when to structure the task
   - decides when to inspect a repository
   - decides when to trigger a specialized skill

2. `~/.codex/skills/`
   - contains the workflow implementations
   - each skill handles one repeatable engineering pattern

In short:

- `config.toml` defines global behavior
- `skills/` defines reusable execution paths

### Global Dispatch Logic

The global `developer_instructions` are designed around these rules:

1. If a request is not well structured, normalize it into:
   - `Goal`
   - `Context`
   - `Constraints`
   - `Done when`

2. When entering a repository:
   - read the nearest `AGENTS.md`
   - if `.codex/` exists, treat it as the repo workflow source

3. Before non-trivial work:
   - check `.codex/repo-bootstrap.yaml`
   - if the marker is valid, reuse the repo workflow
   - if the marker is missing or stale, trigger `repo-bootstrap`

4. Task sizing:
   - small work: execute directly
   - medium work: write a brief plan first
   - large or ambiguous work: use a live exec plan

5. Completion:
   - never finish with code only
   - always report changes, verification, constraint status, and residual risk

### Skill Priority

If multiple skills match, use this priority:

1. `repo-bootstrap`
2. `browser-debug`
3. `ts-refactor-safe`
4. `research-before-code`
5. `pre-code-research`

Dispatch rules:

- prefer the most specific skill
- prefer environment repair before task execution
- never skip `repo-bootstrap` when bootstrap is required

### Included Skills

#### `repo-bootstrap`

Use for:

- new repositories
- missing `.codex/repo-bootstrap.yaml`
- stale repo bootstrap marker
- missing baseline repo workflow files

What it does:

- audits the repository first
- detects stack and conventions
- initializes or refreshes repo-level workflow files
- writes back `.codex/repo-bootstrap.yaml`

Typical outputs:

- `AGENTS.md`
- `.codex/config.toml`
- `.codex/system-workflow.md`
- `.codex/task-brief-template.md`
- `.codex/exec-plan-template.md`
- `docs/`
- optional repo-level skill directories
- `.codex/repo-bootstrap.yaml`

#### `pre-code-research`

Use for:

- architecture-first implementation work
- multi-framework or uncertain technical choices
- large refactors
- explicit “research before coding” requests

What it does:

- gathers official guidance first
- adds community practice and pitfalls
- compares implementation paths
- persists findings back into the repository

#### `research-before-code`

Use for:

- unfamiliar repositories
- ambiguous tasks
- work that requires reading existing docs, decisions, and implementation paths first

What it does:

- reads repo guidance and existing code paths
- reduces ambiguity
- produces a brief or plan before coding

#### `browser-debug`

Use for:

- broken pages
- failed interactions
- console errors
- network failures
- runtime or hydration issues
- browser-facing regressions

Loop:

- reproduce
- capture evidence
- fix
- verify

#### `ts-refactor-safe`

Use for:

- non-trivial TypeScript or TSX refactors
- public interface changes
- type-boundary changes
- module moves or import rewrites

What it does:

- maps boundaries first
- limits blast radius
- performs scoped refactors
- verifies after changes

### Typical Execution Flow

In a new repository, the expected sequence is:

1. load global `developer_instructions`
2. read the nearest `AGENTS.md`
3. inspect `.codex/repo-bootstrap.yaml`
4. trigger `repo-bootstrap` if required
5. continue task work
6. trigger another skill only if the task needs it

### Versioning

This pack includes a minimal version model:

- pack version
  - currently `1.1.0`
  - stored in `.codex/config.toml` comments
  - also written to repo bootstrap markers as `pack_version`
- skill version
  - each skill uses `metadata.version: "1.1.0"`
- bootstrap and marker version
  - `bootstrap_version`
  - `marker_schema_version`

### Migration Strategy

`repo-bootstrap` uses:

- `migration_strategy: safe-refresh`

Meaning:

- prefer incremental refresh
- prefer filling gaps over reinitializing everything
- prefer updating managed scaffolding only
- avoid overwriting stronger existing conventions

### Bootstrap State Machine

`repo-bootstrap` uses an explicit state machine.

Input states:

- `missing`
- `valid`
- `stale`
- `schema_mismatch`
- `partial`
- `failed`

Transitions:

- `missing -> initialize`
- `valid -> skip`
- `stale -> refresh`
- `schema_mismatch -> migrate`
- `partial -> repair`
- `failed -> retry_safe`

Output statuses:

- `success`
- `partial`
- `failed`

### Idempotency

Bootstrap is intended to be safe to rerun:

- valid marker: no-op
- partial repo setup: fill only missing or stale pieces
- do not duplicate scaffolding
- do not overwrite stronger repo conventions by default

### Observability

Bootstrap and skill dispatch should explain:

- why the skill triggered
- current marker state
- chosen path:
  - `skip`
  - `initialize`
  - `refresh`
  - `migrate`
  - `repair`
  - `retry_safe`
- what was created
- what was refreshed
- what was skipped
- what was left untouched

### Development Status Document

This pack includes a default repo status document through bootstrap:

- `docs/STATUS.md`

Its purpose is to make current development state visible at a glance.

Recommended fields:

- `Current Phase`
- `Current Focus`
- `Last Completed`
- `Next Step`
- `Blockers`
- `Last Updated`

### Failure Handling

Minimal failure policy:

- if repository research is incomplete, fall back to the smallest neutral scaffold and state assumptions explicitly
- if bootstrap fails halfway, do not report success
- partial failures should use `status: partial` or `status: failed`

### Validation

The pack currently includes five validated skills:

- `repo-bootstrap`
- `pre-code-research`
- `research-before-code`
- `browser-debug`
- `ts-refactor-safe`

---

<a id="简体中文"></a>
## 简体中文

`codex-harness-pack` 是一个最小化的 Codex 全局安装包。

它只保留应该放在用户级 `~/.codex/` 目录中的可迁移内容：

- `.codex/config.toml`
- `.codex/skills/`

目标是把 Codex 从“一次性 prompt 助手”变成“可复用的工程运行时”。

### 快速开始

```bash
git clone https://github.com/luminlee-labs/codex-harness-pack.git
cd codex-harness-pack
rsync -a .codex/ ~/.codex/
```

同步后重启 Codex。

### 安装方式

```bash
rsync -a .codex/ ~/.codex/
```

同步后重启 Codex。

### 这个包实际安装什么

这个包刻意只安装两类内容：

- `~/.codex/config.toml`
  - 负责全局调度和完成标准
- `~/.codex/skills/`
  - 负责可复用的流程型能力

它不会管理这些运行态文件：

- 认证文件
- 会话历史
- 日志
- sqlite 状态
- 各类缓存

### 核心模型

整个系统分两层：

1. `~/.codex/config.toml`
   - 充当全局调度器
   - 决定什么时候先补任务结构
   - 决定什么时候先看仓库
   - 决定什么时候触发某个 skill

2. `~/.codex/skills/`
   - 放工作流实现
   - 每个 skill 负责一个稳定、可重复的工程模式

也就是说：

- `config.toml` 定义全局行为
- `skills/` 定义可复用执行路径

### 全局调度逻辑

当前 `developer_instructions` 的核心规则是：

1. 如果用户请求不够结构化，先补成：
   - `Goal`
   - `Context`
   - `Constraints`
   - `Done when`

2. 进入仓库时：
   - 先找最近的 `AGENTS.md`
   - 如果有 `.codex/`，把它当作 repo workflow source

3. 开始非 trivial 工作前：
   - 检查 `.codex/repo-bootstrap.yaml`
   - marker 有效则复用现有仓库工作流
   - marker 缺失或 stale 则触发 `repo-bootstrap`

4. 任务分级：
   - 小任务：直接做
   - 中任务：先写 brief 或 plan
   - 大任务或模糊任务：先写 live exec plan

5. 完成标准：
   - 不能只交代码
   - 必须说明改了什么、如何验证、约束是否满足、残余风险是什么

### Skill 优先级

如果多个 skill 同时匹配，按以下优先级：

1. `repo-bootstrap`
2. `browser-debug`
3. `ts-refactor-safe`
4. `research-before-code`
5. `pre-code-research`

调度规则：

- 优先最具体的 skill
- 优先环境修复，再执行任务
- 当必须 bootstrap 时，不能跳过 `repo-bootstrap`

### 内置 Skills

#### `repo-bootstrap`

适用场景：

- 新仓库
- `.codex/repo-bootstrap.yaml` 缺失
- bootstrap marker 已过期
- 仓库缺基础 workflow 文件

它会：

- 先审计仓库
- 识别技术栈与现有约定
- 初始化或刷新 repo 级工作流文件
- 写回 `.codex/repo-bootstrap.yaml`

典型输出：

- `AGENTS.md`
- `.codex/config.toml`
- `.codex/system-workflow.md`
- `.codex/task-brief-template.md`
- `.codex/exec-plan-template.md`
- `docs/`
- 必要时 repo-level skills
- `.codex/repo-bootstrap.yaml`

#### `pre-code-research`

适用场景：

- 架构优先的实现任务
- 多框架或技术选型不明确
- 大改动或重构
- 明确要求“先调研再写代码”

它会：

- 先查官方资料
- 再补社区实践和坑点
- 对比不同实现路径
- 把研究结果落回仓库

#### `research-before-code`

适用场景：

- 面对陌生仓库
- 任务比较模糊
- 需要先读已有 docs、决策和代码路径

它会：

- 先看 repo 指南和相关实现
- 收缩问题范围
- 先形成 brief 或 plan，再编码

#### `browser-debug`

适用场景：

- 页面打不开
- 交互失效
- console 报错
- network 异常
- 浏览器侧运行时或 hydration 问题

执行闭环：

- reproduce
- capture evidence
- fix
- verify

#### `ts-refactor-safe`

适用场景：

- 非 trivial 的 TypeScript / TSX 重构
- public interface 变更
- 类型边界调整
- 模块移动或 import 批量重写

它会：

- 先找边界
- 限制 blast radius
- 做 scoped refactor
- 修改后跑验证

### 典型执行链路

一个新仓库中的标准流程应该是：

1. 读取全局 `developer_instructions`
2. 找最近的 `AGENTS.md`
3. 检查 `.codex/repo-bootstrap.yaml`
4. 如果需要，触发 `repo-bootstrap`
5. 再继续具体任务
6. 如果任务需要，再触发其他专项 skill

### 版本体系

当前使用最小版本模型：

- pack 版本
  - 当前为 `1.1.0`
  - 写在 `.codex/config.toml` 的注释里
  - 也会写入 bootstrap marker 的 `pack_version`
- skill 版本
  - 每个 skill 使用 `metadata.version: "1.1.0"`
- bootstrap / marker 版本
  - `bootstrap_version`
  - `marker_schema_version`

### 迁移策略

`repo-bootstrap` 当前采用：

- `migration_strategy: safe-refresh`

意思是：

- 优先增量刷新
- 优先补缺失项
- 优先更新受管骨架
- 默认不粗暴覆盖已有更强约定

### Bootstrap 状态机

`repo-bootstrap` 使用显式状态机。

输入状态：

- `missing`
- `valid`
- `stale`
- `schema_mismatch`
- `partial`
- `failed`

状态迁移：

- `missing -> initialize`
- `valid -> skip`
- `stale -> refresh`
- `schema_mismatch -> migrate`
- `partial -> repair`
- `failed -> retry_safe`

输出状态：

- `success`
- `partial`
- `failed`

### 幂等性

bootstrap 默认应可安全重复执行：

- marker 有效时：no-op
- 仓库部分初始化时：只补缺失项或刷新 stale 项
- 不重复造骨架
- 默认不覆盖已有更强 repo 约定

### 可观测性

bootstrap 和 skill 调度应说明：

- 为什么触发
- marker 当前状态
- 本次走了哪条路径
- 新建了什么
- 刷新了什么
- 跳过了什么
- 保持不动的是什么

### 开发状态文档

这个包通过 bootstrap 默认生成：

- `docs/STATUS.md`

它用于让人“一眼看出当前开发状态”。

建议字段：

- `Current Phase`
- `Current Focus`
- `Last Completed`
- `Next Step`
- `Blockers`
- `Last Updated`

### 失败兜底

最小失败策略：

- 仓库调研不完整时，退回到最小中性骨架，并明确写出假设
- bootstrap 半途失败时，不能宣称成功
- 部分失败应写成 `status: partial` 或 `status: failed`

### 当前校验结果

当前包内 5 个 skill 已通过校验：

- `repo-bootstrap`
- `pre-code-research`
- `research-before-code`
- `browser-debug`
- `ts-refactor-safe`

### 开源说明

- License: [MIT](./LICENSE)
- Contribution Guide: [CONTRIBUTING.md](./CONTRIBUTING.md)
