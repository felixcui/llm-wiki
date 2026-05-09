---
title: Coordination Engineering
created: 2026-04-25
updated: 2026-04-28
type: concept
tags: [coordination-engineering, agent-framework]
sources:
  - raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md
  - raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md
  - raw/articles/2026-04-25-sub-agent-vs-agent-team.md
  - raw/articles/2026-04-27_智能体工程的隐形技术债：10分钟造出一个Agent，公司却要为它养一个平台团队.md
  - raw/articles/2026-04-27_我用多Agent搭了一家跨境电商公司，起飞！.md
  - raw/articles/2026-04-28-yc-claude-engineering-team.md
  - raw/articles/2026-04-27-multica-interview.md
---

# Coordination Engineering（协调工程）

从单 Agent "驾驭与治理"到多 Agent "协同与进化"的工程范式跃迁，由 openJiuwen 社区（华为支持）在 [[openclaw]] 生态中率先提出，涵盖 Agent Team（多智能体协作）和 Team Skills（协作能力标准化沉淀）两大核心组件。^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]

## 从单 Agent 到多 Agent 协作

单 Agent 模式下，用户面对的是一个智能体完成所有任务。随着任务复杂度提升，单 Agent 在角色分工、并行执行、专业深度等方面面临瓶颈。Coordination Engineering 的核心目标是让多个 Agent 像精锐团队一样协同工作，同时让协作经验可沉淀、可复用。

### Agent Team：多智能体实时协作

Agent Team 解决的是"当下怎么协作"的问题：

- **Leader 智能编排**：主 Agent 负责任务拆解、角色分配和流程管控
- **成员自主执行**：各子 Agent 按分配的任务独立工作
- **共享工作区协同**：Agent 间通过共享文件系统或消息总线交换信息^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]
- **全生命周期管控**：从任务启动到交付验收的完整流程管理

子 Agent 管理在工程实现上需要薄抽象和显式控制流。[[openclaw]] 的实现采用单进程 Node.js 架构，通过 SubagentManager 管理子 Agent 的创建、通信和销毁，避免引入多层中间件导致的调试困难。^[raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md]

### Team Skills：协作经验标准化沉淀

Agent Team 的核心痛点是：会话结束后协作经验全部消失，每次遇到同类任务都要从零规划。Team Skills 将一次成功协作的完整链路封装为标准化能力包：

- **需求拆解**：如何将复杂目标分解为可执行的子任务
- **角色分工**：需要哪些角色、各自负责什么
- **任务分配**：谁先谁后、哪些环节可并行
- **通信机制**：Agent 间如何传递信息和状态
- **冲突处理**：遇到分歧或错误时的处理策略
- **交付规范**：什么条件算完成、输出格式要求^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]

## Team Skills 的技术规范

Team Skills 本质上是一个**文件夹目录结构**，每个 Skill 包含：

| 文件 | 用途 |
|------|------|
| `SKILL.md` | 团队名称、目标、成员构成 |
| `roles/*.md` | 每个角色的职责定义 |
| `workflow.md` | 执行顺序与协作流程 |
| `bind.md` | 边界定义与异常处理 |
| `dependencies.yaml` | 外部工具依赖 |
| `examples/` | 示例与模板 |

简单任务两三个文件即可，复杂任务按需扩展，几乎没有门槛。^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]

## 动态创建与自动生成

JiuwenClaw 提供了配套的"团队技能自动生成专家"（teamskill-creator），支持：
- 从自然语言描述自动生成 Team Skill
- 将现有单 Agent Skill 转化为 Team Skill
- 修改已有 Team Skill（增减角色、调整流程）

实战案例中，该工具成功生成了具备 23 位 AI 医学专科专家的团队技能，可根据病情动态创建专科专家组进行联合会诊。^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]

## 跨框架兼容

Team Skills 扩展的是 Agent Skills 开放标准，不依赖特定平台。经验证可在 [[claude-code]]、Cursor 等支持多智能体协同的平台上零适配运行。这意味着凡是支持 Agent Skills 标准的平台，都可以直接复用 Team Skills 的协作能力。^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]

## Sub-Agent vs Agent Team 架构选择

Agent Team 是多 Agent 协作的一种模式，但并非唯一选择。在决定是否使用 Agent Team 时，需要先判断任务所需协作机制：

- **Sub-agents**（子智能体）：隔离上下文中运行的专门化实例，适合任务彼此独立的场景。本质是"委派"——返回干净结果，压缩混乱探索过程。信息流必须经过父级，sub-agents 之间不能直接通信。详见 [[sub-agent-vs-agent-team]]
- **Agent teams**（智能体团队）：持续持有上下文、彼此沟通、实时调整的协作模式，适合任务之间彼此依赖的场景

核心判断标准：按"上下文"拆分而非按"角色"拆分。如果两个任务共享大量深度上下文，应留在同一 agent 里；只有上下文能被干净切开时，才拆分。^[raw/articles/2026-04-25-sub-agent-vs-agent-team.md]

## 实战案例：CodeBanana 多 Agent 跨境电商协作

博主苍何在 CodeBanana（出门问问产品）上搭建了一个多 Agent 协作的跨境电商公司，结合了 A2A 协议和 PM Agent 路由机制，是 Coordination Engineering 的一个完整生产级实践。^[raw/articles/2026-04-27_我用多Agent搭了一家跨境电商公司，起飞！.md]

