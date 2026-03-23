# codex-harness-pack / Codex Pack

这个仓库只保留可迁移的最小集合：

- `.codex/config.toml`
- `.codex/skills/`

目标很简单：把这里的 `.codex/` 同步到用户目录 `~/.codex/`，就把这套 Codex 全局约束和技能装上去。

## 安装方式

```bash
rsync -a .codex/ ~/.codex/
```

同步后重启 Codex。

## 运行逻辑

整个系统分两层：

1. `~/.codex/config.toml`
   - 这里的 `developer_instructions` 负责全局调度
   - 它决定什么时候先补结构、什么时候先看仓库、什么时候触发某个 skill

2. `~/.codex/skills/`
   - 这里放具体流程能力
   - 每个 skill 负责一种稳定、可重复的工作流

也就是说：

- `config.toml` 负责“总规则”
- `skills/` 负责“专项流程”

## 全局调度逻辑

当前 `developer_instructions` 的核心逻辑是：

1. 如果用户请求不够结构化，先补成：
   - `Goal`
   - `Context`
   - `Constraints`
   - `Done when`

2. 进入任何仓库时：
   - 先找最近的 `AGENTS.md`
   - 如果仓库里有 `.codex/`，把它当 repo workflow source

3. 开工前先检查：
   - `.codex/repo-bootstrap.yaml`
   - 如果 marker 有效，不重复初始化
   - 如果 marker 缺失或 stale，触发 `repo-bootstrap`

4. 任务分级：
   - 小任务：直接做
   - 中任务：先写 brief / plan
   - 大任务：先写 live exec plan

5. 交付标准：
   - 不能只交代码
   - 必须说明改了什么、怎么验证、约束是否满足、还剩什么风险

## Skill 优先级

当多个 skill 同时匹配时，按这个优先级处理：

1. `repo-bootstrap`
2. `browser-debug`
3. `ts-refactor-safe`
4. `research-before-code`
5. `pre-code-research`

规则是：

- 优先最具体的 skill
- 优先环境修复，再执行具体任务
- 当 `repo-bootstrap` 必须运行时，不能跳过

## Skills 与触发逻辑

当前这个包里有 5 个全局 skills。

### 1. `repo-bootstrap`

触发场景：

- 新仓库
- `.codex/repo-bootstrap.yaml` 缺失
- marker stale
- 仓库缺 `AGENTS.md` / `.codex/` / docs 基础骨架

它做的事：

- 先调研仓库实际结构
- 再按需生成 repo 级文件
- 最后写回 `.codex/repo-bootstrap.yaml`

它通常会创建或刷新：

- `AGENTS.md`
- `.codex/config.toml`
- `.codex/system-workflow.md`
- `.codex/task-brief-template.md`
- `.codex/exec-plan-template.md`
- `docs/`
- 必要时 repo-level skill 目录
- `.codex/repo-bootstrap.yaml`

### 2. `pre-code-research`

触发场景：

- 多框架或复杂技术选型
- 大改动、重构、架构设计
- 用户明确要求“先调研再写代码”
- 需要看官方最佳实践、社区最佳实践、坑点、模式对比

它做的事：

- 先做架构和技术调研
- 优先官方资料
- 再补社区实践、坑点、案例
- 最后把研究结果落回仓库文档

### 3. `research-before-code`

触发场景：

- 任务涉及陌生仓库
- 需求比较模糊
- 需要先读已有实现、docs、决策，再开始编码

它做的事：

- 先读 repo map、docs、相关代码路径
- 先收缩问题面
- 形成 brief / plan 后再编码

它偏“先研究这个仓库本身”，和 `pre-code-research` 的“先做技术/架构研究”不同。

### 4. `browser-debug`

触发场景：

- 页面打不开
- 交互失效
- console 报错
- network 异常
- 视觉回归
- hydration / runtime 问题

它做的事：

- reproduce
- capture evidence
- fix
- verify

也就是：

- 先复现
- 再取证
- 再修
- 再回归

### 5. `ts-refactor-safe`

触发场景：

- TypeScript / TSX 重构
- 改 public interface
- 改类型边界
- 拆文件、挪模块、批量改 import

它做的事：

- 先找边界
- 控制 blast radius
- 做 scoped refactor
- 跑验证
- 报 breaking risk

## 一个典型执行链路

一个新仓库里，Codex 的典型流程应该是：

1. 读取全局 `developer_instructions`
2. 进入仓库后先找 `AGENTS.md`
3. 检查 `.codex/repo-bootstrap.yaml`
4. 如果缺失或 stale，触发 `repo-bootstrap`
5. 仓库初始化完后再继续具体任务
6. 如果任务本身复杂，再触发：
   - `pre-code-research`
   - 或 `research-before-code`
   - 或 `browser-debug`
   - 或 `ts-refactor-safe`

