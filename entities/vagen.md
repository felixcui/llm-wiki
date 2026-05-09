---
title: VAGEN
created: 2026-04-25
updated: 2026-04-25
type: entity
tags: [model, agent-framework, computer-vision]
sources:
  - raw/articles/2026-04-25-wang-zihan-leave-deepseek.md
---

# VAGEN（Vision Agent）

**类型**: Vision Agent 框架
**发表**: NeurIPS 2025
**作者**: 王子涵（DeepSeek 前研究员，现西北大学直博二年级）

## 概述

VAGEN 是一个发表在 NeurIPS 2025 的 Vision Agent 研究工作，核心思路是将 Agent 所在的 MDP（马尔可夫决策过程）视为一个需要理解和预测的世界模型，通过对 MDP 任意环节进行"完形填空"来挖掘 Agent 的世界建模能力。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

## 核心方法：MDP 完形填空

传统 Agent 训练只关注策略函数——给定状态，输出动作。VAGEN 的创新在于不局限于策略，而是对 MDP 的任意环节进行"完形填空"：^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

- **正向预测**：给定当前状态和动作，预测下一状态
- **反向推理**：给定目标状态，反推需要的动作

通过这种双向完形填空训练，Agent 被迫建立对环境动态的深层理解，而非仅仅记忆策略规则。这使 Agent 在面对新场景时具备更强的泛化和推理能力。

## 相关研究

- **[[ragen]]**：同属王子涵实验室的研究脉络，用 RL 训练 Agent 实现自我进化
- **[[world-model-agent]]**：VAGEN 是"世界模型 Agent"方向的核心工作之一
- **[[deepseek-v4]]**：王子涵曾在 DeepSeek 工作，DeepSeek 的 RL 研究传统对 VAGEN 有间接影响
