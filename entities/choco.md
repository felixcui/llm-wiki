---
title: Choco
created: 2026-04-29
updated: 2026-04-29
type: entity
tags: [company]
sources:
  - raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md
---

# Choco

**类型**: AI 食品分销平台
**总部**: 柏林（2018 年创立）
**创始人**: Daniel Khachab, Julian Hammer
**融资**: 累计 328M+ 美元，食品分销技术领域第一家独角兽

## 概述

Choco 是一家面向餐饮分销商的 AI 平台，用 AI Agent 替代传统订单台人工录入流程。平台通过 OrderAgent（异步收单）和 VoiceAgent（实时语音电话）两种 Agent 产品，将门店发来的电话、短信、邮件、图片、传真、手写便条等非结构化信息自动转化为结构化 ERP 订单。^[raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md]

目前服务 21000+ 家分销商，覆盖 100000+ 家餐饮买家，业务覆盖美国、英国、法国、德国、西班牙、UAE 六国。

## 核心产品

### OrderAgent（订单 Agent）

接收所有非电话输入（邮件、短信、图片、文档），输出结构化 ERP 订单。关键工程挑战是**动态 in-context learning**——将每位客户特定的 SKU 映射、单位偏好、配送规律等隐式上下文编码进推理层进行消歧。使用 OpenAI 的 speech-to-text、embeddings、function calling API，自建 evaluation framework（ground-truth 数据集 + 连续监控 + A/B 测试）。^[raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md]

### VoiceAgent（语音 Agent）

基于 OpenAI Realtime API 构建，sub-second 延迟，24/7 自动接听电话。支持多语言，门店拨打同一号码、以相同方式沟通。可查库存、缺货时推荐替代品、临期商品做促销。2025 年 12 月初由 Choco 和 OpenAI 联合发布，被称为「食品行业第一个 AI Voice Agent」。^[raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md]

## 核心成绩

| 指标 | 数据 |
|------|------|
| 年处理订单 | 8.8M+ 全部走 AI |
| 生产环境 Token 消耗 | 200B+ |
| 手动录入下降 | 50% |
| 销售团队生产力 | 2x（不加人） |
| 订单错误率 | 1-5%（自动化阈值可配置） |
| 收单时间 | 24/7，无延迟 |
| 新分销商 2-3 周准确率 | 90-97% |
| 每周节省工时 | 15h+ |
| 每单手动工作量降幅 | 70% |
| Autopilot 全自动订单占比 | 50% |

## 工程方法论

Choco 在 OpenAI 案例研究中总结了三条生产 AI 工程原则：^[raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md]

1. **Evaluation 从第一天就跑** — 哪怕只有 10-20 个 ground-truth 例子也能度量进展
2. **AI-native observability** — 捕获模型输入、输出、reasoning trace，超越传统日志范围
3. **管理概率系统的预期** — 教育团队和用户接受 LLM 输出的概率波动，建立信任

## 发展方向：Agent Orchestrator

Choco 正在将 Agent 推进销售、电商、供应链等更多业务流，并定义了一类新角色——**[[agent-orchestrator]]**：不写代码但设计和管理 Agent 的人。标志着从 workflow software 向 AI execution infrastructure 的转型。^[raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md]

## 相关条目

- [[agent-orchestrator]] — Choco 定义的非工程师 Agent 设计/管理新角色
- [[ai-native-organization]] — AI Native 组织形态，Choco 是典型实践案例
- [[agent-first-design]] — Agent First 设计，Choco 的 OrderAgent/VoiceAgent 是生产级示例
