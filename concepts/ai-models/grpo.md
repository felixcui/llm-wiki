---
title: GRPO（Group Relative Policy Optimization）
created: 2026-04-25
updated: 2026-04-25
type: concept
tags: [training, fine-tuning, optimization]
sources:
  - raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md
---

# GRPO（Group Relative Policy Optimization）

GRPO 是一种强化学习算法，由 DeepSeek 在 R1 模型中首次提出。其核心创新在于**不需要单独训练 Reward Model**，而是通过同一组 prompt 下多个采样的相对比较来计算奖励信号，大幅降低了 RLHF 的工程复杂度和成本。

## 算法原理

传统 RLHF 流程需要：训练 SFT 模型 → 训练 Reward Model → 用 PPO 等策略梯度算法优化。GRPO 简化了这一流程：

1. **组采样**：对同一个 prompt，从当前策略中采样 G 个输出（一组 response）
2. **相对奖励**：对每个输出计算奖励分数（可来自规则、代码执行结果、模型自评等），然后在组内做归一化
3. **策略优化**：基于 KL 散度约束的策略梯度更新，鼓励高奖励行为、抑制低奖励行为

关键优势在于：奖励信号来自组内相对比较而非绝对评分，因此不需要一个独立的、高质量的 Reward Model。^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

## 在 DeepSeek V4 后训练中的应用

[[deepseek-v4]] 的后训练将 V3.2 的 mixed RL 阶段整体替换为 **On-Policy Distillation（OPD）**，其中 GRPO 是核心 RL 算法。

### 领域专家培育

每个目标领域（数学、代码、Agent、指令跟随等）单独训练专家模型，流程为：

1. **SFT 打底**：领域监督微调建立基础能力
2. **GRPO 强化**：用 GRPO 做 RL，每个领域配自己的奖励模型
3. **多强度子版本**：训练 Non-think、Think High、Think Max 三种推理强度，分别对应不同上下文窗口和 length penalty^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

### On-Policy Distillation 融合

所有领域专家通过 OPD 合并到一个学生模型中。做法是让学生在自己生成的 trajectory 上学多个 teacher 模型的输出分布。这个范式比传统 SFT 蒸馏更接近 RL 精神，因为分布匹配是在学生当前策略下的状态分布上做的。^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

### 工程挑战

- 百万 token 上下文的 RL 需要专门优化 rollout 数据的存储和加载
- Agent 训练依赖 DSec sandbox 基础设施，单集群扛几十万 sandbox 并发
- Teacher 权重统一存储，按需加载，I/O 和 DRAM 都需减压

## 在 Hermes RL 训练中的应用

[[hermes-agent]] 内置了完整的 RL 训练框架（"Research-Ready"），同样使用 GRPO 作为核心算法，主要目的是**知识蒸馏**——将 Claude Opus 等大模型的能力压缩到 Qwen 3~4B 等小模型。

### 训练流程

1. **任务定义**：指定训练目标和数据集
2. **轨迹捕获**：batch_runner.py 并行生成 Agent 轨迹，默认用 Claude Opus 4.6 作为 Teacher 模型
3. **轨迹压缩**：将原始轨迹（可能几十万 token）压缩至 ~15,250 token，保护头部任务定义和尾部对话，中间用 Gemini Flash 生成摘要
4. **渐进式训练**：小规模验证 → 正式训练 → 自动评估（WandB 指标）^[raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md]

### 奖励函数设计

| 维度 | 权重 | 说明 |
|------|------|------|
| 正确性 | 2.0 | 最高优先级 |
| 格式规范 | 0.5 | 输出格式合规 |
| 渐进格式 | 0~0.5 | 部分符合也给分 |

支持通过 ToolContext 执行终端命令、读取文件、访问网络做"真实验证"。^[raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md]

## GRPO 的意义与局限

**优势**：消除 Reward Model 训练成本；组内相对比较比绝对评分更鲁棒；适合代码、数学等有明确正误标准的领域。

**局限**：组采样增加了推理计算量；对于主观性强的任务（如创意写作），奖励信号不如 Reward Model 精细；百万 token 上下文下的 RL 工程挑战巨大。
