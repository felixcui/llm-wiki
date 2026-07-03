---
title: Prompt Engineering
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [optimization, inference]
sources: [raw/articles/2026-04-28-ai-engineer-learning-path.md]
confidence: medium
---

# Prompt Engineering

Prompt Engineering 是与 LLM 交互的核心技能，目标是从"和模型聊天"升级为"可预测地控制模型"。

## 核心能力

- 理解 LLM 擅长什么、又会在哪些地方失效
- 学会使用 OpenAI、Anthropic 这类 API
- 理解 **Structured Outputs** — JSON、schema、tool responses
- 学会针对不同任务做 prompt engineering

## 在 AI Engineering 中的定位

Prompt Engineering 是 [[ai-engineering]] 学习路径的第一站（LLM 基础），也是其他所有能力的基础。它与 [[retrieval-augmented-generation]] 结合使用，可显著提升 RAG 系统的回答质量。

## 进阶方向

- **LLM-as-a-Judge** — 使用 LLM 评测 LLM 输出，是 AI 系统测试和 Evaluation 的关键技术
- **Tool Calling** — 让 LLM 不只是回答，还能执行动作，这是 [[ai-engineering]] 中 AI Agents 部分的基础
- **Few-shot / Chain-of-Thought** — 高级提示策略，提升复杂推理任务准确率

## 评估

Prompt engineering 的效果需要通过 Evaluation 来量化衡量，从"感觉还不错"走向"可以被量化衡量"。^[raw/articles/2026-04-28-ai-engineer-learning-path.md]
