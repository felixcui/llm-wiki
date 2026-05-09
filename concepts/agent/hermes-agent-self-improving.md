---
title: Hermes Agent Self-Improving
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [agent-framework, optimization, fine-tuning]
sources:
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md
  - raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md
  - raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md
  - raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md
  - raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md
---

# Hermes Agent Self-Improving

> 自进化子页面 — 详见主页面 [[hermes-agent]]。

## 自进化机制：内外双轮驱动

### 路径一：动态 Skill 生成（外挂式进化）

与 [[openclaw]] 的静态 Skill 不同，Hermes 将 Skill 变为动态、可进化的资产：^[raw/articles/2026-04-25_深度解析HermesAgent如何实现\&quot;自进化\&quot;及其PromptContextHarness的设计实践.md]

- **自动生成**：基于 Agent 运行轨迹自动生成新 Skill
- **持续优化**：后续任务中发现更优路径时更新已有 Skill
- **持续积累**：Skill 库随使用不断丰富

**触发机制**：根目录 run_agent.py 中维护 `_iters_since_skill` 计数器，连续10轮未创建/修改 Skill 时系统自动"催促"。

**双信号 Skill 积累机制**：^[raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md]
1. **信号一（主动引导）**：`SKILLS_GUIDANCE` 在系统提示词中引导模型"复杂任务完成后主动存、用到过时的技能立即 patch"
2. **信号二（后台强制复盘）**：每 10 轮推理触发 `_spawn_background_review()`，后台线程 fork 一个 mini Agent（最多 8 轮、quiet mode），拿完整对话判断是否值得存技能。背景线程在回复已发给用户之后才启动，不与用户任务竞争模型注意力

**后台审查 Agent**：每次主 Agent 完成回复后，异步 Fork 一个轻量级审查 Agent，从三个维度审查：
1. **记忆审查**（_MEMORY_REVIEW_PROMPT）：提取值得长期保留的经验
2. **技能审查**（_SKILL_REVIEW_PROMPT）：判断任务模式是否值得抽象为 Skill
3. **综合审查**（_COMBINED_REVIEW_PROMPT）：反思执行过程中的优化空间

### 路径二：RL 训练闭环（权重内化进化）

Hermes 内置完整的 RL 训练框架（"Research-Ready"），核心使用 **GRPO** 算法：^[raw/articles/2026-04-25_深度解析HermesAgent如何实现\&quot;自进化\&quot;及其PromptContextHarness的设计实践.md]

**训练流程**：
1. **任务定义**：指定训练目标和数据集
2. **轨迹捕获**：batch_runner.py 并行生成 Agent 轨迹，默认用 [[claude-code|Claude Opus 4.6]] 作为 Teacher 模型
3. **轨迹压缩**：trajectory_compressor.py 将原始轨迹（可能几十万 token）压缩至 ~15,250 token，保护头部任务定义和尾部4轮对话，中间部分用 Gemini Flash 生成摘要
4. **渐进式训练**：小规模验证 → 正式训练 → 自动评估（WandB 指标）

**奖励函数设计**：
- 正确性：权重 2.0（最高）
- 格式规范：权重 0.5
- 渐进格式：0~0.5（部分符合也给分）
- 支持通过 ToolContext 执行终端命令、读取文件、访问网络做"真实验证"

**实际意义**：主要目的是知识蒸馏——将 Claude Opus 等大模型能力压缩到 Qwen 3~4B 等小模型，实现降本、提速和本地化部署。

## MemoryStore 详细实现

Hermes 的 Memory 系统设计极为克制——仅用两个纯文本文件（MEMORY.md 和 USER.md），用 `§` 分隔条目，分别存储 Agent 的环境事实和用户认知。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

### 字符限制与信息压缩

MEMORY 限 **2200 chars**，USER 限 **1375 chars**。容量有限迫使 Agent 挑重要的记，不重要的自然被挤掉——对比 [[openclaw]] 的纯追加模式（几个月膨胀成几万行），Hermes 倒逼 Agent 做信息压缩。

超限时 `add` 操作直接失败，返回当前所有条目让模型自己决定哪些过时该删、哪些可合并——模型不是被动执行淘汰规则，而是主动做信息整理。

### 冻结快照与 Prefix Cache 保护

每次会话启动时，Memory 加载后立刻捕获一份快照（`_system_prompt_snapshot`），之后系统提示词里用的都是这份快照。**会话内不变**，以保护前缀缓存（Prefix Cache），省掉重复计费。新写入只改磁盘，下一个会话才刷新。

### 声明式事实 vs 操作步骤分离

Memory 要求写成声明式事实（"User prefers concise responses" ✓），而非命令式指令（"Always respond concisely" ✗）。Tool Schema 明确规则——Memory 不存操作步骤，操作步骤归 Skill 管。

## Skill 自动创建与修补机制

### 创建触发条件

Skill 创建由 `skill_manage` 工具的 schema 驱动，门槛清晰：^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

- 工具调用超过 **5 次**才值得创建（简单任务不记）
- 踩过坑再修复的经验才有价值
- 用户纠正过的做法要铭记
- 发现非平凡工作流时主动记录

