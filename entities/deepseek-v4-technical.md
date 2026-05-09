---
title: DeepSeek V4 — 技术细节
created: 2026-04-30
updated: 2026-05-01
type: entity
tags: [model, architecture, training]
sources:
  - raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md
  - raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md
  - raw/articles/2026-04-25-deepseek-v4-review-collection.md
---

> 子页面 — 详见主页面 [[deepseek-v4]]

# DeepSeek V4 — 技术细节

## 架构创新

### Hybrid Attention：CSA + HCA

V4 的 MoE 框架沿用 V3 的 DeepSeekMoE，注意力机制升级为两种层交错使用的混合架构。核心动机：标准 Transformer 在 1M 上下文下计算量膨胀 6.5 万倍、KV Cache 膨胀 256 倍，必须跳出"每一对都算"的范式。

- **CSA（Compressed Sparse Attention）**：每 m 个 token 的 KV 压成一个 entry，再跑 DSA 稀疏注意力，每个 query 只关注 k 个压缩 entry。Flash 版 m=4，indexer query head 64个，sparse top-k=512。
- **HCA（Heavily Compressed Attention）**：更激进的压缩，每 m′=128 个 token 压一个，不做稀疏选择，保持稠密注意力。

**CSA 三层处理流程**：先通过 HCA 定位高关联度的"大块信息"（内容海选）→ 在这些信息中进行 CSA 的稀疏注意力计算（内容精选）→ 稀疏采样与精确计算。

**CSA 压缩机制**：将 token 分组（每组 m 个），组内带权重加和，连续两组的压缩内容拼接以保持语义连贯性。压缩权重矩阵 S 和值投影矩阵 C 由可学习参数训练得到。

**CSA 闪电索引**：快速计算每个 token 与所有"纪要"的关联度分数，每个注意力头加权后选 top-k 份纪要供后续注意力计算使用。

**HCA 压缩机制**：与 CSA 类似但组更大（m′ 个 token），无需连续两组拼接，保持稠密注意力（直接逐一查阅所有纪要）。

**实践流程**：HCA（内容海选）→ CSA（内容精选）→ 稀疏采样与精确计算，三层处理大幅优化超长上下文中的注意力计算量和 KV-Cache 消耗。

**业界对比**：kimi、qwen 采用了混合线性注意力机制。线性注意力引入核函数打破 Softmax 约束，基于矩阵乘法结合律先算 K×V 再作 Q 计算，将复杂度从 O(L²) 降至 O(L)，无需随序列长度增长的 KV-Cache。^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

两者共用 partial rotary 位置编码、attention sink 技巧和 sliding window attention 分支。^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

### mHC：流形约束的残差连接

mHC（Manifold-Constrained Hyper-Connections）将残差映射矩阵约束在双随机矩阵流形（Birkhoff polytope）上，保证谱范数有界、传播非膨胀，深层堆叠不跑飞。残差宽度与 hidden size 解耦，用 expansion factor n=4 控制开销，参数由输入相关和输入无关两部分动态生成。

**核心动机**：标准残差连接（单流直传）在百万参数、百层以上时存在三大问题：
1. **容量瓶颈**：所有层共用一条残差通路，浅层和深层信息相互干扰
2. **路由僵硬**：无法调节各前序层的贡献权重，重要层被非重要层稀释
3. **深度上限**：无衰减累加导致梯度消失/激活爆炸

**演进路径**：标准残差 → Hyper-Connections (HC) → mHC
- **标准残差**：单流直传，每层输入 = 所有前序层变换结果累加和，贡献均等
- **HC**：维护 n 个流（多通道），每次变换前降流为1，变换后升流为n，带权重残差加和。但残差映射矩阵 Hres、降流矩阵 Hpre、升流矩阵 Hpost 均无约束，易导致隐状态衰减/爆炸
- **mHC**：与 HC 公式一致，但将残差映射矩阵 Hres 限制为**双随机矩阵**（每行每列和=1，所有元素非负），Hpre/Hpost 通过 sigmoid 限制在(0,1)。利用双随机矩阵的乘法封闭性，确保历史层系数始终在(0,1)之间，从根本上解决梯度问题。理论上可达 m 种函数表示路径（标准残差仅1种）

**业界对比**：kimi 提出了注意力残差机制 (Attn-Res) 和分块注意力残差 (Block Attn-Res)，同样将无权加和改为有权加和，但使用注意力机制计算权重。分块方案（块数=8时效果接近全注意力）解决了每层回看所有历史层开销过大的问题。^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

### Muon 优化器

大部分模块从 AdamW 换成 Muon，采用改进的 Hybrid Newton-Schulz 迭代做矩阵正交化，叠加 Nesterov trick 和 RMS rescaling。embedding、prediction head、静态 bias、RMSNorm 保留 AdamW。

