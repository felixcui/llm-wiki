---
title: AI Native PM
created: 2026-04-25
updated: 2026-04-27
type: concept
tags: [person, company]
sources:
  - raw/articles/2026-04-25_ai_pm_catwu.md
  - raw/articles/2026-04-27-anthropic-launch-secrets.md
---

# AI Native PM（AI 原生产品经理）

## 核心定义

AI Native PM 的核心不是做 6-12 个月路线图和跨团队排期，而是**缩短从想法到用户使用的周期**。这一理念由 Anthropic Claude Code 产品负责人 [[cat-wu]] 在 Lenny's Podcast 访谈中系统阐述。^[raw/articles/2026-04-25_ai_pm_catwu.md]

## 关键特征

### 懂模型边界

AI Native PM 必须深入理解当前模型的能力边界——知道模型能做什么、不能做什么。这不是理论上的了解，而是通过大量实际使用建立的直觉。

### 亲自试错

不要只看报告和数据，要自己上手用产品。AI 产品迭代速度极快，用户反馈的滞后性远大于传统软件。

### 定义关键任务体验

聚焦用户完成特定任务的全流程体验，而非堆砌功能点。

## "恰好正确程度的 AGI 信仰"

PM 最难的技能是保持**恰到好处的乐观**——^[raw/articles/2026-04-25_ai_pm_catwu.md]

- **太乐观**：忽略当前用户的真实痛点，做出的功能脱离实际
- **太保守**：在模型快速升级时措手不及，被竞争对手超越

## 关键实践

### Research Preview 机制

Anthropic 的发布策略：明确告知用户产品处于早期阶段，降低发布承诺。这使得团队可以在一两周内推出功能，而不必等待"完整产品"。几乎所有功能都以 Research Preview 形式先上线。详见 → [[research-preview]]^[raw/articles/2026-04-25_ai_pm_catwu.md]

### Evergreen Launch Room

跨职能快速反应流水线：工程师在功能完成后发到 Evergreen Launch Room 频道，Docs 负责人 Sarah、PMM 负责人 Alex、DevRel 团队的 Tarek 和 Lydia 直接响应，第二天即可发布。无需走传统发布流程。详见 → [[launch-room]]^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### Team Principles 替代 PRD

用 Team Principles 替代常规 PRD。Team Principles 定义核心用户和取舍原则，让团队每个人都能自主决策。

### 角色融合

传统职能边界在 AI 团队中被打破——

- PM 写代码
- 工程师做产品决策
- 设计师也提交代码

Anthropic Claude Code 团队几乎所有 PM 都有工程背景或直接写代码，设计师也曾是前端工程师。有工程背景的人能更快判断一个东西做起来到底有多难，这个判断在当前节奏下太关键了。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### "为未来模型做产品"

AI Native PM 需要具备前瞻性：提前构建当前模型尚不能完美完成的产品原型，新模型发布时直接替换验证。每次新模型发布，第一件事是审视哪些功能已被模型原生能力覆盖并删除。详见 → [[designing-for-future-models]]^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

## 与传统 PM 的对比

| 维度 | 传统 PM | AI Native PM |
|------|---------|-------------|
| 规划周期 | 6-12 个月路线图 | 一周到一天 |
| 发布策略 | 完整产品发布 | Research Preview |
| 需求文档 | PRD | Team Principles |
| 角色边界 | 职能分离 | 角色融合 |
| 模型理解 | 非必需 | 核心能力 |
| 试错方式 | 看报告 | 亲自上手 |

## 相关条目

- [[cat-wu]] — 提出 AI Native PM 理念的 Anthropic 产品负责人
- [[claude-code]] — AI Native PM 方法论的实践产品
- [[boris-cherny]] — Cat Wu 的技术搭档
- [[anthropic]] — AI Native PM 方法论的实践公司
- [[research-preview]] — AI Native PM 的关键发布实践
- [[launch-room]] — AI Native PM 的关键协同实践
- [[designing-for-future-models]] — AI Native PM 的产品设计哲学
