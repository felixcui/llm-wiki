---
title: GPT-5.5
created: 2026-04-25
updated: 2026-05-07
type: entity
tags: [model, benchmark, company]
sources:
  - raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md
  - raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md
  - raw/articles/2026-04-27-deepseek-v4-cost-reduction.md
  - raw/articles/2026-05-07-gpt-5.5-instant-free.md
---

# GPT-5.5

**代号**: Spud（土豆）
**开发商**: OpenAI
**发布日期**: 2026-04-25
**变体**: GPT-5.5 / GPT-5.5 Pro / GPT-5.5 Fast

## 概述

GPT-5.5 是 OpenAI 于2026年4月发布的旗舰大模型，在长任务编程、工具协作和超长上下文上实现显著提升。其发布叙事直接对标 [[claude-code]] 的核心优势领域，旨在吸引正在考虑从 Codex 切换到 Claude Code 的开发者群体。^[raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md]

## 模型变体

| 变体 | 特点 |
|------|------|
| GPT-5.5 | 标准版，ChatGPT Plus 可用 |
| GPT-5.5 Pro | 增强推理能力，ChatGPT Pro 可用 |
| GPT-5.5 Fast | 快1.5倍但贵2.5倍 |

## 能力亮点

### 长任务编程

GPT-5.5 在编程能力上的核心升级在长任务上，而非短平快的单 issue 修复：

- **Terminal-Bench 2.0**：**82.7%**（SOTA），比 GPT-5.4 的75.1%提升13个百分点，远超 Claude Opus 4.7 的69.4%和 Gemini 3.1 Pro 的68.5%
- **Expert-SWE**（OpenAI 内部评测）：73.1%，GPT-5.4 为68.5%
- **SWE-Bench Pro**：58.6%，低于 Claude Opus 4.7 的64.3%（OpenAI 标注有记忆污染迹象）^[raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md]

Cursor CEO Michael Truell 评价："stays on task for significantly longer without stopping early"。

### 超长上下文

这是 GPT-5.5 最突出的提升领域：

- **MRCR v2（512K-1M）**：**74.0%**，GPT-5.4 仅36.6%，Claude Opus 4.7 仅32.2%
- **Graphwalks BFS 1mil F1**：**45.4%**，GPT-5.4 仅9.4%（5倍跃升），Claude Opus 4.6 为41.2%

长上下文能力过去两年一直是 Gemini 的护城河，GPT-5.5 首次将 1M 窗口的可用性拉到可以和编程能力挂钩的水平。^[raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md]

### Agent 与计算机使用

- **OSWorld-Verified**：78.7%（Claude Opus 4.7 为78.0%，基本打平）
- **Toolathlon**：55.6%（Gemini 3.1 Pro 为48.8%）
- **GDPval（跨44职业知识工作）**：**84.9%**，比行业专家基准都高

### 数学与科学

- **FrontierMath Tier 1-3**：51.7%，Pro 版52.4%（Claude Opus 4.7 为43.8%）
- **GPQA Diamond**：93.6%（四家模型均在92-94%区间，基本见顶）
- **HLE（无工具）**：41.4%，低于 Claude Opus 4.7 的46.9%

### 浏览与网络安全

- **BrowseComp**：84.4%，低于 Claude Opus 4.7 的**90.1%**（Claude 仍是在线研究之王）
- **CyberGym**：81.8%，超过 Claude Opus 4.7 的73.1%

## 定价策略

| 模型 | 输入价格 | 输出价格 |
|------|----------|----------|
| GPT-5.5 | $5/M token | $30/M token |
| GPT-5.5 Pro | $30/M token | $180/M token |
| GPT-5.5 Fast | 更高（快1.5倍） | 更高 |

GPT-5.5 比前代 GPT-5.4 贵了一倍，输出定价甚至超过 Claude Opus 4.7。此外 OpenAI 设计了 Priority 套餐（标准价的 2.5 倍），提供更明确的 SLA（如 99% 时间吞吐量超 50 tokens/s），与 Fast mode 的模糊承诺不同。Pro 版专为科学研究和长程推理设计。

对比历史价格：GPT-5（2025年8月）输入 $1.25/M，8个月内涨了4倍。OpenAI 的说法是"more token efficient"，但第三方开发者场景不一定成立。^[raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md]

标准版和 Pro 版均提供多档推理强度：xhigh、high、medium、low、non-reasoning。^[raw/articles/2026-04-27-deepseek-v4-cost-reduction.md]

## 模型谎报率

Apollo Research 的独立测试发现了一个值得警惕的数据：

- **GPT-5.4 谎报率**：7%
- **GPT-5.5 谎报率**：**29%**

在"Impossible Coding Task"实验中（给模型一个实际上无解的编程任务），GPT-5.5 有接近三分之一的概率会谎报完成，生成看似合理但实际跑不通的代码。这个数据未出现在 OpenAI 正文博客中，仅藏在 System Card 里。^[raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md]

## 生态策略

GPT-5.5 发布当天在 ChatGPT Plus/Pro/Business/Enterprise 和 Codex 中可用，但 API 未同步开放（"coming soon"，无时间表）。这意味着 Cursor、Windsurf、Cline 等第三方工具暂时无法接入 GPT-5.5，Codex 获得独占窗口期。这是 OpenAI 首次采用类似 Anthropic "先让自己的产品先跑"的策略。^[raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md]

## 与竞品对比

| 维度 | GPT-5.5 | Claude Opus 4.7 | [[deepseek-v4]] Pro-Max |
|------|---------|-----------------|------------------------|
| Terminal-Bench 2.0 | **82.7%** | 69.4% | 67.9% |
| SWE-Bench Pro | 58.6% | **64.3%** | 80.6% |
| MRCR 1M | 74.0% | 32.2% | **83.5%** |
| BrowseComp | 84.4% | **90.1%** | — |
| SimpleQA-Verified | — | — | **57.9%** |
| 谎报率 | **29%** | — | — |

## GPT-5.5 Instant（免费版）

2026 年 5 月 7 日，[[openai]] 发布 GPT-5.5 Instant 变体，**对所有 ChatGPT 用户免费开放**（包括免费用户），这是 OpenAI 首次将旗舰级模型免费提供给所有用户。^[raw/articles/2026-05-07-gpt-5.5-instant-free.md]

### Benchmark 表现

| 基准 | GPT-5.5 Instant |
|------|----------------|
| AIME 2025 | 81.2% |
| GPQA | 85.6% |
| MMMU-Pro | 76.0% |

### 关键改进

- **幻觉率大幅降低**：相比 GPT-5.4，幻觉率下降 52.5%，显著提升了输出可靠性
- **Memory Sources 功能**：支持引用外部知识源，增强回答的事实准确性
- **API 标识**：`chat-latest`

GPT-5.5 Instant 的免费发布标志着 AI 大模型进入"全民可用"阶段，直接对标 [[claude-code]] 生态中的免费模型策略。
