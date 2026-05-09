---
title: RAGEN
created: 2026-04-25
updated: 2026-04-25
type: entity
tags: [tool, agent-framework, reinforcement-learning, open-source]
sources:
  - raw/articles/2026-04-25-wang-zihan-leave-deepseek.md
---

# RAGEN

**类型**: Agent RL 框架
**基础架构**: 基于 Verl 构建
**初版发布**: 2025-01-27
**作者**: 王子涵（DeepSeek 前研究员，现西北大学直博二年级）

## 概述

RAGEN 是一个用强化学习训练 Agent 的开源框架。核心思想是让 Agent 的决策从"对输入的响应"转变为"基于世界演化的判断"——Agent 不再只是被动回答问题，而是通过 RL 训练获得在环境中主动行动和自我进化的能力。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

RAGEN 基于伯克利开源的 Verl（强化学习训练框架）构建，初版于 2025-01-27 发布。

## 核心设计

### 从响应到判断

传统 LLM 的决策模式是对输入做出响应，RAGEN 通过 RL 训练让 Agent 学会基于"世界演化"的状态来做判断——Agent 需要理解当前环境的完整状态和动态变化，而非仅关注当前输入。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

### 第二代：关注推理失败案例

第一代 RAGEN 验证了 RL 训练 Agent 的可行性。第二代将焦点转向推理失败案例——研究 Agent 在哪些环节会做出错误决策，并通过 RL 训练针对性地修复这些失败模式。王子涵在访谈中提到经历了多次研究失败后才总结出这一新论点。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

## 相关研究

- **[[vagen]]**（Vision Agent）：同属王子涵实验室的研究脉络，NeurIPS 2025，用"完形填空"思路挖掘世界建模能力
- **[[world-model-agent]]**：RAGEN 和 VAGEN 共同指向的研究方向——让 Agent 在行动前完成对未来预演与模拟
- **[[self-improving-agent]]**：同为 Agent 自进化方向，但路径不同——RAGEN 走 RL 训练路线，Hermes 走 Skill 沉淀路线
- **[[deepseek-v4]]**：王子涵曾在 DeepSeek 工作，RAGEN 的研究思路部分源于 DeepSeek 的 RL 研究传统
