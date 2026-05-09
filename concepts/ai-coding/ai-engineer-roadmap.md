---
title: AI工程师路线图2026
created: 2026-05-09
updated: 2026-05-09
type: concept
tags: [ai-coding, practice]
sources: [raw/articles/2026-05-09-ai-engineer-roadmap-2026.md]
---

# AI工程师路线图 (2026版)

codejunkie99发布的17周6阶段路线图。^[raw/articles/2026-05-09-ai-engineer-roadmap-2026.md]

## 核心论点

2026年真正需要学的是 **agentic AI** 与 **harness engineering**，而非追逐框架。同一模型不同harness结果差异巨大（Opus 4.5在Claude Code 78% vs Smolagents 42%）。^[raw/articles/2026-05-09-ai-engineer-roadmap-2026.md]

## 六阶段规划

### 阶段0：建立心智模型
- workflow vs agent 区别
- 上下文工程四原语：Write / Select / Compress / Isolate

### 阶段1：Agent Loop基础
- 用原始SDK写一次agent loop
- 用 [[claude-agent-sdk]] 再写一次

### 阶段2：有状态持久化Agent
- LangGraph + Deep Agents 构建有状态agent

### 阶段3：亲手构建Harness
- 1500行mini-harness，包含：loop / tool dispatch / context compression / sub-agents / hooks / OTEL tracing / durable execution

### 阶段4：Eval与CI
- 构建 eval 与 CI regression harness

### 阶段5：生产加固
- 持续生产环境加固

## 推荐双栈

- **LangGraph 1.0 + Deep Agents**
- **Claude Agent SDK**

## 相关

- [[agentic-engineering]] — Agent工程化
- [[harness-engineering-deep-dive]] — Harness工程深入
- [[prompt-context-harness-engineering]] — 提示词/上下文/Harness工程
- [[token-roi]] — Token投资回报率
- [[claude-agent-sdk]] — Claude Agent SDK
