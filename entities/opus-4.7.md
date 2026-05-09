---
title: Claude Opus 4.7
created: 2026-05-05
updated: 2026-05-05
type: entity
tags:
  - model
  - benchmark
sources:
  - raw/articles/2026-05-05-anthropic-hackathon-finalists.md
  - raw/articles/2026-05-05-llm-token-optimization-tips.md
---

# Claude Opus 4.7

Claude Opus 4.7 是 [[anthropic]] 2026 年的旗舰模型，以精确执行和深度推理能力著称。

## 核心特性

### 精确执行
- **字面执行提示词**：不自动泛化用户意图，严格按指令操作
- **长时间会话不跑偏**：Extended Thinking 能力确保长对话中保持任务焦点
- 这是与 [[gpt-5.5]] 的关键差异——Opus 4.7 更"听话"，减少意外行为

### 视觉能力
- 图片分辨率提升至 **2576px**（从之前的 1568px）
- 改善文档扫描、图表分析、UI 截图理解等场景

### Extended Thinking
- 深度推理模式，适合复杂编程、数学、架构设计任务
- 在 [[claude-code]] 中驱动 Managed Agents 等高级功能

## 实际应用

### 黑客松获奖项目
在 Anthropic 2026 年黑客松中，Opus 4.7 驱动了多个获奖项目：
- **Managed Agents**：多 Agent 编排系统，获最高奖项
- 展示了 Opus 4.7 在复杂 Agent 工作流中的可靠性

### Token 优化
Opus 4.7 对提示词格式更敏感，精确的提示词工程可显著降低 token 消耗：
- 使用结构化输出格式减少来回对话
- Extended Thinking 模式下一次性给出完整方案

## 竞争定位

Opus 4.7 与 GPT-5.5 代表了 2026 年旗舰模型的两条路线：
- **Opus 4.7**：精确、可控、适合 Agent 编排
- **GPT-5.5**：通用、灵活、适合广泛任务

## 相关链接

- [[anthropic]] — 开发公司
- [[claude-code]] — 主要应用场景
- [[gpt-5.5]] — 竞品模型
