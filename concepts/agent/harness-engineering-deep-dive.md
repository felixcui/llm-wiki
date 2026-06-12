---
title: Harness Engineering 深度解析
created: 2026-04-26
updated: 2026-05-15
type: concept
tags: [agent-framework, inference, data]
sources:
  - raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md
  - raw/articles/2026-04-22_从玩具到生产力：用真实项目讲透 AI Agent 的 Harness Engineering.md
  - raw/articles/2026-04-22_深度解析ClaudeCode在PromptContextHarness的设计与实践.md
  - raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md
  - raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md
  - raw/articles/2026-04-27_智能体工程的隐形技术债：10分钟造出一个Agent，公司却要为它养一个平台团队.md
  - raw/articles/2026-04-29-ai-delete-database.md
  - raw/articles/2026-04-29-ai-agent-memorystack.md
  - raw/articles/2026-05-14_从零设计生产级Multi-AgentHarness：架构、评估、记忆、成本与MCP工具接入全拆解.md
  - raw/articles/2026-05-14_别让AI瞎猜了：用HarnessEngineering终结无限返工.md
  - raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md
  - raw/articles/2026-04-30_深入浅出HarnessEngineerring之核心模式与理念.md
  - raw/articles/2026-05-07-agents-md-guide.md
  - raw/articles/2026-05-07-agent-employee-new-player.md
---

# Harness Engineering 深度解析

> 本文是 [[prompt-context-harness-engineering]] 三维框架的深度补充，聚焦 Harness 维度的工程细节。

## Harness Engineering 的深层定义

### Martin Fowler 的定义

Martin Fowler 在 2026 年 4 月的文章中，将 Harness Engineering 直接定义为**围绕 Coding Agent 的信任建设模型**——核心是通过上下文、约束、反馈回路和工程结构，让人逐步敢把任务交给 Agent。Anthropic 也在官方工程文章中直接把 Claude Code 称为一种优秀的 Harness 实现。^[raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md]

### 管的是"非确定性"

传统软件工程管的是「确定性」（为人类设计的防呆系统），而 Harness Engineering 管的是「非确定性」——大模型是概率引擎，同样的输入可能返回不同结果。Harness 不是泛泛的"好习惯"，它是为了把一台「聪明但没有工程常识的非确定性引擎」嵌进「确定的业务流水线」而设计的物理控制面。^[raw/articles/2026-04-22_从玩具到生产力：用真实项目讲透 AI Agent 的 Harness Engineering.md]

## Harness 七层模型

将 Harness 拆解为七个层次，每一层都有明确的工程目标：^[raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md]

| 层次 | 名称 | 核心目标 |
|------|------|----------|
| 1 | 角色与规则 | 确定身份、边界和职责 |
| 2 | 记忆系统 | 任务过程留痕，下次能接上 |
| 3 | 上下文加载 | 每轮只给模型当前最需要的 |
| 4 | 稳定执行 | 把语言判断变成真实动作 |
| 5 | 有效循环 | 持续推进不空耗 |
| 6 | 评分与可观测 | 不让模型自己给自己打高分 |
| 7 | 中断修复 | 把断掉的任务重新接起来 |

核心洞察：看得太少像失忆，看得太多开始变蠢——第三层（上下文加载）是所有模型工程最难的点。

## AGENTS.md：Harness Engineering 的项目配置实践

[[agents-md]] 是 Harness Engineering 在项目配置层面的标准化落地形式。从演进角度看，它经历了 CLAUDE.md（Anthropic 专属）→ 碎片化配置（`.cursorrules`、`.windsurfrules`）→ 统一标准 AGENTS.md 的过程，目前已有超过 60,000 个项目采用。^[raw/articles/2026-05-07-agents-md-guide.md]

AGENTS.md 本质上是 Harness 七层模型中第一层（角色与规则）和第三层（上下文加载）的轻量级实现：

- **角色与规则**：通过项目概述、编码约定、约束与限制等章节定义 Agent 的行为边界
- **上下文加载**：通过架构描述、常用命令、参考资料等章节为 Agent 提供导航"地图"

核心理念是 **"Map, not Manual"**——AGENTS.md 提供项目导航地图，而非详尽的操作手册。这与 Harness Engineering 中"为非确定性引擎设计物理控制面"的理念高度一致。详见 [[agents-md]] 和 [[claude-code-practice]]。

## Harness for Humans vs Harness for AI

2026 年的 AI 员工竞赛中出现了一个重要的范式区分：^[raw/articles/2026-05-07-agent-employee-new-player.md]

| 维度 | Harness for AI | Harness for Humans |
|------|---------------|-------------------|
| 设计中心 | 以 AI 为中心 | 以人为中心 |
| 适应方向 | 人类适应 AI | AI 适应人类 |
| 自主性 | AI 高自主 | AI 作为增强工具 |
| 代表产品 | [[claude-code]] Managed Agents | [[genspark]] 4.0 |

这一区分对 Harness Engineering 的实践方向有深远影响：

- **Harness for AI** 强调构建更强大的 Agent 运行环境，让 AI 能独立完成更多任务——[[managed-agents]] 是这一方向的代表
- **Harness for Humans** 强调让 AI 融入人类已有的工作流程，降低使用门槛——[[genspark]] 和 [[ai-employee]] 生态中的 WorkBuddy、悟空、aily、DuMate 等产品更偏向这一方向

两种范式并非互斥，但产品设计的选择会影响整个产品体验。Harness Engineering 的深层问题在于：**我们到底是在为 AI 建造更舒适的"驾驶舱"，还是在为人类建造更智能的"副驾驶"？** 不同的回答导向不同的工程优先级。

## 子页面

- [[harness-engineering-patterns]] — 六种核心模式（持久化指令文件、作用域上下文组装、分层记忆、做梦整理、渐进式上下文压缩、工作流与编排）
- [[harness-engineering-implementations]] — 工程取向对比、基础设施映射、Claude Code P/C/H 细节、Safety Failure 案例、Generic Agent、SkVM、Brain 记忆系统
- [[harness-engineering-cases-and-evolution]] — 企业级落地案例与四阶段演化框架

> 返回 [[prompt-context-harness-engineering]]
