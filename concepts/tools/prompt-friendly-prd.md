---
title: Prompt友好型PRD
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [tool, organization]
sources:
  - raw/articles/2026-04-28-youmanju-ai-workflow.md
---

# Prompt友好型PRD

Prompt友好型PRD（Prompt-Friendly PRD）是一种面向 AI 协作的产品需求文档撰写方法论，由 [[youmanju|柚漫剧]] 团队在 AI 全流程提效实践中提出。核心思想是：**将 PRD 的结构设计为 AI 可直接解析和消费的格式，使 AI 能够基于标准化输入高效生成原型、代码和测试用例。**^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 核心原则

1. **结构化/标准化要求**：通过 prompt 撰写规则反推需求模板，提取需求中的关键词，与 prompt 词模板比对查缺补漏
2. **填空式撰写**：提供「单页面 0-1」「单页面 1-2」「多页面流程」3 种常见类型需求撰写模板，以填空形式帮助梳理产品逻辑
3. **AI 做「从 0 到 60 分」**：AI 负责信息收集与处理等重复性工作，人类完成深度判断和体验分析^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 实践方法

### Prompt 组件模板

设计师预先提供一套包含组件基础功能和交互逻辑的 Prompt 模板。产品经理只需输入业务核心逻辑，即可利用模板快速拼搭生成原型。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

### 需求范式协同

产品和设计共同定义一个「需求范式」，AI 基于此范式辅助快速生成原型草稿。范式的重要性在于：AI 辅助生成的前提是有一个清晰的、团队共同定义的框架。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

### 前置约束

- PM 需要先想清楚自己要什么——如果输入模糊，AI 输出也会模糊
- 研发提供前置代码约束 Prompt（组件库规范、布局规则、性能基准），确保设计产出天然符合技术架构^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 与 AI Native PM 的关系

[[ai-native-pm]] 关注的是「缩短想法到用户的周期」，而 Prompt友好型PRD 关注的是「如何让 AI 能够阅读并执行 PRD」。两者互补：AI Native PM 是战略层方法论，Prompt友好型PRD 是战术层执行工具。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 适用范围

适用于需要 AI 深度参与产品研发全流程的团队，尤其在「需求→原型→代码」链路中效果显著。对于涉及复杂交互细节和深度竞品分析的场景，仍需人类主导。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 交叉引用

- [[youmanju]] — 柚漫剧团队，该方法论的提出者和实践者
- [[ai-native-pm]] — AI 原生产品经理方法论，战略层对应
- [[design-md-specification]] — Markdown 原生设计系统规范，与 PRD 的 AI 可读设计理念一致
