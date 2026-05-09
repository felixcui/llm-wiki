---
title: DeepSeek V4 — 成本与市场影响
created: 2026-04-30
updated: 2026-04-30
type: entity
tags: [model, architecture, training]
sources:
  - raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md
  - raw/articles/2026-04-25-deepseek-v4-review-collection.md
  - raw/articles/2026-04-27-deepseek-v4-cost-reduction.md
  - raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md
---

> 子页面 — 详见主页面 [[deepseek-v4]]

# DeepSeek V4 — 成本与市场影响

## API 与定价

- **Pro**：输入缓存命中 1元/M token，未命中 12元/M，输出 24元/M
- **Flash**：输入缓存命中 0.2元/M，未命中 1元/M，输出 2元/M
- 接口兼容 OpenAI ChatCompletions 和 Anthropic 两套标准
- 旧模型名（deepseek-chat、deepseek-reasoner）将于 2026-07-24 停用^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

## 定价对比（美元计价）

| 模型 | 输入价格（$/M token） | 输出价格（$/M token） | 1M+1M 合计 |
|------|----------------------|----------------------|-----------|
| [[gpt-5.5]] | $5 | $30 | $35 |
| Claude Opus 4.7 | — | — | ~$30 |
| DeepSeek V4 Pro | $1.74 | $3.48 | $5.22（缓存命中 $3.625） |
| DeepSeek V4 Flash | $0.14 | $0.28 | $0.42（缓存命中 $0.308） |

在标准定价下，V4-Pro 成本约为 GPT-5.5 的 **1/7**、Claude Opus 4.7 的 **1/6**。缓存命中时差距进一步拉大——约为 GPT-5.5 的 **1/10**。Flash 版不到 GPT-5.5 和 Claude Opus 4.7 的 **2%**。^[raw/articles/2026-04-27-deepseek-v4-cost-reduction.md]

## 与竞品对比

在 Agent 编码场景中，DeepSeek V4-Pro 的表现优于 [[claude-code]] 的 Sonnet 4.5 基线，接近 Opus 4.6 非思考模式。与 [[gpt-5.5]] 相比，V4-Pro 在知识问答（SimpleQA）上领先，但在 Terminal-Bench 和 GDPval 上仍有差距。^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

## 竞争格局影响

V4 发布迫使大模型竞争从单纯比性能转向同时比价格与实用性。OpenAI 在 GPT-5.5 上涨价（标准版 $30/M 输出，Pro 版 $180/M），Anthropic 通过 Opus 4.7 新 tokenizer 变相涨价约 35%，DeepSeek 则以开源 + 极低价直接"掀桌"。参见 [[llm-price-competition]]。^[raw/articles/2026-04-27-deepseek-v4-cost-reduction.md]

## 用户迁移案例

### Sean Donahoe：全面切换编程 Agent

科技博主兼 AI 系统架构师 Sean Donahoe 在 V4 发布当天宣布将 Claude Code、Codex、Cursor、Aider 等所有编程 Agent 全部指向 DeepSeek 端点（原生 API，不经 OpenRouter/代理），月账单预计从数千美元降至数百美元（降幅 **90%+**），且强调输出质量不降反升。^[raw/articles/2026-04-27-deepseek-v4-cost-reduction.md]

### Rohan Paul：卡丁车游戏测试

AI 研究员 Rohan Paul 给 V4 Pro 和 GPT-5.5 同一提示词开发完整卡丁车竞速游戏（Canvas 渲染、物理引擎、AI 对手、道具系统、音效）。V4 Pro 输出了近 2 倍 token，但便宜了 4.3 倍。^[raw/articles/2026-04-27-deepseek-v4-cost-reduction.md]

### Simon Willison：鹈鹕骑自行车 SVG 进化

Simon Willison 从 V3 到 V4 的历代版本都用同一提示词生成 SVG。V4 Pro 在细节（链条、反光片、翅膀姿势）上显著优于前代，展示持续进化趋势。^[raw/articles/2026-04-27-deepseek-v4-cost-reduction.md]

## 成本优势总结

通过 CSA + HCA 混合注意力机制，V4-Pro 在百万上下文下的单 Token 推理算力需求仅为 V3.2 的 **27%**，KV Cache 内存占用低至 **10%**。Flash 版 API 成本极低（缓存命中 0.2 元/M token），推理速度极快，适合大规模高频商业应用。^[raw/articles/2026-04-25-deepseek-v4-review-collection.md]

## 相关页面

- [[deepseek-v4]] — 主页面，包含概述、模型规格与子页面索引
- [[deepseek-v4-technical]] — 架构创新、训练方法与基础设施优化
- [[gpt-5.5]] — OpenAI 旗舰模型，V4 定价的主要对标对象
- [[llm-price-competition]] — 大模型价格竞争全局分析
- [[claude-code]] — Anthropic 编码 Agent，Agent 场景主要竞品
