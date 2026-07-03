---
title: Prompt / Context / Harness Engineering
created: 2026-04-25
updated: 2026-05-09
type: concept
tags: [agent-framework, inference, data]
sources:
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md
  - raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md
  - raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md
  - raw/articles/2026-04-22_从玩具到生产力：用真实项目讲透 AI Agent 的 Harness Engineering.md
  - raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md
  - raw/articles/2026-04-22_深度解析ClaudeCode在PromptContextHarness的设计与实践.md
  - raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md
  - raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md
  - raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md
  - raw/articles/2026-04-29_HarnessEngineering实践心得：如何高效驾驭AI？.md
  - raw/articles/2026-04-30_深入浅出HarnessEngineerring之核心模式与理念.md
  - raw/articles/2026-05-09-yidian-context-engineering.md
  - raw/articles/2026-05-09-ai-engineer-roadmap-2026.md
---

# Prompt / Context / Harness Engineering

Agent 系统的三维设计分析框架，由 [[hermes-agent]] 率先系统化提出，将 Agent 工程拆解为三个正交维度：Prompt Engineering（模型交互层）、Context Engineering（上下文管理层）、Harness Engineering（运行时保障层）。

## 框架总览

传统的 Agent 开发往往聚焦于提示词优化或工具调用，缺乏系统性的工程视角。三维框架的核心洞察是：一个高质量的 Agent 是三个维度协同作用的结果，任何单一维度的短板都会导致整体表现下降。[[claude-code]] 2026 年 3-4 月的"降智"事件就是典型例证——问题不在模型本身，而在 Harness 层的三个 Bug 叠加。^[raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md]

## 维度一：Prompt Engineering（系统提示词设计）

Prompt Engineering 在三维框架中指**系统级提示词的架构设计**，而非简单的提示词调优。

### 核心概念

- **动态拼装范式**：基础结构为"定义 Agent 身份 → 加载配置文件 → 注入工具指南"，按需动态组装而非静态硬编码
- **模型异构兼容**：不同模型有不同"性格"，需针对性调整指令风格。Claude 天然强调工具使用；GPT/Codex 容易"只说不做"需强调"执行而非描述"；Gemini 需提醒使用绝对路径和并行调用
- **跨平台配置兼容**：支持读取 [[claude-code]] 的 CLAUDE.md、[[openclaw]] 的 AGENT.md/SOUL.md/USER.md、.cursorrules 等，实现零成本迁移^[raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md]

### 最佳实践

1. 提示词应模块化，避免大段硬编码
2. 针对目标模型特性做差异化适配
3. 支持多 IM 平台（WhatsApp、Slack 等）的提示词变体

## 维度二：Context Engineering（上下文窗口管理）

Context Engineering 关注**如何在有限的上下文窗口内高效利用信息**，是 Agent 长时间运行的关键。

### 核心概念

- **上下文压缩**：当上下文达到模型窗口阈值时自动触发压缩。Hermes 采用相对阈值（50% 窗口），[[openclaw]] 采用绝对阈值。压缩策略通常为"头尾保留、中间摘要"^[raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md]
- **记忆架构**：双层设计——内部静态记忆（MEMORY.md 等文件）存储长期事实，外部动态记忆（Mem0、Honcho 等服务）支持向量检索和跨会话记忆。[[hermes-agent]] 进一步提出了五层记忆架构（短期记忆、技能手册、知识库、用户建模、工作日志），详见下文。^[raw/articles/2026-04-30_深入浅出HarnessEngineerring之核心模式与理念.md]
- **上下文注入**：通过 `@` 符号即时挂载资源（文件、diff、URL），将工具调用转化为上下文预加载，省去多轮推理

### 最佳实践

1. 压缩时保留任务定义（头部）和最近对话（尾部）
2. 记忆写入需区分事实性知识和临时上下文
3. 长程任务需保留完整 reasoning 历史（如 [[deepseek-v4]] 的做法）

### Hermes 五层记忆架构

[[hermes-agent]] 将记忆体系细化为五层，每层对应不同粒度和持久度：^[raw/articles/2026-04-30_深入浅出HarnessEngineerring之核心模式与理念.md]

| 层级 | 名称 | 说明 | 持久性 |
|------|------|------|--------|
| L1 | 短期记忆（便利贴） | 当前对话的临时信息，会话结束后丢弃 | 临时 |
| L2 | 技能手册（肌肉记忆） | 完成复杂任务（如 5 次以上工具调用）后自动生成 SKILL.md，记录完整解决步骤，形成可复用流程 | 持久 |
| L3 | 知识库（语义记忆） | 基于向量存储的语义检索，即使字面不同、语义相近的文本也能匹配（如"进度报告"与"项目周报"相似度 0.92） | 持久 |
| L4 | 用户建模（对你的了解） | 通过黑格尔"辩证式"持续观察和修正对用户的认知（如"林总平时喝美式，但周三下午会换拿铁"），允许改变和例外 | 持久且动态 |
| L5 | 工作日志（长期档案） | FTS5 全文检索 + LLM 摘要，跨会话搜索历史对话，永久存储 | 永久 |

