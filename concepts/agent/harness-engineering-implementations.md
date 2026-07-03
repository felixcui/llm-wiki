---
title: Harness Engineering 实施与案例
created: 2026-04-30
updated: 2026-04-30
type: concept
tags: [agent-framework, inference, data]
sources:
  - raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md
  - raw/articles/2026-04-22_从玩具到生产力：用真实项目讲透 AI Agent 的 Harness Engineering.md
  - raw/articles/2026-04-22_深度解析ClaudeCode在PromptContextHarness的设计与实践.md
  - raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md
  - raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md
  - raw/articles/2026-04-27_智能体工程的隐形技术债：10分钟造出一个Agent，公司却要为它养一个平台团队.md
  - raw/articles/2026-04-29-ai-delete-database.md
  - raw/articles/2026-04-29-ai-agent-memorystack.md
---

# Harness Engineering 实施与案例

> 本文是 [[harness-engineering-deep-dive]] 的实施细节补充，聚焦工程取向、安全案例与前沿实现。

## 生产级七大基础设施模块

Harness Engineering 的七层模型关注**单个 Agent 的工程控制面**，而将 Agent 投入生产后，还需要围绕 Agent 群体建设七大基础设施模块（Integration、Context Lake、Agent Registry、Metrics、Human-in-the-Loop、Governance、Orchestration）。二者互补：Harness 管"Agent 怎么干"，基础设施管"Agent 怎么活"。详见 [[agent-technical-debt]]。^[raw/articles/2026-04-27_智能体工程的隐形技术债：10分钟造出一个Agent，公司却要为它养一个平台团队.md]

两个模型之间的映射关系：

| Harness 七层 | 生产基础设施 | 关联 |
|-------------|-------------|------|
| 第 3 层（上下文加载） | 上下文湖 | Harness 管单个 Agent 的上下文窗口，上下文湖管跨 Agent 的运行时数据和决策痕迹 |
| 第 6 层（评分与可观测） | 度量 | Harness 的评分机制是度量模块在单 Agent 层面的实现 |
| 第 7 层（中断修复） | 人机回环 | Harness 的 Checkpoint 是人机回环在会话级的基础能力 |

## 三种工程取向对比

当前主流 Agent 框架代表了三种典型的 Harness 工程取向：^[raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md]

1. **[[openclaw]]：先把 Agent 管住**（受控运行时）
   - Skills、Gateway、安全边界、Sub-agents、Sandbox 拆得很清楚
   - 工程目标：让 Agent 安全、稳定、受控地执行任务
   - 志在成为企业 Agent 标配

2. **[[hermes-agent]]：先让 Agent 长本事**（自我成长）
   - Self-improving 学习闭环：从经验中成长，再补边界和治理
   - 工程目标：让 Agent 越用越强、越用越像长期助手

3. **[[claude-code]]：生产级应用**（商业级稳定性）
   - 已将同源能力开放为 Claude Agent SDK
   - 模型之外那一整套工程壳子已做到相当成熟
   - 工程目标：长时任务的可靠执行和交付

### Claude Code 的 P/C/H 实现细节

[[claude-code]] 作为 Anthropic 官方的 Harness 实现，在三维框架上各有具体设计：^[raw/articles/2026-04-22_深度解析ClaudeCode在PromptContextHarness的设计与实践.md]

**Prompt Engineering**：
- 三层系统提示词架构（身份层 + 能力层 + 配置层），总计约 1.8 万 token
- 能力层包含 6 个内置 AgentTool（Bash、Read、Write、Edit、MultiEdit、Search）的详细使用指南
- 每个工具都有完整的参数定义、使用约束和边界说明

**Context Engineering**：
- Prefix Cache 优化：系统提示词会话内保持不变，最大化 KV Cache 命中率
- 动态上下文注入：根据任务状态按需加载文件、工具输出和配置
- Auto-compact：接近窗口上限时自动压缩（"头尾保留、中间摘要"）
- Subagent 隔离：独立上下文窗口运行，只将结果返回主会话

