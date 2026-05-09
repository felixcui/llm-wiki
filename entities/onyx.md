---
title: Onyx
created: 2026-04-25
updated: 2026-04-25
type: entity
tags: [tool, open-source, data, inference]
sources:
  - raw/articles/2026-04-25-deep-researcher-onyx-crewai-voxtral.md
---

# Onyx

**类型**: 开源 AI 平台（RAG + Web Search + Deep Research + Agents）
**核心能力**: 企业级检索增强生成与深度研究
**部署方式**: 100% 可自托管

## 概述

Onyx 是一个开源 AI 平台，集成了 RAG、Web Search、Deep Research 和 Agents 四大能力，完全可自托管。在 DeepResearch Bench（22 领域 100 个博士级任务）测试中，基于 Onyx 的技术栈超越了 OpenAI、Gemini 和 Perplexity。^[raw/articles/2026-04-25-deep-researcher-onyx-crewai-voxtral.md]

Onyx 的架构理念强调**阶段隔离**和**可控的上下文流动**，与 [[hermes-agent]] 的分层上下文管理有异曲同工之处——两者都认为 Agent 系统的关键在于控制信息在各阶段间的传递方式。

## 三阶段架构

Onyx 的 Deep Research 流程分为三个阶段：

1. **澄清**（Clarification）：理解并细化用户意图
2. **规划**（Planning）：制定检索策略和执行路径
3. **迭代执行**（Iterative Execution）：循环执行检索与综合

两个关键隔离点确保信息不被过度传播：
- **Orchestrator 不搜索**：编排层不直接执行检索，避免上下文污染
- **Agent 不看完整查询**：执行层只接收子任务指令，防止原始查询在多次迭代中被"重新解释"

这种隔离设计与 [[coordination-engineering]] 中多 Agent 协作的上下文隔离原则一致。

## 六阶段检索流水线

Onyx 的检索系统采用六阶段流水线：^[raw/articles/2026-04-25-deep-researcher-onyx-crewai-voxtral.md]

1. **查询生成**（Query Generation）
2. **搜索重组**（Search Re-ranking）
3. **LLM 选择**（LLM-based Selection）
4. **上下文扩展**（Context Expansion）
5. **Prompt 构建**（Prompt Construction）
6. **答案综合**（Answer Synthesis）

这套流水线的核心特点是"会推理的检索"——不依赖简单的关键词匹配，而是在每个阶段都引入 LLM 做语义理解和质量判断。

## 数据源与部署

- **40+ 企业数据源**：支持主流企业数据系统（文档库、数据库、API 等）
- **自主索引**：所有数据索引在自己的基础设施上，不依赖第三方
- **完全自托管**：数据不出本地，满足企业合规要求

## 在 Deep Research 技术栈中的位置

Onyx 作为 **检索层**，与 CrewAI（编排层）和 Voxtral（语音层）组成完整的技术栈：

| 层级 | 工具 | 职责 |
|------|------|------|
| 检索层 | **Onyx** | RAG、Web Search、数据源连接 |
| 编排层 | CrewAI | 多 Agent 协作、阶段隔离 |
| 语音层 | Voxtral | 语音输入与输出 |
