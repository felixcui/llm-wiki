---
title: Sub-Agent vs Agent Team
created: 2026-04-25
updated: 2026-04-28
type: concept
tags: [agent-framework, coordination-engineering, architecture]
sources:
  - raw/articles/2026-04-25-sub-agent-vs-agent-team.md
  - raw/articles/2026-04-27_我用多Agent搭了一家跨境电商公司，起飞！.md
  - raw/articles/2026-04-28-moxt-agent-team.md
  - raw/articles/2026-04-28-subagents-clean-context.md
---

# Sub-Agent vs Agent Team（子智能体 vs 智能体团队）

复杂任务不应本能选择多智能体，而要先判断所需协作机制。Sub-agents 适合隔离、并行、压缩明确任务结果（信息流可控），Agent teams 适合需要持续上下文、成员通信、任务依赖和动态调整的真实协作场景。二者决定截然不同的系统架构。^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

## Sub-Agents（子智能体）：带隔离的并行化

Sub-agent 是一个在**独立隔离上下文**中运行的专门化实例，本质是"委派"。^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

每个 sub-agent 拿到：

- 定义其角色的 system prompt
- 受限的工具集合
- 完全隔离的上下文
- 边界清晰的单一任务

执行结束后只返回最终输出，**不带回推理过程或中间步骤**。核心价值不只是"更快"，更在于"**压缩**"——把混乱的探索过程压缩成一个干净的信号。

严格限制：^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

- Sub-agents 之间**不能互相交流**
- Sub-agents **不能再生成新的 agents**
- 所有信息流**必须经过父级 agent**

> 实际案例：[[claude-agent-sdk]] 的 `AgentDefinition` 中，`description` 字段本质上承担 routing signal 的作用，决定任务委派给哪个 sub-agent。^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

## Agent Teams（智能体团队）：通过通信实现协作

Agent teams 为"协作"而设计：一组能够**持续持有上下文、彼此沟通、实时调整**的 agents。^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

典型组成：

- **Lead agent**：负责分配任务与综合结果
- **Teammates**：执行具体任务
- **共享任务层**：跟踪进度和依赖关系

这让真正的协作成为可能——例如 frontend agent 可以把 backend 变更即时同步出去，相关内容立刻更新。

> 与 [[coordination-engineering]] 中描述的 Agent Team 概念一致：Leader 智能编排 + 成员自主执行 + 共享工作区协同。

## 核心差异对比

| 维度 | Sub-Agents | Agent Teams |
|------|-----------|-------------|
| 关注点 | **执行** | **协作** |
| 上下文 | 隔离的、独立的 | 持续存在的、共享的 |
| 状态 | 无状态、一次性 | 有状态、可交互 |
| 控制流 | 父级集中控制 | 点对点协作 |
| 通信 | 只能通过父级 | 成员间直接通信 |
| 适用场景 | 任务彼此独立 | 任务之间彼此依赖 |

^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

## 设计原则

### 按"上下文"拆分而非按"角色"拆分

很多系统喜欢按角色拆分（planner、developer、tester），这种拆法在每次交接中制造**上下文丢失**——implementer 不知道 planner 当时掌握了什么，tester 不知道 implementer 做了哪些关键决策，质量在每一道边界上持续下滑。^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

正确的问题是：**这个任务到底真正需要哪些信息？** 如果两个任务共享了大量深度上下文，就让它们留在同一个 agent 里。只有当上下文能够被干净切开时，才去拆分。

### 先简单后复杂

围绕上下文边界来设计，先从简单系统开始，只有在真正需要时才把复杂度加上去。^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

## Multi-Agent 的 5 种模式

1. **Prompt chaining**：顺序步骤，前一步输出作为后一步输入
2. **Routing**：将任务送到正确的 agent
3. **Parallelization**：彼此独立的工作并行执行
4. **Orchestrator–worker**：一个 agent 负责委派，多个 worker 执行（类似 sub-agent 模式）
5. **Evaluator–optimizer**：先生成，再迭代优化