五层架构的工作流：L1 捕获实时对话 → 高频使用的模式沉淀为 L2 技能 → 可检索的知识进入 L3 向量库 → 对用户偏好的持续理解构建 L4 模型 → 所有跨会话历史在 L5 中归档。此分层设计与 [[harness-engineering-implementations]] 中 Git 日志 + SQLite FTS5 索引 + MCP 接口的记忆栈实现相互印证。

## 维度三：Harness Engineering（运行时框架）

Harness Engineering 是**连接模型能力与实际任务的运行时基础设施**，决定 Agent 的可靠性和可扩展性。

### 核心概念

- **生命周期 Hook**：on_agent_start、on_tool_call、on_tool_result、on_pre_compress、on_memory_write 等，覆盖 Agent 运行的全流程^[raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md]
- **错误分类与恢复**：标准化错误分类体系（认证失败、超时、参数错误等），每种预设自动恢复策略
- **Tool 系统**：薄抽象、显式控制流、贴近 SDK 的实现方式，优先于多层中间件^[raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md]
- **REPL 主循环**：Read-Eval-Print Loop 交互模式，支持多轮连续交互

### 最佳实践

1. 中间层越薄，调试越容易，对 API 行为的控制越精确
2. 错误处理需区分可恢复和不可恢复异常
3. Hook 机制应支持异步执行，避免阻塞主循环

## 三个维度的协同关系

三个维度相互依赖：Prompt 定义 Agent "知道什么"，Context 决定 Agent "记住什么"，Harness 保障 Agent "能做什么"。在实际系统中，Prompt 层的配置文件变更需要 Context 层的记忆系统配合更新，而 Harness 层的工具调用结果又会反馈到 Context 层。这种协同关系使得三维框架成为分析、设计和优化 Agent 系统的系统性方法论。^[raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md]

## 延伸阅读

- [[harness-engineering-deep-dive]] — Harness 深层定义、七层模型、三种工程取向对比、[[claude-code]] 实现细节、上下文密度最大化与 [[generic-agent]] 实践
- [[harness-engineering-cases-and-evolution]] — 企业级落地案例（阿里云 Aegis、腾讯 LEGO）、四阶段演化框架、OpenAI 实践

## 易点天下六层上下文体系

易点天下在实践中构建了更为精细的六层上下文体系，超越了传统的短期/长期记忆二元划分：^[raw/articles/2026-05-09-yidian-context-engineering.md]

| 层级 | 名称 | 说明 |
|------|------|------|
| L1 | 会话记忆 | 当前对话的实时上下文 |
| L2 | 短期记忆 | 近期任务的临时状态和中间结果 |
| L3 | 长期记忆 | 跨会话持久化的知识和经验 |
| L4 | 知识图谱 | 结构化的领域知识和实体关系 |
| L5 | 个人经验 | 针对特定用户的行为模式和偏好 |
| L6 | 组织技能 | 组织级沉淀的最佳实践和标准化流程 |

与 Hermes 五层记忆架构相比，增加了"知识图谱"（L4）和"组织技能"（L6）两个维度，更适应企业级 Agent 场景。

## 主动注入钩子

易点天下提出了"主动注入钩子"机制——在 Agent 生命周期的关键节点（如任务开始、上下文即将溢出、遇到失败重试）主动注入相关上下文信息，而非等待用户手动提供。这提升了 Agent 的自主性和连续性。^[raw/articles/2026-05-09-yidian-context-engineering.md]

## Token 三级分层预算治理

为解决 [[token-roi]] 问题，易点天下提出了 Token 三级分层预算治理体系：^[raw/articles/2026-05-09-yidian-context-engineering.md]

| 级别 | 名称 | 说明 | 预算控制 |
|------|------|------|----------|
| L0 | 核心指令 | 系统提示词、任务定义、关键约束 | 严格限制，最高优先级 |
| L1 | 任务上下文 | 当前任务相关的代码、文档、历史 | 按任务动态分配 |
| L2 | 扩展知识 | 背景资料、参考文档、组织知识 | 按需加载，可压缩/裁剪 |

每一级都有明确的 Token 预算上限，超出预算时自动触发压缩或裁剪策略。

## 上下文工程四原语

易点天下将上下文工程抽象为四个基本操作原语：^[raw/articles/2026-05-09-yidian-context-engineering.md]

1. **Write**：将新信息写入上下文（记忆写入、状态更新）
2. **Select**：从可用信息中选择最相关的子集注入上下文
3. **Compress**：将冗余或低优先级信息压缩为更紧凑的表示
4. **Isolate**：将特定任务的上下文隔离，避免跨任务污染

这四个原语组合起来可以描述所有上下文管理操作，为构建标准化的上下文管理引擎提供了理论基础。

## 相关概念

- [[token-roi]] — Token 投资回报率
- [[ai-engineer-roadmap]] — AI 工程师路线图
- [[agentic-engineering]] — Agent 工程方法论
- [[claude-code]] — 上下文工程的最佳实践载体
