---
title: 世界模型 Agent
created: 2026-04-25
updated: 2026-04-25
type: concept
tags: [agent-framework, reinforcement-learning, inference]
sources:
  - raw/articles/2026-04-25-wang-zihan-leave-deepseek.md
---

# 世界模型 Agent（World Model Agent）

Agent 的决策从"对输入的响应"转变为"基于世界演化的判断"的研究方向。核心目标：让 Agent 在行动之前就在内部完成对未来预演与模拟，构建真正理解世界的 Agent。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

## 核心方法

### MDP 抽象

将 Agent 抽象为 MDP（马尔可夫决策过程），包含四个要素：状态（State）、动作（Action）、状态转移（Transition）、反馈（Reward）。基于这一抽象，研究 Agent 能力随规模变化的规律（Agentic Scaling Law）。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

### 完形填空挖掘世界建模能力

不局限于传统策略函数（给定状态输出动作），而是对 MDP 任意环节进行完形填空：通过动作预测下一状态（正向），通过状态反推动作（反向）。这种训练方式迫使 Agent 建立对环境动态的深层理解。[[vagen]] 是这一思路的代表性工作。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

### RL 训练驱动进化

通过强化学习训练 Agent，使其决策模式从被动响应转变为基于世界演化的主动判断。[[ragen]] 是这一路线的代表性框架。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

## 环境决定论

Agent 的智能取决于环境的开放程度，这是一个关键的观察：^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

| 环境开放程度 | 代表产品 | 智能表现 |
|-------------|---------|---------|
| 完全开放计算机环境 | [[openclaw]]（OpenClaw） | Agent 能力上限最高 |
| 受限工具环境 | Claude Code / Codex | 适合特定领域任务 |
| 仅聊天界面 | GPT | 能力受限于对话上下文 |

环境越开放，Agent 需要处理的不确定性越大，但也越能展现真正的智能。

## 资源自适应 Agent

世界模型 Agent 的一个重要研究方向是**资源自适应**：给一万块算力做出一万块的效果，给一百万做出一百万的效果——Agent 应根据可用资源自动调整策略深度和推理复杂度。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]

## 与其他自进化方向的对比

| 维度 | 世界模型 Agent（RL 路线） | [[self-improving-agent]]（Skill 沉淀路线） |
|------|--------------------------|-------------------------------------------|
| 进化机制 | RL 训练改变模型权重 | 运行时积累 Skill 和 Memory |
| 核心能力 | 世界建模与预演模拟 | 经验复用与持续改进 |
| 代表工作 | [[ragen]]、[[vagen]] | [[hermes-agent]] |
| 研究阶段 | 学术前沿（NeurIPS 2025） | 工程落地（开源框架） |

两条路线不矛盾，未来可能融合：RL 训练提供基础世界理解能力，Skill 沉淀提供领域特定的高效策略。^[raw/articles/2026-04-25-wang-zihan-leave-deepseek.md]
