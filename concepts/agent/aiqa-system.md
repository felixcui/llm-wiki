---
title: AIQA智能测试体系
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [tool, agent-framework, ai-coding]
sources:
  - raw/articles/2026-04-28-youmanju-ai-workflow.md
---

# AIQA智能测试体系

AIQA 智能测试体系是 [[youmanju|柚漫剧]] 团队在测试环节构建的 AI Agent 测试框架，实现了从 Copilot 到 AI Agent 的范式转移——人从「执行者」变为「监督者/调教者」，LLM 成为具备独立行动能力的 AI QA。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 架构设计

### 两大角色 + 六大专业 Agent

- **主 Agent**：统筹调度，接收需求并分发任务
- **数字员工「度小智」**：固定角色数字员工，负责特定领域的持续测试
- **六大专业 Agent**：覆盖不同测试维度的专用 Agent
- **工具生态**：各类测试工具的集成层
- **交付工程记忆**：持续积累的测试知识库^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

### 工作模式

从「人用工具」升级到「人机协同」，LLM 具备以下能力：

- **主动感知**：自动识别需求变更并触发测试
- **持续测试**：打通端到端全流程链路
- **自动拦截**：根据需求准出标准自动拦截未达标需求
- **智能报告**：自动发送提测报告，以如流侧边栏形式展示需求和版本维度风险^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 实践效果

| 维度 | 指标 |
|------|------|
| AIQA 需求落地 | 覆盖率 100% |
| 客户端需求用例生成 | 占比 74% |
| 服务端接口用例生成 | 占比 81% |
| 稳定自动化用例（客户端） | 占比 10% |
| 稳定自动化用例（服务端） | 占比 2% |
| 用例生成时间 | 8 秒 |
| 单步执行时间 | 1 分钟 |

^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## AI 助力测试左移

### AICR（智能代码审查）

在 Commit 和 CR 阶段引入 AI 扫描能力：

- **Commit 阶段**：基于 Agent + Rules 引入 AISA 扫描能力，提前发现静态代码风险；支持 Comate IDE / VSCode / JetBrains 三端
- **CR 阶段**：引入小码哥智能评审，融合 AISA 能力，支持多端多语言扫描
- **经验**：不同业务线扫描规则应用效果不同，需增加知识本和代码库 CR 规则打通功能，支持工程级和个人级 CR 规则配置^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

### AI Checker「茶茬」（建设中）

- **背景**：从走查阶段痛点出发，结合 AI + 算法 + 代码实现 Figma 与图像对比前置召回 UI 问题
- **能力**：支持文本/元素布局/样式/区域类的检测能力，通过 OpenClaw 实现知识与算法的自主进化
- **Skill 化**：能力 Skill 化，支持全流程覆盖，渗透至设计/开发/测试阶段^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 与 Agent 生态的关系

该体系与 [[agent-skill-specification]] 中的 Skill 概念高度一致——AI Checker 的「能力 Skill 化」正是将测试知识封装为可复用任务流程的实践。同时，其「主 Agent + 专业 Agent」的架构设计反映了 [[coordination-engineering]] 中的多 Agent 协作模式。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 局限性

- 客户端稳定自动化用例占比仍较低（10%），复杂交互场景的自动化覆盖有待提升
- 初期 AICR 存在误报率较高的问题，需要针对不同语言持续优化
- AI Checker「茶茬」仍在建设中，能力尚未完全落地^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 交叉引用

- [[youmanju]] — 柚漫剧团队，该体系的提出者和实践者
- [[agent-skill-specification]] — Agent Skill 规范，AIQA 的能力 Skill 化理论基础
- [[coordination-engineering]] — 协调工程，多 Agent 协作架构的理论框架
- [[multi-agent-code-review]] — 多 Agent 代码审查，与 AIQA 的 AICR 实践直接相关