^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

## 何时使用 / 不使用 Multi-Agent

**适合使用：**^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

- 需要上下文隔离（避免一个任务的噪音污染另一个）
- 有可以并行执行的任务
- 确实需要专门化分工

**不适合使用：**^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

- Agents 之间高度互相依赖
- 协调成本过高（超过了多 agent 带来的收益）
- 任务本身其实很简单

## 生产实践：CodeBanana PM Agent 路由模式

CodeBanana 跨境电商案例中的 PM Agent 是 sub-agent 架构在真实业务中的典型实现：^[raw/articles/2026-04-27_我用多Agent搭了一家跨境电商公司，起飞！.md]

**架构模式：** PM Agent 作为父级 orchestrator，4 个专职 Agent（Coding、选品、设计、社媒）作为 sub-agents 运行。每个 Agent 拥有独立上下文、技能和工具，信息流通过 PM Agent 中转。

**路由机制：** PM Agent 维护委派路由表（TEAMS.md），记录每个 Agent 的职能和 project_id。用户提出需求后，PM Agent 判断任务类型并自动委派给对应 Agent——这与本文"5 种模式"中的 **Orchestrator–worker** 模式完全一致。

**并行执行实例：** PM Agent 接到社媒推广包任务后，同时委派社媒运营 Agent 和产品设计 Agent 并行执行，汇总后输出完整推广包。这体现了 sub-agent 模式中"彼此独立的工作并行执行"（**Parallelization** 模式）与 orchestrator–worker 模式的组合运用。

**该案例也验证了核心设计原则：** 5 个 Agent 按职能（上下文能被干净切开）而非按角色名称拆分，选品、开发、设计、社媒各自拥有独立的信息域。Agent 间不直接通信，所有协调通过 PM Agent 完成，符合"所有信息流必须经过父级 agent"的 sub-agent 限制。详见 [[ai-native-organization]] 和 [[coordination-engineering]]。

## 生产实践补充

### Moxt：角色化 Agent 团队 ^[raw/articles/2026-04-28-moxt-agent-team.md]

Moxt 平台展示了 Agent Team 模式的实战落地：创建专门化的 AI "同事"（设计总监、研究员、视觉设计师、甚至"唱反调"角色），通过**跨审查**（cross-review）让"魔鬼代言人"Agent 发现其他 Agent 输出中的盲点。Agent 间通过飞书集成共享，实现了从单 Agent（OpenClaw）到多 Agent 团队的演进。

### Claude Code Subagent 实现 ^[raw/articles/2026-04-28-subagents-clean-context.md]

Claude Code 提供了 sub-agent 模式的生产级实现：
- **内置 subagents**：Explore（代码搜索，不污染主上下文）和 Plan（架构研究 + 分步计划）
- **Context forking**：`CLAUDE_CODE_FORK_SUBAGENT=1` 继承父级上下文但保持隔离，共享 prompt cache 前缀，输入 token 成本降低约 10 倍
- **自定义 subagents**：通过 `.claude/agents/` 目录定义，每个 subagent 拥有独立 system prompt、工具集和权限
- **上下文隔离**：subagent 在独立窗口运行，仅返回摘要给父级，防止 80K+ token 噪音累积

## 与其他概念的关系

- [[hermes-agent]] 的 `delegate_task` 机制本质上是 sub-agent 模式：父级委派任务、子级隔离执行、结果返回父级
- [[claude-agent-sdk]] 的 `AgentDefinition` API 是 sub-agent 模式的编程接口
- [[coordination-engineering]] 的 Agent Team 对应本文的 agent team 概念，在其基础上增加了 Team Skills 经验沉淀机制
- [[multi-agent-code-review]] 的 ultrareview 是 sub-agent 模式的典型应用：3 Explorer 并行独立审查 + 1 Critic 交叉验证
- [[agent-cluster]] 的 300 子 Agent 集群是 sub-agent 大规模并行化的极端案例
