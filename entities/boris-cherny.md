---
title: Boris Cherny
created: 2026-04-25
updated: 2026-05-09
type: entity
tags: [person, company]
sources:
  - raw/articles/2026-04-25_ai_pm_catwu.md
  - raw/articles/2026-04-27-anthropic-launch-secrets.md
  - raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md
  - raw/articles/2026-05-09-claude-md-8-tips.md
---

# Boris Cherny

**身份**: Anthropic Claude Code 技术负责人和创造者

## 背景

- 《Programming TypeScript》作者（O'Reilly 出版）
- 2024年9月加入 Anthropic
- 构建了 Claude Code 的**第一个终端原型**

## 在 Anthropic 的角色

Boris Cherny 是 [[claude-code]] 的技术负责人和创造者，与产品负责人 [[cat-wu]] 搭档。两人共同带领团队将 Claude Code 的功能交付周期从半年压缩到一天。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### 分工模式

Boris 负责**产品愿景**，定义三到六个月后产品应该长什么样。[[cat-wu]] 负责把愿景拆解成可执行路径，并协调市场、销售、财务等各个团队。两人约 80% 的想法一致，剩下的 20% 谁更在意就谁来推。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

## 相关条目

- [[claude-code]] — Boris Cherny 创造的核心产品
- [[cat-wu]] — Boris Cherny 的产品搭档
- [[anthropic]] — Boris Cherny 所属公司
- Claude Design — Claude Code 的设计系统
- [[mcp-model-context-protocol]] — 模型上下文协议

## Sequoia 访谈：工程未来（2026年5月）

### 工作模式

Boris 在 Sequoia 访谈中透露，2026 年他**未亲手写过一行代码**。日常工作完全在手机上通过 5-10 个 session 管理数百个 Agent，单日可处理 150 个 PR。^[raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md]

### Claude Code 的诞生

Claude Code 诞生于 Anthropic 的"product overhang"时期——产品愿景领先于模型能力。前 6 个月几乎不可用，但随着底层模型能力追上，产品迅速获得成功。^[raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md]

### 对未来的预测

- **模型对齐增强**：预测一年后模型对齐能力将显著增强，安全外壳（safety guardrails）将变轻，开发者可以直接让模型做更多事
- **Loop（定时 Agent 任务）**：强调 Loop 是未来方向——让 Agent 在无人监督下按计划执行任务，实现真正的自主化
- **工程师角色变化**：预测一年后工程师角色将发生根本性变化，从写代码转变为管理 Agent 团队^[raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md]

### CLAUDE.md 最佳实践

Boris 建议 CLAUDE.md 应控制在 **200 行以内**，避免过长导致模型注意力分散。推荐采用渐进式上下文三层披露策略：全局摘要 → 详细规范 → 按需深入。^[raw/articles/2026-05-09-claude-md-8-tips.md]

Boris 的工作模式体现了 [[ai-native-organization]] 的理念——工程师不再直接写代码，而是通过管理和协调 Agent 来交付成果。
