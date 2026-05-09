---
title: Designing for Future Models（为未来模型做产品）
created: 2026-04-27
updated: 2026-04-27
type: concept
tags: [tool, prediction]
sources:
  - raw/articles/2026-04-27-anthropic-launch-secrets.md
---

# Designing for Future Models（为未来模型做产品）

## 核心定义

"为未来模型做产品"是一种 AI 产品设计哲学：提前构建当前模型尚不能完美完成的产品原型，当新模型发布时直接替换验证，而非等到模型能力到位才开始设计。这一理念由 Instagram 联合创始人 Mike Krieger 在播客中提出，被 [[anthropic]] 产品负责人 [[cat-wu]] 引用并实践。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

## 核心现象：模型会吃掉你的产品

每次新模型发布，[[claude-code]] 团队做的第一件事是**删功能**——因为新模型的能力直接覆盖了之前需要 harness 来弥补的缺陷。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### 典型案例：To-do List 功能

Claude Code 早期有一个 to-do list 功能。当时的模型在做大规模代码重构时，改了 5 个调用点就停了（实际有 20 个要改），团队让模型先列清单再逐个完成，效果显著。

但 Opus 4 发布后，模型自己就能主动列清单、逐个完成。to-do list 从"必要的拐杖"变成了"可有可无的辅助界面"——最终被删除。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### 代码审查案例

Claude Code 团队尝试了多个版本的自动代码审查，准确率一直不够。直到 Opus 4.5 和 4.6 发布，才达到让工程团队真正依赖的水平。现在 [[anthropic]] 内部合并 PR 之前，必须先过 Claude 的代码审查。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

## Harness 会被模型当早餐吃掉

Lenny Rachitsky 用一个生动的比喻总结了这一现象：**"模型会把你的 harness 当早餐吃掉。"** [[cat-wu]] 对此表示完全同意。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

这意味着：
- 为当前模型能力缺陷设计的 harness 层，会随着模型升级而变得多余
- 产品设计需要持续审视：这个功能/约束是否还必要？
- [[claude-code]] 团队的做法：每次发布新模型，通读整个 system prompt，逐段反思模型是否还需要某个提醒

## 实践策略

### 提前做原型

> "提前做好还没完全能用的产品原型很重要。这样新模型一出来，你直接换上去看看差距有没有被弥合，就可以了。" —— Kat Wu^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### 让模型自我反思

[[cat-wu]] 分享的独特技巧：发现模型行为异常时，直接问模型"你为什么做出了那个选择？"模型的回答有时会指向 system prompt 的困惑或 harness 层的缺陷，直接给出改进方向。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### 定期清理

每次新模型发布后，系统性地审视现有功能：
- 这个 harness 约束还有必要吗？
- 这个 UI 辅助还有价值吗？
- 这个功能是否已被模型原生能力覆盖？

## 对产品设计的启示

1. **Harness 设计要轻量**：既然模型会进步，harness 层就不宜过度工程化
2. **拥抱功能删除**：删除被模型能力覆盖的功能不是失败，是进步
3. **原型先行**：为下一个模型提前准备好产品原型，缩短从模型到产品的时滞
4. **持续审视**：建立定期审视机制，避免为过时的模型能力维护冗余代码

## 相关条目

- [[claude-code]] — 实践"为未来模型做产品"的核心产品
- [[prompt-context-harness-engineering]] — Harness 工程三维框架，理解"模型吃掉 harness"的理论基础
- [[harness-engineering-deep-dive]] — Harness Engineering 深度解析
- [[cat-wu]] — 系统阐述这一理念的产品负责人
- [[anthropic]] — 实践这一理念的AI公司
- [[ai-native-pm]] — AI Native PM 方法论中包含的迭代思维
