---
title: Research Preview（研究预览）
created: 2026-04-27
updated: 2026-04-27
type: concept
tags: [organization]
sources:
  - raw/articles/2026-04-25_ai_pm_catwu.md
  - raw/articles/2026-04-27-anthropic-launch-secrets.md
---

# Research Preview（研究预览）

## 核心定义

Research Preview 是一种产品发布策略：明确告知用户产品处于早期阶段，可能不完美、甚至不永久保留，以此降低发布承诺和用户预期。这使得团队可以在一两周内推出功能，而不必等待"完整产品"。^[raw/articles/2026-04-25_ai_pm_catwu.md]

## Anthropic 的实践

[[anthropic]] 是 Research Preview 策略的典型实践者。产品负责人 [[cat-wu]] 在 Lenny's Podcast 中系统阐述了这一机制：^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

- **几乎所有功能**都以 Research Preview 形式先上线
- 用户知道这是早期版本，预期管理前置
- 团队不需要做到完美才能发布，一两周就能推出去
- 通过用户实际使用反馈来决定功能是否保留或改进

## 核心价值

1. **速度优先**：消除"完美主义"带来的发布延迟
2. **真实反馈**：尽早获取用户在真实场景中的使用数据
3. **低成本试错**：不合适的功能可以快速废弃，沉没成本低
4. **迭代优化**：先上线再打磨，基于数据而非假设做决策

## 与传统发布模式的对比

| 维度 | 传统发布 | Research Preview |
|------|---------|-----------------|
| 发布条件 | 功能完整、质量达标 | 功能可用即可 |
| 用户预期 | 完整产品体验 | 早期版本，可能变更 |
| 迭代周期 | 数月 | 一到两周 |
| 废弃成本 | 高（已承诺） | 低（已预期） |

## 适用条件

Research Preview 策略并非万能，适用于以下场景：

- **用户群体成熟度高**：能理解早期产品的不完善
- **品牌信任度强**：用户愿意为早期体验承担风险
- **迭代能力足够**：团队有能力快速响应反馈并改进
- **功能独立性**：单个功能可独立发布和废弃

## 相关条目

- [[ai-native-pm]] — Research Preview 是 AI Native PM 方法论的关键实践
- [[launch-room]] — 与 Research Preview 配套的快速发布流程
- [[anthropic]] — Research Preview 策略的主要实践公司
- [[cat-wu]] — 系统阐述 Research Preview 机制的产品负责人
- [[claude-code]] — 大量功能通过 Research Preview 发布的产品
