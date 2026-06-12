---
title: NotebookLM
created: 2026-05-15
updated: 2026-05-15
type: entity
tags: [tool, consumer, company]
sources:
  - raw/articles/2026-05-15_AI知识库技术演进拆解：从RAG到NotebookLM，再到LLMWiki.md
---

# NotebookLM

Google 推出的 AI 知识库产品，基于用户上传 sources 的 AI 笔记与研究助手。核心设计理念是 **只根据用户喂给它的资料来回答**，极大降低 AI 幻觉。

## 技术架构推测

NotebookLM 本质上是一套被高度产品化的 [[retrieval-augmented-generation|RAG]] 系统，其七层黑盒架构推测为：

| 层级 | 功能 |
|------|------|
| Source 接入 | 资料作为可引用和追溯的事实来源 |
| 文档理解 | 标题层级/章节关系/表格/图注恢复（非简单 OCR） |
| Chunk | 多粒度切分（Source/Chapter/Section/Paragraph/Chunk/Sentence） |
| 索引 | 向量索引 + BM25 + 元数据 + 文档树 + 引用映射 |
| Retrieval & Ranking | 多角度探索用户 sources |
| 长上下文装配 | Gemini 基于资料生成 |
| 引用与可追溯 | 回到原文核查 |

## 与传统 RAG 的区别

传统 RAG 链路暴露明显：分块→向量化→检索→重排→拼上下文→回答。NotebookLM 将这套工程链路 **自动化、黑盒化**，用户只需关心三点：

1. 我上传了哪些资料？
2. 我想问什么问题？
3. 我是否能回到原文核查？

## 文档理解是关键

> 就算我们知道了这是 NotebookLM 技术路径的核心，但我们其实也是做不出来的。因为 Google 在信息搜索这块是世界 T0 级别。

与 [[llm-wiki|LLM Wiki]] 理念对比：NotebookLM 是查询时拼答案，LLM Wiki 是提前编译知识结构。
