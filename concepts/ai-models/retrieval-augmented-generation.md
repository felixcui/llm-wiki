---
title: RAG（Retrieval-Augmented Generation）
created: 2026-04-28
updated: 2026-05-15
type: concept
tags: [model, data, inference]
sources:
  - raw/articles/2026-04-28-ai-engineer-learning-path.md
  - raw/articles/2026-05-15_AI知识库技术演进拆解：从RAG到NotebookLM，再到LLMWiki.md
---

# RAG（Retrieval-Augmented Generation）

RAG（检索增强生成）是大多数真实 AI 系统的骨架技术，允许将自定义数据注入 LLM，使模型能够基于私有知识库做出回答，而不需要重新训练模型。

## 核心构成

- **Vector Search** 与 **Semantic Retrieval** — 将文档向量化后，基于语义相似度检索相关内容
- **Chunking 策略** — 文档分割策略比很多人想象中更重要，直接影响检索质量
- **向量数据库** — Elasticsearch、Qdrant、Pinecone、Weaviate、pgvector
- **真实数据源处理** — PDF、网页、transcript 等异构数据

## 实践项目

- FAQ 助手
- 文档问答系统
- 内部知识检索

## 与其他概念的关系

RAG 是 [[ai-engineering]] 的核心技能之一，与 [[prompt-engineering]] 结合使用可显著提升 LLM 输出质量。在 Agent 系统中，RAG 也常作为 Skill 的知识来源（详见 [[skills-rag-integration]]）。

## 工具生态

- **Vector DBs**: Pinecone、Weaviate、Qdrant、pgvector
- **检索框架**: Elasticsearch
- **与 Agent 框架结合**: LangChain、PydanticAI、OpenAI Agents SDK 均支持 RAG 模式