### Fuzzy Patch 自我修补

Agent 按已有 Skill 执行但发现步骤遗漏或踩了新坑时，会做精确局部 patch（而非全量重写）：使用 `fuzzy_find_and_replace` 做模糊匹配容忍格式差异，修改前备份原内容，修改后跑 `_security_scan_skill()` 安全扫描，不通过就自动回滚。^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

### 落盘原子替换

Skill 写入磁盘时遵循严格的原子替换纪律：先写入临时文件，再通过 `os.replace()` 原子替换目标文件，防止 TOCTOU 竞态条件。^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

### User Message 注入保护 Prompt Cache

Skill 内容不走 System Prompt 注入，而是通过 User Message 注入（带 `[SYSTEM: ...]` 前缀标记语义权重）。核心考量是保护 **Prompt Cache**：System Prompt 位于上下文最前端，任何变动都会导致整个前缀缓存失效；而 User Message 位于尾部，新增或修改 Skill 只影响后续增量部分。^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

### Patch 后最终一致性

Skill 被 patch 修改后，不会强制更新当前正在进行的会话上下文。当前会话仍使用旧版本 Skill 内容，新版本在下一个会话才生效。这种"最终一致性"策略牺牲了实时性，但避免了会话中途上下文突变导致的 Agent 行为不可预测。^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

### 渐进式加载

Skill 采用"动态图书馆"模式：系统提示词只放轻量索引（名字+一句话描述），Agent 判断相关时才通过 `skill_view` 加载完整内容——"先看目录再翻全文"，按需加载。系统提示词还灌输责任感："Skills that aren't maintained become liabilities"。

## 过程资产三层模型

Hermes 将 Agent 的长期资产分为三个独立管理的层次：^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

| 层次 | 功能 | 存储形式 | 生命周期 |
|------|------|---------|---------|
| **事实记忆**（Memory）| 记住用户偏好和环境事实 | MEMORY.md / USER.md | 会话间持久化，容量受限主动淘汰 |
| **会话检索**（Session Search）| 当前会话的上下文检索 | 内存中的对话历史 | 单会话内有效 |
| **过程资产**（Skill）| 沉淀可复用的操作步骤 | SKILL.md 文件 | 持久化，Agent 参与维护 |

核心洞察：Skill 不只是"人写给 Agent 的资料"，而是 **"Agent 参与维护的过程资产"**——Agent 在运行中创建、修补、优化 Skill，Skill 成为 Agent 能力增长的载体。^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

## Nudge Engine（后台审查引擎）

Nudge Engine 维护两个计数器，定时提醒 Agent 停下来反思：^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

- **Memory 计数器**：每 **10 个用户回合**触发一次审查
- **Skill 计数器**：每 **10 次迭代**触发一次审查（可配置）

### 后台 Fork Agent

Nudge 触发后不在主对话中插入审查消息（避免打扰用户），而是**后台 fork 一个独立 Agent 实例**，拿着主对话快照做静默审查。输出重定向到 /dev/null，最多 8 次工具调用，review agent 自身的 nudge 被禁用（避免无限递归），与主 Agent 共享 Memory。

两套审查提示词分别关注：Memory Review（用户偏好和个人信息）和 Skill Review（非平凡的解题过程），均以 "If nothing is worth saving, just say 'Nothing to save.'" 收尾，防止 review agent 每次都往里塞东西"交差"。

## Self-Improving 三子系统闭环

Memory（记住你是谁）+ Skill（记住怎么做事）+ Nudge Engine（保证循环不停转）构成完整的自我改进闭环：^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

实际案例（K8s 部署场景）：
- **第 1 次会话**（冷启动）：12 次工具调用，2 个错误 → 触发 Skill 创建
- **第 2 次会话**（Skill 复用）：9 次调用，1 个新错误 → Skill 自我修补 + Memory 写入
- **第 3 次会话**（全协同）：6 次调用，0 错误 → 全协同工作

## 安全机制

两层防护：^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

1. **Memory 内容扫描**：检测 prompt injection、deception、sys_prompt_override、exfil_curl 等威胁模式（因为 Memory 最终注入系统提示词，被诱导写入恶意内容等于会话劫持）
2. **Skill 安全扫描**：自创和 Hub 安装的 Skill 走同一套扫描，不通过就回滚

## 自我修复机制

Hermes 的错误恢复采用分类器模式而非大 try/except：^[raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md]

- **错误分类器**：14 种 `FailoverReason` 枚举，每种映射到 4 个恢复标记（retryable / should_compress / should_rotate_credential / should_fallback），主循环只看标记做 dispatch
- **上下文压缩**：裁旧工具输出 → 保护头部（系统提示词 + 前 3 条消息）→ 保护尾部（阈值 20% token 预算）→ 中间摘要（SUMMARY_PREFIX 暗示"这是另一个助手留下的笔记"防重复执行）
- **凭证轮换**：429（临时限流）与 402（额度耗尽）处理完全不同——前者退避重试同一 Key，后者立即切换下一个 Key