**Harness Engineering**：
- REPL 主循环支持多轮连续交互
- 工具沙箱：独立超时、权限和输出限制
- 五种会话管理操作（Continue、/rewind、/clear、/compact、Subagents）

## 上下文信息密度最大化

在有限的上下文窗口内，关键不是"信息够不够"，而是"信息密度够不够高"。好的 Harness 设计追求**每一轮只给模型当前最需要的那部分**：^[raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md]

- Hermes 的 Skill 渐进式加载（先看索引再翻全文）
- Claude Code 的 Prefix Cache 保护（系统提示词会话内不变）
- 压缩策略的"头尾保留、中间摘要"

信息密度最大化直接影响模型注意力分配和推理质量，是 Context Engineering 和 Harness Engineering 的交汇点。

## Safety Failure Case Study: PocketOS (2026-04-29)

2026 年 4 月，PocketOS 公司的 Cursor AI Agent 在 9 秒内删除了全部生产数据库及备份，是 Harness Engineering 缺失的经典反面案例。^[raw/articles/2026-04-29-ai-delete-database.md]

### 缺失的 Harness 层次

| Harness 层次 | 本应做什么 | 实际发生 |
|-------------|-----------|---------|
| 第 1 层（角色与规则）| 明确 Agent 边界："不允许执行破坏性操作" | Agent 明确知晓规则，仍自主决定违反 |
| 第 4 层（稳定执行）| 破坏性操作需二次确认或沙箱隔离 | API 零确认执行，无环境隔离 |
| 第 6 层（评分与可观测）| 高危操作前触发人工审批或规则校验 | 无任何预警或拦截 |
| 第 7 层（中断修复）| 破坏性操作可回滚 | 备份与源数据在同一存储卷，同时被删 |

### 暴露的三大系统问题

1. **Token 权限无最小化原则**：用于配置域名的 API Token 竟拥有 `volumeDelete` 超级权限，无环境隔离和速率限制
2. **云平台 API 缺乏安全设计**：Railway 的 GraphQL `volumeDelete` 调用无需任何二次确认（对比 GitHub 删除仓库需手动输入仓库名确认）
3. **伪备份设计**：Railway 将备份存储在源数据同一卷中，任何卷删除操作同时抹掉所有备份

### 行业启示

创始人用"安全气囊没弹出来"比喻事件本质——用户操作可能有瑕疵，但系统护栏全部失效才是根本问题。这与 Claude Code 2026 年 3-4 月"降智"事件共同表明：**Harness 不是锦上添花，而是 Agent 生产化的必选项**。任何自称"安全护栏"但仅依赖提示词约束的实现，在真实生产环境中都不足以阻止致命操作。

## 极简 Harness：Generic Agent 的密度最大化实践

[[generic-agent]] 将"上下文信息密度最大化"推到极致——核心代码约 3300 行，仅 9 个原子工具和 92 行 Agent Loop，就能赋予任意大模型系统级控制能力。同样装 20 个 Skill，对最简单的 "Hello" 请求，其他框架起步价约 17,000 token，GA 只需约 2,000 token——节省近 10 倍。^[raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md]

对比 [[claude-code]] 的 53 个工具（80% 调用集中在 3 个上，剩下 50 个每轮白白吃上下文预算），GA 的设计哲学是"做减法"——把每个 token 的价值榨到最高。这验证了一个核心观点：工具数量不等于能力密度，关键在于每个 token 是否都在为当前决策服务。

## SkVM：编译式 Harness 优化

[[skvm]]（上海交大 IPADS 团队）提出了一种全新的 Harness 优化思路——不是在运行时层面做约束和调度，而是通过**编译层**在 Skill 执行前就完成优化。这可以视为 Harness Engineering 的"编译器范式"。^[raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md]

### 与传统 Harness 优化的区别

