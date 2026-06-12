---
title: MCP (Model Context Protocol)
created: 2026-05-09
updated: 2026-05-15
type: entity
tags: [tool, open-source]
sources:
  - raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md
  - raw/articles/2026-05-14_SkillsPluginsMCPAgent到底是什么.md
  - raw/articles/2026-05-14_从零设计生产级Multi-AgentHarness：架构、评估、记忆、成本与MCP工具接入全拆解.md
---

# MCP (Model Context Protocol)

Agent与外部系统之间的标准化连接层，由 [[anthropic]] 推动。^[raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md]

## 功能定位

MCP定义了工具注册、调用和结果返回的标准协议，是实现"Agent作为一等公民"的技术基础。

## 在Claude Code生态中的角色

Boris Cherny在Sequoia对话中强调MCP是 [[claude-code]] 生态的关键基础设施，使Agent能够连接Slack、Linear等外部工具。^[raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md]

## 协议要素

- 工具注册（Tool Registration）
- 标准化调用接口
- 结果返回格式规范

## 相关

- [[anthropic]] — MCP推动方
- [[claude-code]] — 基于MCP构建的工具
- [[claude-code-capabilities]] — Claude Code能力体系
- [[agent-first-design]] — Agent优先设计理念
- [[chi-jianqiang]] — 提出"MCP 是 AI 的 UI"观点的创业者
