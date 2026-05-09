---
title: OpenAI
created: 2026-05-05
updated: 2026-05-07
type: entity
tags:
  - company
  - model
sources:
  - raw/articles/2026-05-05-openai-codex-mac-takeover.md
  - raw/articles/2026-05-05-codex-pet-goal.md
  - raw/articles/2026-05-07-gpt-5.5-instant-free.md
---

# OpenAI

OpenAI 是全球领先的 AI 公司，以 GPT 系列模型、ChatGPT、Codex CLI 等产品闻名。2026 年重点推进通用 AI Agent 战略。

## 2026 年战略重点

### GPT-5.5 旗舰模型
[[gpt-5.5]] 是 OpenAI 2026 年的旗舰模型，驱动 Codex、ChatGPT 等核心产品。

### Codex：从代码工具到通用 Agent
Sam Altman 在 2026 年公开表示"Codex 正在经历 ChatGPT 时刻"——[[codex]] 从代码生成 CLI 演进为能操控整台 Mac 的通用 AI Agent。这是 OpenAI 从"聊天助手"向"自主 Agent"战略转移的标志性产品。

### Agent 生态布局
- Codex CLI（终端 Agent）
- Codex App（独立桌面应用）
- ChatGPT（消费级对话产品）
- Operator（网页操作 Agent）

## GPT-5.5 Instant 免费发布（2026.05.07）

OpenAI 于 2026 年 5 月 7 日宣布 [[gpt-5.5]] Instant 变体对所有 ChatGPT 用户免费开放，这是 OpenAI 首次将旗舰级模型免费提供。^[raw/articles/2026-05-07-gpt-5.5-instant-free.md]

### Memory Sources 功能

同时推出的 **Memory Sources** 功能允许 ChatGPT 引用外部知识源，增强回答的事实准确性。该功能解决了 AI 对话中长期存在的"幻觉"问题——通过让模型明确标注信息来源，用户可以验证回答的可靠性。这与 [[claude-code]] 中通过工具调用获取实时数据来减少幻觉的策略异曲同工。

### 战略意义

GPT-5.5 Instant 的免费发布被解读为 OpenAI 对 [[anthropic]] 和 Google 在免费用户争夺战中的强势回应。API 标识为 `chat-latest`，开发者可通过标准 API 接入。幻觉率相比 GPT-5.4 下降 52.5%（AIME 2025 81.2%、GPQA 85.6%、MMMU-Pro 76.0%）。

## 竞争格局

OpenAI 在 AI Agent 领域与 [[anthropic]] 直接竞争：
- Codex vs [[claude-code]]：终端 AI 编程 Agent
- GPT-5.5 vs Claude Opus 4.7：底层模型能力
- Operator vs Computer Use：GUI 操控能力

## 相关链接

- [[gpt-5.5]] — 旗舰模型
- [[codex]] — 核心 Agent 产品
- [[claude-code]] — Anthropic 竞品
- [[anthropic]] — 主要竞争对手
