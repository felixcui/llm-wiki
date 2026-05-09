---
title: Hermes Agent
created: 2026-04-25
updated: 2026-04-28
type: entity
tags: [tool, agent-framework, open-source, fine-tuning]
sources:
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md
  - raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md
  - raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md
  - raw/articles/2026-04-22_万字保姆级教程：Hermes+KimiK2.6打造7x24hAgent军团.md
  - raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md
  - raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md
  - raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md
---

# Hermes Agent

**开发商**: Nous Research
**发布时间**: 2026年2月
**GitHub Stars**: 4.7万+（两个月内，截至2026年4月28日）^[raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md]
**类型**: 开源自进化 Agent 框架

## 概述

Hermes Agent 是由 Nous Research 推出的开源 Agent 项目，主打"持久运行"（Persistent）和"自进化"（Self-Evolving）两大核心能力。它是一个部署在服务器上的自主智能体——运行时间越长，能力就越强。^[raw/articles/2026-04-25_深度解析HermesAgent如何实现\&quot;自进化\&quot;及其PromptContextHarness的设计实践.md]

Hermes Agent 的热度已超过 [[openclaw]]，许多用户从"养虾"转向使用 Hermes，且官方支持从 OpenClaw 无缝迁移。它同时也是 [[claude-code]] 的重要开源替代。

## 子页面

- [[hermes-agent-architecture]] — 五层架构、PTC、delegate_task、Session 链、Prompt/Context/Harness 三维设计
- [[hermes-agent-self-improving]] — 自进化机制、MemoryStore、Skill 自动创建与修补、Nudge Engine、安全与自我修复

## 与其他框架的关系

- **兼容 [[openclaw]]**：可直接读取 AGENT.md、SOUL.md、USER.md 等配置文件，零成本迁移
- **兼容 [[claude-code]]**：支持读取 CLAUDE.md、.cursorrules 等编码规范文件
- **多模型支持**：兼容 Claude、GPT/Codex、Gemini/Gemma 等主流模型
- **兼容 [[skvm]]**：SkVM（上海交大 IPADS 团队的 Skill 语言虚拟机）可无缝接入 Hermes，通过 AOT/JIT 编译优化 Skill 执行效率，降低 token 消耗至多 40%，代码执行加速至多 50 倍。^[raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md]

## 深度访谈中的定位（2026-04-29）

在 [[feng-chengcheng]] 和 [[yao-liuyi]] 的深度访谈中，HermesAgent 被定位为 OpenClaw 之外的另一条技术路线：^[raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md]

### 与 OpenClaw 的架构对比

| 维度 | OpenClaw | HermesAgent |
|------|----------|-------------|
| 风格 | "人教 Agent 做事" | "Agent 自己学会做事" |
| 记忆 | 文本文件 + 上下文透传 | 短期/中期/长期/技能记忆体 |
| Skills | 需用户预先安装 | 可在执行中自动沉淀 |
| 进化 | 依赖社区贡献/版本升级 | Learningloop 学习闭环 |
| 适合场景 | 确定性强、流程清晰 | 目标模糊、需探索优化 |

### 核心优势：Learningloop

HermesAgent 的核心优势在于 **Learningloop（学习闭环）**：Agent 可空闲时反复查看过去执行轨迹，总结经验并沉淀为可复用能力。配合 RL 强化学习训练，可形成"执行→记录→沉淀→优化→再执行"的完整闭环。

### 行业评价

- 代表"潜力更大，能力天花板更高"的技术路线
- 自进化消耗更多 Token 和计算资源，需权衡稳定可控与成长潜力
- 长期趋势：企业级 Agent 应融合 OpenClaw 的稳定可控底座和 Hermes 的自我进化能力
