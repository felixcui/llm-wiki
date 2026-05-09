---
title: Software 3.0
created: 2026-05-05
updated: 2026-05-05
type: concept
tags:
  - comparison
  - timeline
  - practice
sources:
  - raw/articles/2026-05-05-karpathy-agentic-engineering.md
  - raw/articles/2026-05-05-software-3-0-era.md
---

# Software 3.0

**Software 3.0** 是 [[karpathy]] 提出的软件范式演进三期模型，描述了从传统编程到 AI 原生编程的完整演进路径。

## 三期演进

### Software 1.0 — 显式逻辑

人类手写 if/else、循环、函数调用等显式逻辑。这是传统软件工程时代，开发者完全控制每一行代码的执行路径。

### Software 2.0 — 数据驱动（2016–至今）

用数据训练神经网络替代显式逻辑。典型场景：图像分类、语音识别、翻译。核心转变：从"编写规则"到"提供数据和损失函数"。深度学习的黄金时代。

### Software 3.0 — LLM 即解释器（2025–）

LLM 作为新的"编程层"，通过以下要素构建应用：

- **Prompt** — 自然语言指令替代传统 API 调用
- **上下文（Context）** — 动态注入信息替代硬编码配置
- **Agent** — 自主决策和工具调用替代固定流程
- **工具（Tools）** — MCP 等协议连接外部能力
- **记忆（Memory）** — 持久化状态替代传统数据库查询逻辑
- **验证（Verification）** — 输出验证替代传统单元测试

## 核心观点

> LLM 吞噬传统应用的中间层。

传统应用需要多个专用模块协作：OCR → NLP → 图像搜索 → 前端排版。Software 3.0 中，LLM 直接从 prompt 到结果，大幅简化技术栈。

## 与 AI 编程的关系

Software 3.0 既是**应用构建范式**的演进，也改变了**软件开发方式**本身：

- [[vibecoding]] — Software 3.0 的"低门槛"入口
- [[agentic-engineering]] — Software 3.0 的"高可靠"保障
- [[claude-code]] — Software 3.0 时代的典型开发工具

## 挑战

- 可靠性：LLM 输出不确定性带来验证难题
- 成本：大模型推理开销
- 安全：Agent 自主执行的风险控制
- 生态系统：工具链和标准仍在快速演变

## 相关概念

- [[karpathy]] — 概念提出者
- [[vibecoding]] — Software 3.0 的编程方式
- [[agentic-engineering]] — Software 3.0 的工程方法论
- [[openai]] — 推动这一范式的主要公司
- [[retrieval-augmented-generation]] — Software 3.0 的关键技术之一
