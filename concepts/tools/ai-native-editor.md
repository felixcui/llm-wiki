---
title: AI 原生编辑器（AI Native Editor）
created: 2026-05-05
updated: 2026-05-05
type: concept
tags:
  - tool
  - ai-coding
  - product-design
sources:
  - raw/articles/2026-05-05-zed-1-0-launch.md
  - raw/articles/2026-05-05-cursor-sdk-launch.md
---

# AI 原生编辑器（AI Native Editor）

**AI 原生编辑器** 是代码编辑器的新范式：AI 不是插件或附加功能，而是编辑器的**内置核心能力**。编辑器从底层架构上为 AI Agent 的深度集成而设计。

## 代表产品

### Zed

- Rust 重写的高性能编辑器
- GPU 加速的 GPUI 渲染框架
- Atom 原班人马打造
- 2026 年发布 1.0 正式版
- 支持并行调用多个模型 Agent

### Cursor

- 基于语义索引的代码理解
- Trigram 搜索加速代码导航
- 2026 年推出 Cursor SDK
- 深度集成本地与云端模型

## 核心特征

1. **AI 即核心** — 不是"编辑器 + AI 插件"，而是"AI 优先的编辑器"
2. **多 Agent 并行** — 可同时调用多个模型 Agent 处理不同任务
3. **ACP 协议** — 让外部 Agent（如 [[claude-code]]、[[codex]]）与编辑器深度交互
4. **实时协作** — 人类与 AI 在同一编辑空间中协同工作
5. **语义理解** — 基于代码语义（而非纯文本）的导航和修改

## ACP 协议

Agent Communication Protocol（ACP）是 AI 原生编辑器的关键技术：

- 标准化 Agent 与编辑器之间的通信
- 支持外部 Agent 读写编辑器状态
- 使 [[claude-code]]、[[codex]] 等 Agent 能直接操作编辑器

## 对开发流程的影响

AI 原生编辑器是 [[agentic-engineering]] 的基础设施之一：

- 为 Agent 提供结构化的代码操作接口
- 降低 Agent 与代码库交互的摩擦
- 支持 [[vibecoding]] 的快速迭代工作流
- 使多 Agent 协作在同一编辑环境中成为可能

## 与传统编辑器的对比

| 维度 | 传统编辑器 | AI 原生编辑器 |
|------|-----------|--------------|
| AI 定位 | 插件/扩展 | 内置核心 |
| 架构 | 文本操作为主 | 语义操作为主 |
| 模型调用 | 单次、串行 | 多次、并行 |
| Agent 集成 | 无/有限 | ACP 深度集成 |

## 相关概念

- [[claude-code]] — 可通过 ACP 集成到编辑器
- [[codex]] — OpenAI 的编程 Agent
- [[agentic-engineering]] — AI 原生编辑器支撑的工程方法论
- [[vibecoding]] — AI 原生编辑器支持的主要工作流