**两项关键举措**：
1. **梯度正交化**：将梯度正交化后各方向独立更新，减少震荡发散，提升稳定性同时加速收敛。类比：两只手拧魔方，互不牵扯时每一下都是真实进步。
2. **QK RMSNorm**：Logits 爆炸会导致 Softmax 分布极端化和训练震荡。V4 在 Q、K 计算注意力前先进行 RMSNorm，在 Softmax 计算前规避 QK^T 矩阵中少数值过大的问题，从根本上解决 Logits 爆炸。类比：给每人发定制麦克风增益，确保每个人都能被公平听见。^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

### MoE 改进

门控函数从 Sigmoid 换成 Sqrt(Softplus)，前几层 dense FFN 换成 Hash 路由的 MoE，采用 auxiliary-loss-free 负载均衡和 sequence-wise balance loss。

## 训练方法

### OPD（On-Policy Distillation）

后训练最关键的方法学替换：将 V3.2 的 SFT + mixed RL 整体换成 OPD，分两步：

1. **领域专家培育（Specialist Training）**：每个目标领域（数学、代码、Agent、指令跟随等）单独训练专家模型，走 SFT 打底 + GRPO RL 流程，每个领域训三种推理强度子版本（Non-think、Think High、Think Max）。
2. **OPD 融合**：将所有专家合到一个学生模型里，让学生在自己生成的 trajectory 上学多个 teacher 的 output 分布。

### 三种思考强度

- **Non-think**：直觉式回应，短上下文窗口
- **Think High**：逻辑分析，128K 上下文
- **Think Max**：极限推理，384K 上下文，把推理预算拉满

### Quick Instruction 机制

给输入序列直接附一组 special token 对应附加任务（如搜索触发、意图识别），复用 KV cache 省掉冗余 prefill，降低 TTFT。^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

## FP4 量化感知训练

DeepSeek 首次在旗舰模型上全面跑 FP4 量化感知训练（QAT）。核心思想：在训练过程中让模型"预演"低精度计算，提前适应数值扰动，部署时吃到推理加速 + 显存节省双重红利。分模块量化：
- MoE 路由专家参数用 FP4——参数量大，压缩收益最高
- CSA 闪电索引中的 q、K 用 FP4——长上下文热点路径
- 闪电索引的打分仅量化到 BF16——top-k 排序对数值精度敏感

采用 simulated quantization：forward 时 FP8 master weight 量化到 FP4，backward 时梯度直接传给 FP32 master weight（STE 穿透量化算子）。inference 和 RL rollout 直接用真 FP4 权重。当前硬件上 FP4×FP8 峰值算力与 FP8×FP8 相同，理论上新硬件可快 1/3。^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

## 性能评测（V4-Pro-Max）

### 代码
- LiveCodeBench Pass@1：**93.5**（对比组最高）
- Codeforces Rating：**3206**
- Apex Shortlist：**90.2**

### 知识
- SimpleQA-Verified：**57.9**（高于 Opus-4.6-Max 的 46.2 和 GPT-5.4-xHigh 的 45.3）
- MMLU-Pro：87.5

### Agent
- Terminal Bench 2.0：67.9
- SWE Verified：80.6
- MCPAtlas：73.6

### 长文本
- MRCR 1M：**83.5**（超过 Gemini-3.1-Pro 的 76.3）
- CorpusQA 1M：62.0

### 数学与形式推理
- Putnam-2025：**120/120 满分**（hybrid formal-informal 推理）
- HMMT 2026 Feb：95.2
- IMOAnswerBench：89.8

## 为什么需要 1M 上下文

三个典型场景驱动了超长上下文的需求：

1. **Agent 多轮任务**：一个跑 30 轮的 Coding Agent，每轮塞入用户指令、源文件、shell 输出、reasoning trace，30 轮下来数十万 token 是常态。V3.2 需要丢弃 thinking history 不是因为不想留，而是留不下。

2. **整仓库级代码理解/重构**：中型 Python 项目 300 文件 15 万行代码，tokenize 后约 80–120 万 token。RAG 方式漏一个调用点就是一个 bug；超长上下文让模型"一口气读完整个项目"，直接看见所有调用关系。V4 在 SWE Verified 上拿到 80.6 与此直接相关。

3. **长文档推理**：200 页法律合同、500 页学术综述、季度财报附件——单个文档 50–80 万 token 常见。以前切块摘要再合并逻辑缺失巨大；现在直接让模型在原始材料上做跨段落推理。V4-Pro 在 MRCR 1M 上拿到 83.5（开源最高）。^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

## 标准 Transformer 在 1M 上下文下的瓶颈

