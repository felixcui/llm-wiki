---
title: LLM Wiki
created: 2026-05-15
updated: 2026-05-15
type: concept
tags: [tool, memory, practice]
sources:
  - raw/articles/2026-05-15_AI知识库技术演进拆解：从RAG到NotebookLM，再到LLMWiki.md
  - raw/articles/2026-05-14_深度解析LLMWikiObsidian-WikiGBrain：Agent时代知识的"自组织"与"自进化".md
---

# LLM Wiki

Andrej Karpathy（[[karpathy]]）于 2026 年 4 月开源的项目，核心是将非结构化资料转化为 Agent 可持续维护的结构化知识库。

## 核心理念

传统 RAG / [[notebooklm|NotebookLM]] / ChatGPT 文件上传的模式：

> 上传资料 → 查询时召回相关片段 → 临时综合回答

问题是：每次问问题，模型都在重新从碎片里 **现拼答案**，知识没有持续沉淀。

LLM Wiki 的模式：

> 原始资料不动 → LLM 读取资料 → 增量维护一个结构化 Wiki → 不断更新实体页、主题页、交叉引用、矛盾点和综合结论

**不是查询时拼答案，而是提前把知识编译成一个可持续演化的知识结构。**

## 技术实现

LLM Wiki 本质是一个 Markdown 文件，指导大模型 Agent 进行知识的更新与结构化。核心流程：

1. 原始资料保持不动（不可变源）
2. LLM 读取资料后增量更新 Wiki 页面
3. 维护实体页、主题页、交叉引用
4. 标注矛盾点和综合结论

## 与其他知识系统的对比

| 系统 | 模式 | 特点 |
|------|------|------|
| 传统 RAG | 查询时检索 | 碎片拼凑，无持续沉淀 |
| NotebookLM | 高阶 RAG | 产品化 RAG，Google 级文档理解 |
| LLM Wiki | 提前编译 | 结构化 Wiki 持续演化 |
| Obsidian-Wiki | 本地优先 | Markdown + Wikilinks + 图谱 |
| GBrain | YC 总裁项目 | 类似 LLM Wiki 但更工程化 |

## 与 Agent 自进化的关系

LLM Wiki 解决的是 Agent 自进化中"知识"层面的问题：

- 知识库能"自动梳理"、"自动组织"、"自动更新"甚至"自动进化"
- 通过人给予 Agent 更多"知识"来提升 Agent 能力
- 知识的效果是影响 [[prompt-context-harness-engineering|Prompt/Context/Harness]] 非常重要的一部分

与 [[self-improving-agent|自进化 Agent]] 的 Skill 机制互补 —— Skill 是经验沉淀，LLM Wiki 是知识沉淀。
