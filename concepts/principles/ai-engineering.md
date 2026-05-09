---
title: AI Engineering
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [tool, optimization, alignment]
sources: [raw/articles/2026-04-28-ai-engineer-learning-path.md]
---

# AI Engineering

AI Engineering 是一门**围绕 LLM 构建可靠生产系统**的工程学科，区别于传统 ML 研究和纯模型训练。其核心技能集中在 20% 的关键能力驱动 80% 的实际工作。

> AI engineering 的重点，不是从零训练模型，而是围绕 LLM 去构建系统。^[raw/articles/2026-04-28-ai-engineer-learning-path.md]

## 核心技能栈

1. **LLM 基础** — 理解 LLM 工作原理、能力边界，掌握 API 调用、structured outputs 与 [[prompt-engineering]]
2. **RAG（Retrieval-Augmented Generation）** — 将自定义数据注入 LLM，涉及 vector search、chunking、semantic retrieval
3. **AI Agents** — tool calling、agent loop（think → act → observe → repeat）、[[mcp-model-context-protocol]]、multi-agent systems
4. **AI 系统测试** — 测试工具调用、评估一致性、LLM-as-a-judge
5. **Monitoring & Observability** — tracing、logging、cost tracking、feedback loops、Grafana/OpenTelemetry
6. **Evaluation** — 离线评测、retrieval quality 衡量、合成数据生成、基于结果迭代
7. **Production Systems** — 部署、云平台（AWS/GCP/Azure）、guardrails、并行处理

## 典型技术栈

- **Frontend**: React / Next.js
- **Backend**: FastAPI
- **AI orchestration**: LangChain、LangGraph、PydanticAI
- **LLMs**: OpenAI、Anthropic、Groq、本地模型
- **Vector DB**: Pinecone / Weaviate / Qdrant
- **Infra**: Docker + Kubernetes + Cloud
- **Monitoring**: OpenTelemetry、Grafana
- **Evaluation**: LLM judges、Evidently

## 技能优先级

### Must-Have
Python、Prompt engineering、RAG systems、一种云平台、Docker

### High-Value
LangChain/PydanticAI、FastAPI、TypeScript、CI/CD、Kubernetes、PyTorch 基础

### Differentiators
Agent frameworks（LangGraph、CrewAI）、Fine-tuning、Evaluation systems、Vector databases、Multi-agent architectures

## AI Engineering 的本质

AI engineering 并不是追逐最新 hype 工具。**它的本质，是构建可靠的系统，而 LLM 只是其中的一个组件。** 重点应放在 RAG、Agents、Evaluation 和 Production systems 上。

## 参考来源

- Alexey Grigorev 的 [AI Engineering Field Guide](https://github.com/alexeygrigorev/ai-engineering-field-guide) — 该路线图的大量基础来源
