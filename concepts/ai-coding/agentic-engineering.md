---
title: Agentic Engineering
created: 2026-05-05
updated: 2026-05-14
type: concept
tags:
  - agent-framework
  - coordination-engineering
  - practice
sources:
  - raw/articles/2026-05-05-karpathy-agentic-engineering.md
  - raw/articles/2026-05-05-vibecoding-to-agentic-engineering.md
  - raw/articles/2026-05-05-claude-code-engineering-truth.md
  - raw/articles/2026-05-08-veteran-developer-agent-journey.md
  - raw/articles/2026-05-08-agent-eval-ai-coding-310k-lines.md
  - raw/articles/2026-05-09-agent-era-productivity-paradox.md
  - raw/articles/2026-05-09-ai-engineer-roadmap-2026.md
  - raw/articles/2026-05-09-yidian-context-engineering.md
---

# Agentic Engineering

**Agentic Engineering** 是 [[karpathy]] 于 2026 年在 Sequoia 访谈中提出的概念，定义为：**在设计、协调和监督 Agent 的同时，保住质量、安全与责任**。

## 核心定义

与 [[vibecoding]] 互补的"天花板"实践。Vibe Coding 降低门槛，Agentic Engineering 保证上限。

## 核心要素

1. **明确的规格（Spec）** — 定义 Agent 的输入/输出/约束
2. **可验证的输出** — 每一步都可检查，而非信任黑箱
3. **安全的执行环境（Harness）** — 沙箱、权限控制、回滚机制
4. **多 Agent 协调** — 子 Agent 分工、通信、冲突解决

## Martin Fowler 公式

> Agent = Model + Harness

这一公式与现有的 [[harness-engineering-deep-dive]] 和 [[coordination-engineering]] 概念高度吻合。Harness 是连接原始模型能力与可靠工程实践的桥梁。

## 与现有概念的关系

Agentic Engineering 整合了多个已有的工程实践：

- [[harness-engineering-deep-dive]] — 构建 Agent 的安全执行环境
- [[coordination-engineering]] — 多 Agent 协调与编排
- [[prompt-context-harness-engineering]] — Prompt 上下文的工程化管理
- [[spec-driven-development]] — 用规格约束 Agent 行为
- [[agent-technical-debt]] — 管理 Agent 系统的长期维护成本

## 实践原则

- **不要完全信任 Agent 输出** — 每个关键步骤都需要验证
- **为 Agent 设计 Harness** — 限制权限、提供工具、设置边界
- **渐进式自动化** — 从辅助到自主，逐步增加 Agent 自主度
- **可观测性优先** — 记录 Agent 决策过程，便于调试和审计

## 脚手架 > 模型

[[zhiyuanfu]] 提出的反直觉原则：**模型升级成本 +300% 效果 +20%，脚手架升级成本 +50% 效果 +200%。** 优先投资脚手架（SDD 流程 + 调度层 + 失败切换 + constitution.md），而不是追最新最贵的模型。一个设计精良的系统让弱模型发挥惊人性能，烂系统完全浪费顶级模型的能力。^[raw/articles/2026-05-08-veteran-developer-agent-journey.md]

## 生产级 Observability 六维度

从 demo 到系统，至少需要看得见：^[raw/articles/2026-05-08-veteran-developer-agent-journey.md]

1. **目标**：当前目标是什么
2. **步骤**：正在执行哪一步
3. **工具**：用了什么工具
4. **失败**：为什么失败
5. **恢复**：是否触发了重试 / 回退 / 切换
6. **成本**：本轮 token / 时间 / 成本消耗

## Agent 系统五层地基

zhiyuanfu 总结的 Agent 系统基础层次：

1. **目标表达** — 到底想完成什么
2. **能力单元** — 有哪些 Skill / 工具 / 工作流
3. **运行时状态** — 当前正在做什么
4. **治理边界** — 允许做什么，不允许做什么
5. **评估反馈** — 哪些行为值得固化，哪些必须修正

> 垃圾的思考乘以强大的模型，等于精美的垃圾。

## 相关概念

- [[vibecoding]] — 降低门槛的互补概念
- [[claude-code]] — 典型支持 Agentic Engineering 的工具
- [[codex]] — OpenAI 的编程 Agent
- [[agent-first-design]] — 以 Agent 为核心的设计理念
- [[agent-orchestrator]] — Agent 编排器
- [[ai-customer-service]] — Agentic Engineering 在客服领域的典型落地场景

## Agent 时代的生产力悖论

向邦宇指出，仅引入 AI 工具而不改变组织形态会导致 [[productivity-paradox|生产力悖论]]——传统协作流程（代码审查、PR 合并、人工测试）成为 Agent 产能的瓶颈。Agentic Engineering 不仅需要技术层面的 Harness，还需要组织层面的 All-in-Code 版本管理理念和 Agent IAM 身份管理。^[raw/articles/2026-05-09-agent-era-productivity-paradox.md]

## All-in-Code 版本管理理念

向邦宇提出"All-in-Code"理念：将所有工作产物（代码、文档、配置、流程定义）统一纳入代码版本管理，使 Agent 可以完整理解和操作整个工作流。这要求组织从"人驱动流程"转向"代码驱动流程"。^[raw/articles/2026-05-09-agent-era-productivity-paradox.md]

## 1500 行 mini-harness 构建实践

[[codejunkie99]] 在 [[ai-engineer-roadmap]] 中提出，用约 1500 行代码构建 mini-harness 即可显著提升 Agent 表现——同一模型在不同 harness 下结果差异巨大（Opus 4.5 在 Claude Code 78% vs Smolagents 42%）。这印证了 Martin Fowler 的公式 Agent = Model + Harness 中 Harness 的决定性作用。^[raw/articles/2026-05-09-ai-engineer-roadmap-2026.md]

## 六层上下文体系

易点天下在实践中构建了六层上下文体系：会话记忆 → 短期记忆 → 长期记忆 → 知识图谱 → 个人经验 → 组织技能。这一体系与 Hermes 的五层记忆架构相呼应，但增加了"组织技能"维度，强调组织级知识沉淀对 Agent 效能的重要性。详见 [[prompt-context-harness-engineering]]。^[raw/articles/2026-05-09-yidian-context-engineering.md]

## Agent Loop 自主架构

Agent 从被动响应进化为自主执行的 Loop 架构：定时触发 → 自主规划 → 执行验证 → 结果归档。[[boris-cherny]] 在 Sequoia 访谈中强调这是 Claude Code 的未来方向，也是 Agentic Engineering 的终极形态——Agent 不仅完成单个任务，还能持续自主运行和改进。^[raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md]