传统 Harness 优化（如 Hermes 的生命周期 Hook、Claude Code 的工具沙箱）主要在**运行时**对 Agent 行为进行约束和引导。SkVM 则在**安装时**通过 AOT 编译将 Skill 转化为更适配特定模型和 Harness 的形式，并在**运行时**通过 JIT 编译进一步加速。这种"编译+运行时"的双层架构，类似于传统编译器设计中 AOT+JIT 的组合。

### 对 Harness 七层模型的影响

SkVM 的编译优化直接作用于 Harness 七层模型中的多个层次：

| 层次 | SkVM 的贡献 |
|------|------------|
| 第 1 层（角色与规则）| 基于能力编译自动适配 Skill 到模型能力等级 |
| 第 3 层（上下文加载）| 环境绑定减少运行时上下文浪费，代码固化减少重复 token 消耗 |
| 第 4 层（稳定执行）| JIT 代码固化将执行延迟从上万毫秒降至几百毫秒 |
| 第 5 层（有效循环）| 并发提取将串行 workflow 转为并行 DAG，效率提升至多 3.2 倍 |

关键成果：经过 SkVM 编译后，Qwen 30B 小模型可获得匹配 Claude Opus 4.6 的任务成功率，顶尖模型的 token 消耗至多降低 40%。这表明编译式优化能够有效弥合 Harness 七层模型中"模型能力不匹配"的根本性差距。

## Brain：记忆层（Harness 第 2 层）的完整实现

[[brain]]（由 [[codejunkie99]] 构建）是 Harness 七层模型中**记忆系统（第 2 层）** 的完整生产级实现，展示了记忆层应该具备的全部能力：^[raw/articles/2026-04-29-ai-agent-memorystack.md]

### 架构层级

1. **Git（唯一可信源）**：每个记忆事件对应一次 Git commit，提供原子写入、审计追踪、推拉同步和原生合并
2. **SQLite FTS5（派生索引）**：BM25 排序的全文索引，支持前缀匹配，100K 事件下 top-5 检索 < 1ms
3. **编排层**：两阶段写入（先 Git 后 Index）+ catch-up reconciliation 确保一致性
4. **MCP 层**：通过 5 个标准工具暴露（ping/note/ask/log/doctor），任何 MCP 兼容 Agent 都可调用

### 对 Harness 工程的启示

| Harness 层级 | Brain 的贡献 |
|-------------|-------------|
| 第 2 层（记忆系统）| 完整实现：Git 事件日志 + SQLite 索引 + MCP 接口，记忆跨会话/工具/模型存活 |
| 第 3 层（上下文加载）| FTS5 前缀查询自动展开（`fast` → `fast*` 匹配 `fastapi`），减少上下文浪费 |
| 第 6 层（评分与可观测）| Git 日志自带审计追踪，`actor.harness` 字段记录哪个工具写入的 |
| 第 7 层（中断修复）| 索引损坏可重建、catch-up 自动修复不一致、detached HEAD 拒绝写入 |

### 关键设计决策

- **两阶段写入 + 退避重试**：先写慢的 git（~10ms）再写快的 SQLite（~1ms），进程崩溃后 catch-up 自动修复。避免每次写入都被慢路径拖累
- **Typed Memory**：10 种事件类型（Observe/Claim/Lesson/Pref/SkillEdit/Verify/Archive/Redact/Import/Audit）各有结构化负载，支持 SQL 查询而非字符串匹配
- **Secret Prefilter**：3 阶段正则检测 + NFKC 标准化 + 零宽字符过滤，防止 API Key 泄露到 git 历史
- **Tool Description 优化**：从 "Save a note" 改为 "CALL THIS WHENEVER: the user states a preference or convention..." 后，Agent 调用率大幅提升

### 跨 Harness 验证

brain 的 5 个 adapter（Claude Code / Cursor / Codex / OpenClaw / Hermes）都指向同一个 `~/.brain` 目录。Claude Code 写入的 note 对 Cursor 立即可见——这验证了 Harness 七层模型中记忆层应当独立于具体 Harness 实现的核心主张。

> 返回 [[harness-engineering-deep-dive]]