传统 Transformer 架构面临三个必须同时解决的问题：
1. **残差稳定**：标准残差在百层以上时层间通道单一，无法调节各层贡献，无衰减累加导致梯度消失/激活爆炸
2. **计算与存储**：Prefill 复杂度 O(L²)，L 提升 8 倍则 Prefill FLOPs 提升 64 倍；Decode 阶段每生成一个 token 需搬移全部 L 组 KV，显存和带宽压力与 L 成正比
3. **训练稳定**：万亿参数、更深网路的训练规范化

以上三个问题分别由 DeepSeek V4 的三项核心创新（mHC、CSA/HCA、Muon）回应。

## 基础设施优化

### 细粒度计算通信重叠

MoE 架构从"算完再通信，通信完继续算"（硬件利用率低）→ 计算通信重叠（矩阵运算同时并行分发/回收）→ 更细粒度调度（尽可能消除气泡）。当计算量/通信量比值 > 硬件算力/带宽比值时，通信可完全"隐藏"进计算，实现气泡完全消除。MoE 模块的硬件选型不必一味追求高带宽。^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

### TileLang 算子开发

TileLang 是一种面向 tile 的高级抽象语言，核心思想是数据流逻辑与调度策略解耦。V4 中的 mHC、CSA、HCA 等机制若纯靠 CUDA 编写算子非常复杂，通过 TileLang 编写的 kernel 性能对齐甚至超过手工 CUTLASS/cuBLAS。TileLang 可移植性好，同一套代码可针对 NVIDIA 和国产硬件等不同后端编译。^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

### 批无关性与计算确定性

**批无关性（Batch-Invariant）**：无论 batch 如何切分，token 落在哪个 batch，输出结果在 bit 级别完全一致。通过将不同 kernel 计算顺序完全对齐 + 使用 DeepGEMM 矩阵乘法库实现矩阵乘法的批无关。

**计算确定性（Determinism）**：浮点加法不满足结合律，大规模累加（注意力反向传播、MoE 反向传播、mHC 的 split-k 归约）的执行顺序不同会导致结果不同。V4 强制要求这三个场景的累加顺序固定，实现 bit 级确定训练。^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

### 训练框架优化

1. **Muon 与分布式训练共存**：Muon 需一次性拿到完整参数梯度，与传统分布式训练切分参数矛盾 → 重新设计参数分组和分配方式，机器间通信量砍半
2. **mHC 流水线优化**：新残差结构占更多显存、机间传输更多数据 → 定制计算内核 + 选择性重计算 + 调整流水线节奏，额外耗时压至 6.7%
3. **长文本训练跨机器压缩**：KV 压缩段跨机器边界时 → 两步走机器间通信：相邻机器交换边界数据，压缩结果收拢重排
4. **显存-算力精细权衡**：传统做法粒度太粗（整层存或整层重算）→ 精细到单个 tensor 的自动化机制，自动算出重算哪些最省

### 推理框架优化

1. **KV Cache 架构**：V4 的 CSA/HCA 混合注意力使每层 KV 结构、大小、更新规则各不同，传统 PagedAttention（假设所有层 KV 形状一致）失效 → 专门设计包括滑动窗口注意力缓存 + CSA/HCA 混合注意力的 KV Cache 架构
2. **KV Cache 持久化**：将算过的 KV 持久化到磁盘，下次命中直接读跳过重算。V4 创新点：面对异构 KV（CSA/HCA 压缩 + SWA 未压缩 + 尾部 state），拆开分类存储，为 SWA 提出三档取舍方案^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

## 架构演进启示

V4 没有发明全新的轮子。mHC、CSA/HCA、Muon、TileLang、QAT——每一块拆开看原理都似曾相识，但 V4 将它们逐一重新雕琢并组织在一起，完成了从架构到工程的一次系统级闭环。它让开源大模型又向前迈出了决定性的一步。^[raw/articles/2026-04-29_读完这篇，你就搞懂DeepSeekv4了.md]

通过 CSA + HCA 混合注意力机制，V4-Pro 在百万上下文下的单 Token 推理算力需求仅为 V3.2 的 **27%**，KV Cache 内存占用低至 **10%**。Flash 版 API 成本极低（缓存命中 0.2 元/M token），推理速度极快，适合大规模高频商业应用。^[raw/articles/2026-04-25-deepseek-v4-review-collection.md]

## 相关页面

- [[deepseek-v4]] — 主页面，包含概述、模型规格与关键对比
- [[deepseek-v4-market]] — 成本优势、定价竞争与市场影响
- kimi — Moonshot 旗下模型，采用混合线性注意力机制
- qwen — 阿里通义千问，也采用了混合线性注意力机制
