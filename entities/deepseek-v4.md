---
title: DeepSeek V4
created: 2026-04-25
updated: 2026-05-14
type: entity
tags: [model, architecture, training, optimization, inference, open-source]
sources:
  - raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md
  - raw/articles/2026-04-25-deepseek-v4-review-collection.md
  - raw/articles/2026-04-27-deepseek-v4-cost-reduction.md
  - raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md
---

# DeepSeek V4

**发布日期**: 2026-04-25
**开发商**: DeepSeek
**协议**: MIT（开源）
**变体**: Pro / Flash

## 概述

DeepSeek V4 是 DeepSeek 于2026年4月发布的开源旗舰大模型，分为 Pro 和 Flash 两档，均支持 1M token 上下文窗口。Pro 在 Agent 编码、世界知识、推理和长文本任务上达到开源领先水平，部分指标接近或超过顶级闭源模型；Flash 主打低成本，推理和简单 Agent 能力接近 Pro。^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

## 模型规格

| 维度 | V4-Pro | V4-Flash |
|------|--------|----------|
| 总参数 | 1.6T | 284B |
| 激活参数 | 49B | 13B |
| 上下文长度 | 1M token | 1M token |
| MoE 配置 | — | 1共享 + 256路由专家，每 token 激活 6 个 |
| 预训练数据 | 33T token | 32T token |
| 词表大小 | 128K | 128K |

## 子页面

- [[deepseek-v4-technical]] — 架构创新（CSA/HCA混合注意力、mHC残差连接、Muon优化器）、训练方法（OPD、思考强度）、FP4量化感知训练、基础设施优化与性能评测
- [[deepseek-v4-market]] — API定价、成本优势、定价对比、用户迁移案例与竞争格局影响

## 关键对比

在 Agent 编码场景中，DeepSeek V4-Pro 的表现优于 [[claude-code]] 的 Sonnet 4.5 基线，接近 Opus 4.6 非思考模式。与 [[gpt-5.5]] 相比，V4-Pro 在知识问答（SimpleQA）上领先，但在 Terminal-Bench 和 GDPval 上仍有差距。^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

V4 发布迫使大模型竞争从单纯比性能转向同时比价格与实用性。参见 [[llm-price-competition]]。^[raw/articles/2026-04-27-deepseek-v4-cost-reduction.md]

## 国产化适配

V4 明确走向"去 CUDA 化"，针对华为昇腾等国产 AI 芯片进行了深度适配与优化，保证最顶尖的大模型可以跑在完全自主的算力基础设施上。^[raw/articles/2026-04-25-deepseek-v4-review-collection.md]

在训练方法上，DeepSeek V4 采用了 [[grpo]]（Group Relative Policy Optimization）等强化学习策略进行模型对齐和优化。

## 相关页面

- [[gpt-5.5]] — OpenAI 旗舰模型，V4 在定价上的主要对标对象
- [[llm-price-competition]] — 大模型价格竞争全局分析
- [[claude-code]] — Anthropic 编码 Agent，V4 在 Agent 场景的主要竞品
