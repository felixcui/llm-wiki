---
title: AGENTS.md
created: 2026-05-07
updated: 2026-05-15
type: concept
tags:
  - ai-coding
  - practice
sources:
  - raw/articles/2026-05-07-agents-md-guide.md
  - raw/articles/2026-05-14_别让AI瞎猜了：用HarnessEngineering终结无限返工.md
---

# AGENTS.md

**类型**: AI Coding Agent 上下文文件开放标准

## 概述

AGENTS.md 是一个开放格式标准，用于为 AI Coding Agent 提供项目上下文。它经历了从 CLAUDE.md（Anthropic 专属）到碎片化配置文件，最终统一为跨平台标准 AGENTS.md 的演进过程，由 Linux Foundation / Agentic AI Foundation 推动标准化。目前已有超过 60,000 个项目采用该标准。^[raw/articles/2026-05-07-agents-md-guide.md]

## 核心原则

**"Map, not Manual"**（地图，而非手册）

AGENTS.md 的核心理念是为 AI Agent 提供项目导航的"地图"，而非详尽的操作手册。它应该告诉 Agent 项目结构、关键约定和约束条件，让 Agent 能够自主导航和决策。^[raw/articles/2026-05-07-agents-md-guide.md]

## 五大实践

### 1. 仓库整合

将分散的 Agent 配置文件整合到统一的 AGENTS.md 中，避免多文件碎片化。^[raw/articles/2026-05-07-agents-md-guide.md]

### 2. 统一环境配置

标准化开发环境的配置描述，确保 Agent 能正确理解项目的构建和运行要求。^[raw/articles/2026-05-07-agents-md-guide.md]

### 3. 验证循环

定义 Agent 完成任务后的验证步骤，确保代码质量。^[raw/articles/2026-05-07-agents-md-guide.md]

### 4. 自动化检查

集成自动化检查流程（lint、test、type check 等），让 Agent 修改后自动验证。^[raw/articles/2026-05-07-agents-md-guide.md]

### 5. 参考项目

提供同类项目的参考链接，帮助 Agent 理解代码风格和架构模式。^[raw/articles/2026-05-07-agents-md-guide.md]

## 写作模板（9 个章节）

标准的 AGENTS.md 文件应包含以下章节：

1. **项目概述** — 项目是做什么的
2. **架构** — 代码结构和关键模块
3. **开发环境** — 如何搭建和运行
4. **编码约定** — 命名规范、风格要求
5. **测试** — 如何运行和编写测试
6. **常用命令** — 构建、部署等常用操作
7. **约束与限制** — Agent 应避免的操作
8. **验证流程** — 任务完成后的检查步骤
9. **参考资料** — 相关文档和项目链接

## 相关工具与概念

AGENTS.md 标准被 [[claude-code]]、Cursor、Windsurf 等主流 AI Coding 工具广泛支持。在实践中，它与 [[spec-driven-development]] 理念高度契合——通过明确的规格说明指导 AI 的工作。其设计思想也影响了 [[claude-code-system-prompt]] 中对 Agent 行为的规范化描述。
