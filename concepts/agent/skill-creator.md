---
title: Skill Creator
created: 2026-05-15
updated: 2026-05-15
type: concept
tags: [agent-framework, practice]
sources:
  - raw/articles/2026-05-15_SkillFactory：三天手搓面向Harness设计的技能工厂（附AIcoding实践）.md
  - raw/articles/2026-05-14_当我把AI变成一个算法：Skill工程化设计的心路历程.md
  - raw/articles/2026-05-14_SkillsPluginsMCPAgent到底是什么.md
---

# Skill Creator

用于自动生成 AI Agent [[agent-skill-specification|Skill]] 的工具/方法论。当前主要分为三类实现：

## 三类 Skill Creator

| 来源 | 特点 |
|------|------|
| Anthropic 官方 skill-creator | [[claude-code|Claude Code]] 内置，基于对话生成 Skill |
| [[openclaw|OpenClaw]] skill-creator | 开源，社区广泛使用 |
| Superpowers writing-skills | 基于 Superpowers 框架（GitHub 138K stars），结构化工作流驱动 |

## Skill 生成的工程化挑战

### 单一路径的不确定性

LLM 执行生成本身具有随机性，单一 skill-creator 的输出质量不稳定。[[skillfactory|SkillFactory]] 的解决方案是多路并发：并行调用 3 种策略同时生成，择优录用。

### 验证闭环的缺失

对话生成 Skill 缺乏自动化验证：
- 不确定生成的 Skill 是否真的能解决目标问题
- 没有基线对照（裸模型就能解决的不需要 Skill）
- 无法自动检测与已有 Skill 的重复

### 从 Trace 到 Skill

千问团队提出 Trace2Skill 方向：将 Agent 执行轨迹蒸馏为可复用的 Skill。不需要人工编写，不更新模型参数，通过轨迹分析提炼能力。

与 [[skill-design-patterns|Skill 设计模式]]、[[agent-skill-evaluation|Skill 评估]] 框架和 [[self-improving-agent|自进化 Agent]] 中的 Skill 自动沉淀机制密切相关。
