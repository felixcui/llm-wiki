---
title: Agent Orchestrator
created: 2026-04-29
updated: 2026-04-29
type: concept
tags: [organization, agent-framework]
sources:
  - raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md
---

# Agent Orchestrator

**Agent Orchestrator（Agent 编排者）** 是一类不写代码、但设计和管理 AI Agent 的新角色。这个术语由 [[choco]] 在 2025-2026 年间明确提出，标志着组织从 workflow software 向 AI execution infrastructure 的转型中产生的全新岗位。^[raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md]

## 核心理念

> 操作 agent 的人，正在替代写脚本的人。

Agent Orchestrator 的核心工作是：
- **设计 Agent 工作流**：定义 Agent 的职责边界、触发条件、决策逻辑
- **管理 Agent 行为**：配置 Agent 的参数、工具、上下文源
- **监控 Agent 质量**：通过 evaluation framework 持续度量 Agent 输出的准确性和可靠性
- **迭代 Agent 能力**：根据生产数据调整 Agent 的提示、工具选择和决策策略

## 与现有角色的区别

| 角色 | 核心技能 | 产出 |
|------|----------|------|
| 传统工程师 | 编程 | 脚本/代码 |
| 提示词工程师 | Prompt 写作 | 提示词模板 |
| **Agent Orchestrator** | 系统设计 + Agent 行为理解 | Agent 工作流和编排逻辑 |

Agent Orchestrator 不直接写代码，但需要理解：
- Agent 的能力边界和限制
- 不同模型（语音、文本、图像）的适用场景
- 隐式上下文消歧的工程挑战
- 概率系统的质量评估与预期管理
- 工具链的编排与组合

## 与 [[ai-native-organization]] 的关系

Agent Orchestrator 是 [[ai-native-organization]] 理念的自然产物：
- 组织中 Agent 数量可能超过员工 5-10 倍，需要专门的角色来管理和编排
- 人类从「执行者」转变为「Agent 管理者」——与 AI Native 组织中人类角色的转变一致
- Agent Orchestrator 对应 [[ai-native-organization]] 中「AI Founder Type」员工原型的一个具体细分——擅长 AI 编排的复合型人才

## 实践背景

[[choco]] 在其 OpenAI 案例研究中明确了这一角色定位：Choco 正在将 Agent 推进销售、电商、供应链等更多业务流，需要非工程师人员来设计和管理 Agent。这标志着从「工程师写脚本实现自动化」到「业务人员编排 Agent 实现自动化」的范式转移。^[raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md]

## 相关条目

- [[choco]] — 首次明确提出 Agent Orchestrator 角色的公司
- [[ai-native-organization]] — Agent Orchestrator 作为 AI Native 组织的关键角色
- [[agent-first-design]] — Agent 优先设计理念，为 Agent Orchestrator 提供设计原则
- [[agent-technical-debt]] — Agent 生产化的基础设施挑战，Agent Orchestrator 需要面对的治理问题
