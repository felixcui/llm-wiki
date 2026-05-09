---
title: Managed Agents
created: 2026-05-05
updated: 2026-05-07
type: concept
tags:
  - agent-framework
  - coordination-engineering
  - practice
sources:
  - raw/articles/2026-05-05-anthropic-hackathon-finalists.md
  - raw/articles/2026-05-07-agent-employee-new-player.md
---

# Managed Agents

**Managed Agents** 是 [[anthropic]] 在 Claude 生态中推出的托管 Agent 功能：一个主 Agent（Manager）管理多个子 Agent，子 Agent 各司其职，形成层级化的 Agent 团队。

## 架构

```
Manager Agent
├── Sub-Agent A（任务 A）
├── Sub-Agent B（任务 B）
└── Sub-Agent C（任务 C）
```

Manager 负责：
- 任务分解与分配
- 子 Agent 之间的协调
- 结果聚合与质量把控
- 长时间会话的目标保持

## 技术实现

[[claude-code]] 的 Managed Agents 使用 [[opus-4.7]] 驱动，具备以下特点：

- **长时间会话不跑偏** — Opus 4.7 的强推理能力确保长期目标一致性
- **子 Agent 隔离** — 各子 Agent 独立执行，互不干扰
- **动态调度** — Manager 可根据任务进展调整子 Agent 分配

## Anthropic 黑客松案例（2026）

Anthropic 举办的黑客松展示了 6 个 Managed Agents 应用：

| 项目 | 奖项 | 领域 | 描述 |
|------|------|------|------|
| MedKit | 金奖 | 医学 | 医学知识数字化，多 Agent 协作诊断 |
| Wrench Board | 银奖 | 汽车 | 汽车维修指导，多步骤推理 |
| Maieutic | 铜奖 | 教育 | 苏格拉底式教育对话 |

## 与相关概念的关系

- [[coordination-engineering]] — Managed Agents 是协调工程的具体实现
- [[agent-orchestrator]] — Manager Agent 本质上是一个编排器
- [[agent-first-design]] — 从设计阶段就考虑多 Agent 架构
- [[agent-skill-specification]] — 子 Agent 的能力可以通过技能规格定义

## 局限性

- Manager 本身可能成为瓶颈
- 子 Agent 数量增加时协调复杂度上升
- 成本随 Agent 数量线性增长
- 调试困难——多 Agent 系统的决策链路复杂

## 相关概念

- [[claude-code]] — 托管 Agent 的运行平台
- [[opus-4.7]] — 驱动 Managed Agents 的模型
- [[anthropic]] — 推出该功能的机构
- [[coordination-engineering]] — 底层工程方法论

## AI 员工竞赛格局

Managed Agents 是更广泛的 [[ai-employee]]（AI 员工）趋势的一部分。2026 年，多家公司推出了以"AI 员工"为定位的产品，形成了激烈的竞赛格局：^[raw/articles/2026-05-07-agent-employee-new-player.md]

| 产品 | 公司 | 特点 |
|------|------|------|
| [[genspark]] 4.0 | GenSpark | "Harness for humans"理念，以人为中心 |
| Claude Managed Agents | Anthropic | 多 Agent 层级管理，Opus 驱动 |
| WorkBuddy | 腾讯 | 腾讯生态深度集成 |
| 悟空 | 钉钉 | 钉钉工作流集成 |
| aily | 飞书 | 飞书文档协同 |
| DuMate | 百度 | 百度 AI 能力整合 |

### Harness for Humans vs Harness for AI

这一竞争格局中一个重要的范式区分是：^[raw/articles/2026-05-07-agent-employee-new-player.md]

- **Harness for AI**（以 AI 为中心）：系统围绕 AI 的能力构建，人类需要适应 AI 的工作方式。代表：传统 Claude Code 等编程 Agent
- **Harness for Humans**（以人为中心）：AI 适应人类的工作流程和习惯，作为增强工具融入现有体系。代表：[[genspark]] 4.0

[[genspark]] 明确选择了"Harness for humans"方向，这与 [[claude-code]] 的 Managed Agents 形成了有趣的设计哲学对比。Managed Agents 更偏向"Harness for AI"——让 AI 自主管理和协调，而 GenSpark 更强调让 AI 融入人类已有的工作方式。

这一对比也与 [[harness-engineering-deep-dive]] 中的核心讨论相关：Harness Engineering 的最终目标究竟是让 AI 更强大，还是让人类更高效？不同的产品选择给出了不同的答案。
