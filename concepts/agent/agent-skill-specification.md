---
title: Agent Skill 规范
created: 2026-04-25
updated: 2026-04-28
type: concept
tags: [agent-framework, open-source]
sources:
  - raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md
  - raw/articles/2026-04-25_开源一个PPTSkill｜压进了我10年的设计经验.md
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md
  - raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md
  - raw/articles/2026-04-28-google-agent-skill-toolkit.md
  - raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md
  - raw/articles/2026-04-23_软件正走向无界面化！旧金山创始人：智能体操作员将创造几十万岗位！AI不会取代人类！认同黄仁勋立场，硅谷在被中国开源模型反向赋能.md
---

# Agent Skill 规范

Agent Skill 是一种将领域知识和操作规范封装为可复用模块的标准化方法，正在成为 AI Agent 生态中的核心复用单元。

## 什么是 Agent Skill

Agent Skill 本质上是一份"技能说明书"——告诉 Agent 遇到某类任务该怎么做、遵循什么规则、产出什么格式。它是结构化的自然语言文档，配合少量配置文件，让 Agent 能够按既定规范执行复杂任务。^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]

## SKILL.md 文件结构

SKILL.md 是 Skill 规范的核心文件，定义目标、适用场景、执行规范、输出格式和约束与边界。对于 Team Skill（多 Agent 协作），还需补充 `roles/` 和 `workflow.md`。

## 5 个核心设计原则

1. **规则显式化** — 将隐含的行业经验转化为 Agent 可执行的量化规则（如字体分级、色彩纪律）
2. **约束优先于自由** — 严格约束保证输出质量（禁止自定义颜色，只暴露有限选项）
3. **前置对齐，后置执行** — 执行前与用户澄清目标和约束，拦截 80% 返工
4. **稳定前缀不乱动** — Skill 通过 User Message 注入以保护 Prompt Cache
5. **来源分级信任** — built-in / trusted / community / agent-created 四级信任策略

## 动态 Skill 生成

[[hermes-agent]] 将 Skill 从静态配置升级为动态资产：基于运行轨迹自动生成新 Skill、持续优化已有 Skill、连续 10 轮未创建/修改时自动催促审查。后台审查 Agent 从记忆审查、技能审查、综合反思三个维度判断是否更新。^[raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md]

## 相关子页面

- [[skill-design-patterns]] — 5 种核心设计模式、反偷懒技巧、3 层知识架构
- [[agent-skill-evaluation]] — 8 维度 Skill 评估框架与多模型交叉验证
- [[agent-skill-distillation-vs-rag]] — Skill 蒸馏与 RAG 的正确组合
- [[agent-skill-ecosystem]] — 跨框架生态、[[skvm]] 编译优化层、应用案例

## 跨框架 Skill 生态

| 框架 | Skill 支持 | 特点 |
|------|-----------|------|
| [[claude-code]] | CLAUDE.md / Agent Skills | 原生支持，社区生态丰富 |
| [[openclaw]] | 静态 Skill 文件 | 基础支持，作为配置文件 |
| [[hermes-agent]] | 动态 Skill + 自进化 | 自动生成与持续优化 |
| JiuwenClaw | Agent Skills + Team Skills | 单 Agent + 多 Agent 全覆盖 |

Google 于 2026 年 4 月开源 Agent Skills（github.com/google/skills），标志着 Skills 向厂商级标准迈进。^[raw/articles/2026-04-28-google-agent-skill-toolkit.md]

## Skill 与规范驱动开发（SDD）

Skill 规范与 [[spec-driven-development]]（SDD）高度互补：SDD 强调"先写规范再写代码"，Skill 是将领域经验编码为可执行的规范文档。[[spec-kit]] 的 Constitution、[[openspec]] 的 Delta Specs 都可视为 Skill 在编程领域的具体实践。^[raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md]

## Skill 作为新兴岗位的核心技能

[[aaron-levie]]（Box CEO）预测"智能体操作员"将催生 50-100 万个新岗位，需要"懂 MCP、CLI，会编写 Skills"。编写 [[agent-skill-specification|Skill]] 正从开发者的附加技能转变为企业 AI 落地的核心能力。^[raw/articles/2026-04-23_软件正走向无界面化！旧金山创始人：智能体操作员将创造几十万岗位！AI不会取代人类！认同黄仁勋立场，硅谷在被中国开源模型反向赋能.md]
