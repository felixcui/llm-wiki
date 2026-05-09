---
title: 池建强（Chi Jianqiang）
created: 2026-04-29
updated: 2026-04-29
type: entity
tags: [person]
sources:
  - raw/articles/2026-04-29_MCP-production-agents.md
---

# 池建强（Chi Jianqiang）

**知名科技作者、墨问（MoWen）产品创始人**
**运营公众号**: MacTalk

## 概述

池建强是中国科技行业知名作者和产品人，长期运营微信公众号 **MacTalk**，在 AI Agent、开发者工具和科技趋势领域有持续深度输出。他是 **墨问（MoWen）** 产品的创始人，该产品提供 CLI 工具和 MCP 服务（基于 Streamable HTTP 协议的远程 MCP Server）。

## 代表观点

- 三种 Agent 集成外部系统方式的对比分析：API 直连、CLI + Skills、MCP
- 支持 Anthropic 的判断——生产级 Agent 越来越多运行在云端，MCP 是生产系统的核心连接层
- 提出 "MCP 是给 AI Agent 用的 UI，不是给开发者用的 SDK" 的设计理念
- 倡导将 MCP 工具设计为**任务单元**（对应完整目标），而非后端的原子操作
- 预测未来产品设计需同时考虑"人怎么用"和"Agent 怎么用"

## MCP 实践

池建强主导的墨问 MCP 采用远程 MCP Server 架构（基于 Streamable HTTP 协议），一次安装即可在 Web、移动端和云托管 Agent 中使用，不依赖本机环境和沙箱 Shell。

## 相关条目

- [[mcp-model-context-protocol]] — 其重点讨论的技术协议
- [[anthropic]] — 其引用的核心来源
- [[agent-first-design]] — 其"为 Agent 设计 MCP 工具"方法论的理论基础
