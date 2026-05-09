---
title: Agent-First Design（为 Agent 设计产品）
created: 2026-04-26
updated: 2026-04-26
type: concept
tags: [agent-framework, tool]
sources:
  - raw/articles/2026-04-26-designing-products-for-agents.md
---

# Agent-First Design（为 Agent 设计产品）

将 AI 智能体（而非人类用户）视为主要调用方的产品设计方法论。UI 不会消失，但未来人与软件之间 80% 的交互将通过 AI 智能体完成，产品设计需要相应转型。^[raw/articles/2026-04-26-designing-products-for-agents.md]

## 软件交互范式演进

### 第一阶段：用户直接操作

```
用户 → 界面 → 数据库
```

传统模式下，界面本身就是产品体验的核心。用户打开产品、点来点去、完成任务。

### 第二阶段：用户智能体直连

```
用户 → 用户的 AI 智能体 → 数据库
```

AI 智能体代表用户直接读写数据，界面消失，智能体直接和底层系统对话。

### 第三阶段：双层智能体协作

```
用户 → 用户的 AI 智能体 → 软件自己的 AI 智能体 → 数据库
```

软件公司设计自己的智能体，承接业务逻辑、落实规则、补充调用方智能体缺失的上下文。两个 LLM 协作推进同一个结果。^[raw/articles/2026-04-26-designing-products-for-agents.md]

## 三大设计原则

### 1. 教会 AI 智能体如何成功

调用你家智能体的一方需要知道什么才能成功？主动把这些信息交给它，不要让它自己摸索。

**正面案例 — Notion MCP**：`notion-create-pages` 工具描述明确要求智能体先获取 MCP 资源 `notion://docs/enhanced-markdown-spec` 再动笔，"不要猜测或幻觉 Markdown 语法"。所有 Notion 特有的格式规范都被明确交付给智能体。^[raw/articles/2026-04-26-designing-products-for-agents.md]

**反面案例 — Slack MCP**：智能体默认使用标准 Markdown 而非 Slack 特定格式，用户花在修改格式上的时间可能比自己手写还多。

核心思路：过去这类规范放在 API 文档里让开发者读，现在直接在智能体需要时交到它手中。

### 2. 建立反馈循环

通过以下机制从智能体交互中收集产品改进信号：^[raw/articles/2026-04-26-designing-products-for-agents.md]

- **Rationale 参数**：每次工具调用都要求智能体带上 `rationale` 解释调用意图，用于重建用户目标
- **反馈工具**：提供独立工具让智能体报告遇到的阻碍、尝试过的方案和卡住的地方
- **上下文种子**：在工具中加入专门参数捕捉后续有用的上下文

Rationale 日志中的模式可以揭示新产品机会。例如反复出现"生成事故报告"类表达，说明需要专门的 `build-incident-report` 工具。上线后又通过反馈循环持续迭代（增加日期范围参数、客户分组筛选器等）。

### 3. 留意上下文缺口

在智能体到智能体的交互中，双方各自掌握不同上下文。设计时应清楚判断哪一方在哪些信息上更有优势。^[raw/articles/2026-04-26-designing-products-for-agents.md]

**典型案例 — 费用报销**：

| 用户的 AI 首席幕僚知道 | 费用管理系统知道 |
|---|---|
| 日历（会议、时间、参与者） | 原始交易数据（商户、时间） |
| 邮箱（酒店/航班确认附件） | 公司报销政策 |
| Slack（晚餐关联到商务宴请对话） | 总账科目（GL accounts） |
| 收据（邮件附件、照片） | 公司过往费用归类习惯 |

传统 API 会把复杂选择（如选 GL code）丢回给用户；设计良好的智能体交互则索要上下文（"这是客户餐还是团队餐？"），由 AI 从日历或 Slack 中找到答案，系统自动套用正确科目。双方各自贡献自己知道的信息，交付更好的结果。

## 行业实践

### Ramp

企业支出管理平台 Ramp 的 MCP 每周活跃用户在三个月内增长了 10 倍，越来越多客户通过 Claude、ChatGPT 等 AI 智能体进入产品。^[raw/articles/2026-04-26-designing-products-for-agents.md]

### Salesforce Headless 360

Salesforce 在 2026 年 4 月宣布 27 年历史中最激进的架构转型，将平台每一项能力暴露为 API、MCP 工具或 CLI 命令，让 AI 智能体可以在不打开浏览器的情况下操作整个系统，首批开放 100 多个新工具和技能。^[raw/articles/2026-04-26-designing-products-for-agents.md]

## 与相关概念的关系

- [[aaron-levie]] 提出了类似的"软件无界面化"和 headless-first 趋势判断，认为价值从 UI 层转移到 API 层
- [[coordination-engineering]] 关注多 Agent 协作的工程范式，Agent-First Design 则聚焦于产品层面的智能体交互设计
- [[agent-skill-specification]] 标准化了智能体的能力描述格式，是 Agent-First Design 的技术基础之一
- [[context-rot]] 描述的是单 Agent 会话内的上下文衰减，而上下文缺口（Context Gap）关注的是不同智能体之间的信息不对称