## 当前校验结果

当前打包进来的 5 个 skills 已重新校验，结果都是通过：

- `repo-bootstrap`
- `pre-code-research`
- `browser-debug`
- `ts-refactor-safe`
- `research-before-code`

## 版本体系

这次补上了最小版本体系，分三层：

1. pack 版本
   - 当前 pack 版本：`1.1.0`
   - 写在 `.codex/config.toml` 的头部注释中
   - 也写入 `repo-bootstrap` 生成的 marker：`pack_version`

2. skill 版本
   - 每个 skill frontmatter 的 `metadata.version` 都是 `1.1.0`
   - 用来标识这个仓库里打包的 skill 版本

3. bootstrap / marker 版本
   - `bootstrap_version`
   - `marker_schema_version`

它们的职责不同：

- `bootstrap_version`：表示 bootstrap 行为本身的版本
- `marker_schema_version`：表示 `.codex/repo-bootstrap.yaml` 的结构版本
- `pack_version`：表示是哪一版全局 pack 触发或写入了这个状态

## 迁移策略

`repo-bootstrap` 现在明确采用：

- `migration_strategy: safe-refresh`

意思是：

- 优先增量刷新
- 优先补缺失项
- 优先更新受管骨架
- 不粗暴覆盖已有内容

当下面这些情况出现时，会走 refresh / migrate 路径，而不是简单 skip：

- `bootstrap_version` 过旧
- `marker_schema_version` 过旧
- marker 记录的关键文件缺失
- 仓库 stack 与 marker 明显不一致
- 仓库结构从简单单应用演化成更复杂形态

## Bootstrap 状态机

`repo-bootstrap` 现在有显式状态机。

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

这样就把原来隐式的判断形式化了，避免出现：

- 把 `stale` 误当成 full re-init
- 把 `partial` 误当成 safe skip
- 把 `failed` 误当成可以直接继续

## 幂等性声明

`repo-bootstrap` 现在的原则是幂等安全：

- marker 有效时：重复执行应该 no-op
- 仓库部分初始化时：只补缺失项或刷新 stale 项
- 不重复生成相同内容
- 不覆盖已有强约定
- 不在重复执行时把仓库越跑越乱

也就是说，重复跑 bootstrap 应该是安全的，最多是 refresh，不应该产生重复骨架。

## 可观测性

现在要求 `repo-bootstrap` 和全局调度都尽量说明“为什么”。

至少要能看见这些信息：

- 为什么触发
- marker 是 missing / valid / stale / schema-mismatch
- 本次走的是：
  - skip
  - initialize
  - refresh
  - migrate
- 创建了什么
- 刷新了什么
- 跳过了什么
- 哪些内容保持不动

这样用户至少能知道：

- 为什么这次会 bootstrap
- bootstrap 做了什么
- 为什么某些文件没有被改

## 开发状态文档

你提到的“打开 docs 下面某个文档，立刻知道现在开发到哪里了”，这次也补进去了。

当前约定是：

- repo bootstrap 生成 `docs/STATUS.md`
- 它是当前开发状态的最快入口
- 对 medium / large work，应保持它或 active exec plan 是最新的

`docs/STATUS.md` 的最小结构是：

- `Current Phase`
- `Current Focus`
- `Last Completed`
- `Next Step`
- `Blockers`
- `Last Updated`

所以答案是：

- 之前这套没有显式做成一个固定状态文档
- 现在补上了，由 `repo-bootstrap` 默认生成
- 后续只要维护这个文件，就能快速知道当前开发状态

## 失败兜底策略

这次也补了最小失败策略。

### 1. research 失败

如果仓库研究不完整或被阻塞：

- 允许退回到“最小中性骨架”
- 但必须显式写出假设
- 不能把未验证的推断伪装成确定事实

### 2. bootstrap 中途失败

如果 bootstrap 只做了一半：

- 不能宣称成功
- 必须报告最后成功的一步
- 必须报告已经创建了哪些文件
- 必须说明阻塞原因
- 必须给出最安全的下一步

### 3. marker 写入策略

如果是部分失败，不应该写出一个看起来“完全成功”的 marker。

必要时应写成：

- `status: partial`
- 或 `status: failed`

而不是误导后续运行直接 skip。

## 为什么只保留这两类文件

这个仓库现在故意只保留：

- `.codex/config.toml`
- `.codex/skills/`

因为对系统级用户目录来说，最核心的就是：

- 规则入口：`~/.codex/config.toml`
- 能力入口：`~/.codex/skills/`

其他诸如：

- `auth.json`
- `history.jsonl`
- `sessions/`
- `logs_*`
- `state_*.sqlite`
- `models_cache.json`
- `tmp/`
- `vendor_imports/`
- `rules/`

都属于运行态或机器态，不应该进仓库，已经通过 `.gitignore` 忽略。