**协作架构：** PM Agent 作为 Team Agent 承担 Leader 智能编排角色，负责需求接收→拆解→委派→聚合→输出。4 个专职 Agent（Coding、选品、设计、社媒）各自拥有独立上下文、技能和工具，通过 PM Agent 的委派协议进行协作。PM Agent 维护委派路由表（TEAMS.md），自动判断任务类型并路由到对应 Agent。

**并行协作实例：** PM Agent 接到"为 NeuralGlide Pro 生成社媒推广包"任务后，自动拆解为两个子任务并行执行——社媒运营 Agent 输出多平台文案，产品设计 Agent 生成推广配图——最终由 PM Agent 汇总为完整推广包。整个过程约 2 分钟，覆盖了 Team Skills 描述的需求拆解、角色分工、任务分配和交付规范全链路。

**与框架概念的对应：** 该案例中 PM Agent 的路由机制对应 Agent Team 的 Leader 智能编排；Agent 间通过 PM Agent 中转而非直接通信，更接近 [[sub-agent-vs-agent-team]] 中的 sub-agent 委派模式；Agent 以"团队成员"身份出现在组织通讯录中，人与 Agent 共享同一协作空间，体现了 AI Native 组织理念（详见 [[ai-native-organization]]）。

## 生产化挑战：编排与治理

当 Coordination Engineering 从原型走向生产，编排和治理成为核心瓶颈：^[raw/articles/2026-04-27_智能体工程的隐形技术债：10分钟造出一个Agent，公司却要为它养一个平台团队.md]

### 编排（Orchestration）

多 Agent 工作流混合了 Agent、工具和人，编排债务不在个别步骤而在节点之间：

- **路由错误**：分诊 Agent 误判根本原因（如将数据库超时误判为部署问题），导致不必要的回滚，而真正问题未解决。工作流"自信地执行了错误操作"——这不一定是工作流停止，而是做了错误的事情且难以追踪
- **非确定性挑战**：传统工作流可测试每条路径（确定性），Agent 工作流引入非确定性后路径不可预测。Agent 之间用自然语言传递信息，"接口"模糊——一个 Agent 的模型更新可能悄无声息地破坏链路下游
- **所有权模糊**：工作流跨三个团队时，谁对结果负责？单个步骤故障容易排查，但找出三步前哪个决策导致工作流走错路径——这需要大多数组织尚未建立的追踪机制

### 治理（Governance）

多 Agent 协作放大了治理需求：

- 集中策略执行：发现漏洞时需从一个地方禁用工具，自动影响所有 Agent
- 审计追踪：区分 Agent 行为和人类行为（本地创建的 Agent 继承创建者凭证，无法区分）
- 成本治理：陷入重试循环的 Agent 持续消耗 Token，需按 Agent/团队进行成本分解

这些挑战详见 [[agent-technical-debt]]。

## 实践模式补充

### Multica：人 A 协作新范式 ^[raw/articles/2026-04-27-multica-interview.md]

Multica（12K GitHub stars/周）提出了"人是 AI 团队中最慢的节点"这一核心洞察，其协作模式为：**任务分发 → Agent 自主执行 → 人类审查**。关键指标 **Agent Idle Rate**（Agent 空闲率）衡量团队效率——目标是最小化 Agent 等待人类反馈的时间。该平台支持 Claude Code、Codex、OpenClaw、[[hermes-agent]] 等 runtime，以 Issue 为粒度进行任务分发。详见 [[multica]]。

## 实践模式：并行 PR 工厂

### "Thin Harness, Fat Skills"（薄脚手架，胖技能）

Gary Tan（YC CEO）提出这一核心理念：外层框架应尽量薄，真正的能力沉到可复用的 Skills 里。[[claude-code]] 的正确用法不是"更强的单个助手"，而是"需要角色、流程和评审的工程团队"。GStack 框架就是这一理念的实现——把复杂度放在 Skills 中而非堆一个笨重外壳。^[raw/articles/2026-04-28-yc-claude-engineering-team.md]

### 并行 Work Item 模式

Gary Tan 同时运行 10-15 个 [[claude-code]] 会话，每个想法/bug report 变成一个独立 worktree：^[raw/articles/2026-04-28-yc-claude-engineering-team.md]

- 每条线都有清楚的边界和交付门槛（Office Hours → CEO review → 工程评审 → 对抗性评审 → 最终 review）
- 并行的前提是每条线独立可追踪（独立分支、独立评审、独立发布门槛）
- 人类不再维护待办清单，而是不断判断哪些 work item 值得继续、哪些应该停下
- 设计阶段也并行（Design Shotgun 一次生成三个方向，人做取舍）

## 与其他框架的关系

- [[hermes-agent]] 的三维设计框架（Prompt/Context/Harness）为单 Agent 提供了系统化工程方法论，Coordination Engineering 则在此基础上扩展到多 Agent 层面
- Agent Team 对应 Harness 维度中的子 Agent 管理能力
- Team Skills 对应 Context 维度中的经验记忆与复用机制
- [[sub-agent-vs-agent-team]] 系统性对比了 sub-agent 与 agent team 两种架构模式的选择依据
- [[agent-technical-debt]] 的编排和治理模块揭示了 Coordination Engineering 生产化时必须解决的基础设施挑战
