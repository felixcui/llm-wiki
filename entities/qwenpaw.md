---
title: QwenPaw
created: 2026-04-29
updated: 2026-04-29
type: entity
tags: [tool, agent-framework, open-source, company]
sources:
  - raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md
---

# QwenPaw

**别名**: coPaw（前身）
**开发商**: 阿里巴巴（ATH 事业群通义大模型事业部）
**类型**: 开源 Agent 产品
**负责人**: [[yao-liuyi]]（姚柳佚）

## 概述

QwenPaw 是阿里巴巴推出的开源 Agent 产品，前身为 coPaw。名称含义："Qwen"代表拥抱 Qwen 开源生态，"Paw"（小爪子）代表轻量陪伴的初心。旨在提供轻量、易部署、能自然融入用户日常场景的个人助手。^[raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md]

## 产品特性

### 模型与部署

- 坚定支持**本地模型部署**，满足大型国企等客户的数据安全需求
- 支持大模型协同、端云协同
- 针对 Agent 场景在模型能力上持续优化适配^[raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md]

### 任务执行

- 新版本设计 **Mission 模式**：支持 Agent 自主规划任务并巡视任务是否完成
- 引入**插件机制**：处理 Agent 部署中的"接缝"工作（日志平台、监控平台、自定义安全规则等）^[raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md]

### 协议兼容

- 支持包括 **ACP 协议**在内的多种开源协议
- 便于与企业现有系统和工具无缝对接^[raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md]

## 定位

QwenPaw 属于 [[openclaw]] 同类产品（"养虾"类），但强调更轻量、更简单地部署，更自然地进入用户日常场景。未来愿景是让 Agent "像一个戴着安全帽、穿着工作服的正式工，真正融入企业 IT 系统"。^[raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md]
