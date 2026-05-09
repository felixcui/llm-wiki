---
title: Goal-Driven Agent
created: 2026-05-08
updated: 2026-05-08
type: concept
tags:
  - agent-framework
  - coordination-engineering
  - practice
sources:
  - raw/articles/2026-05-08-veteran-developer-agent-journey.md
---

# Goal-Driven Agent

**Goal-Driven** 是相对于 Task-Driven 的 Agent 系统设计范式跃迁：从人持续派活执行任务，到 Agent 围绕目标自主推进。由 [[zhiyuanfu]] 在构建"24h 打工人"系统的实践中提炼。

## Task-Driven vs Goal-Driven

| 维度 | Task-Driven | Goal-Driven |
|------|-------------|-------------|
| 人的角色 | 项目经理 + 执行监督 | 目标设定者 / 审核者 |
| Agent 的角色 | 执行器 | 自主推进者 |
| 决策中心 | 在人脑子里 | 在目标 + 边界 + 系统状态里 |
| 主要成本 | 人持续编排 | 前期建模和约束设计 |
| 适用场景 | 简单、一次性任务 | 长期、复杂、持续推进任务 |

核心区别：**Task-Driven 解决执行问题，Goal-Driven 解决迭代问题。** Task-Driven 让系统开始能跑，Goal-Driven 让系统开始能持续向前。^[raw/articles/2026-05-08-veteran-developer-agent-journey.md]

## Goal-Driven 的五个前提

1. **目标必须清晰** — 不是模糊愿望，而是可推进、可判断的目标表达
2. **边界必须清晰** — 哪些能做，哪些不能做，资源上限是什么
3. **状态必须可见** — 当前做到哪一步，卡在哪，为什么卡
4. **过程必须留痕** — 否则无法知道为什么成功/失败
5. **权限必须可控** — Agent 能调用哪些工具，能写到哪里，谁来兜底

> Goal-Driven 不是更放权，而是更强约束下的有限自治。

## 共享状态机制

Goal-Driven 多步骤、多角色协作中，主 Agent 本身会变成新瓶颈（上下文越来越重、单点故障）。更务实的做法是用共享状态文件（如 `STATE.yaml`）协调：

- 每个 Agent 自己读取状态、写回进度
- 主会话只负责高层目标和验收
- 主会话负责方向，不负责搬运；系统负责推进，不靠人反复传话

## 落地路径

必须建立在成熟的 Task-Driven 基础上，按顺序推进：

1. 写清楚 [[spec-driven-development|spec]]
2. 执行过程留痕（Prompt / 状态 / 输出 / 错误全记录）
3. 补 Observability 和 Eval
4. 高频动作沉淀为 Skill
5. 引入调度和并发
6. 最后才尝试 Goal-Driven

> 先让一次执行可复盘，再让它可重复，再让它可规模化，最后让它可有限自主。

## 核心洞察

**"24h 在线，不等于 24h 迭代"** — 只要任务还需要人持续供给，人就仍然是系统的瓶颈。真正的跃迁不是让 AI 多做几个步骤，而是让人退出微观调度。

## 与相关概念的关系

- [[agentic-engineering]] — Goal-Driven 是 Agentic Engineering 的高级实践形态
- [[spec-driven-development]] — Goal-Driven 的地基，spec → plan → tasks 的标准路径
- [[agent-technical-debt]] — Goal-Driven 系统需要更强的治理基础设施
- [[coordination-engineering]] — 多 Agent Goal-Driven 协作需要协调工程支撑
- [[managed-agents]] — Claude 的 Managed Agents 是 Goal-Driven 的一种实现
- [[self-improving-agent]] — Agent 自主推进 + 持续改进
