---
title: Harness Engineering 落地案例与演化
created: 2026-04-26
updated: 2026-05-08
type: concept
tags: [agent-framework, inference, data]
sources:
  - raw/articles/2026-04-22_从玩具到生产力：用真实项目讲透 AI Agent 的 Harness Engineering.md
  - raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md
  - raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md
  - raw/articles/2026-04-29_HarnessEngineering实践心得：如何高效驾驭AI？.md
  - raw/articles/2026-05-08-harness-engineering-90pct-ai-coding.md
  - raw/articles/2026-05-08-medical-ai-harness-era.md
---

# Harness Engineering 落地案例与演化

> 本文是 [[prompt-context-harness-engineering]] 三维框架的案例补充，聚焦企业级实践与演化脉络。

## 企业级落地案例

### 阿里云 Aegis 项目

Aegis 是一个内部 AI Agent 工程实践项目，其演进过程验证了 Harness 的必要性：^[raw/articles/2026-04-22_从玩具到生产力：用真实项目讲透 AI Agent 的 Harness Engineering.md]

- **起步阶段**：先收敛目标（让 Agent 读架构文档并复述需求），不急着编码
- **连续开发**：用 Spec 和 Handoff 对抗上下文腐烂，外部持久化记忆防止认知漂移
- **执行阶段**：将 Prompt 溶解进 Capability 框架（专属 Prompt + Python 脚本 + Validator），不再用巨型提示词穷举分支
- **运行阶段**：跨越"能聊"与"能跑"的分水岭，处理 504 超时、403 拦截等真实环境问题
- **交付阶段**：测试与回归前置化，不再是收尾动作而是工作轨道

核心理念：不是把大需求整包丢给模型等它"神奇做完"，而是持续对齐阶段目标，要求复述目标、检查偏差、提交中间产物。

### 腾讯 CDN LEGO 项目（百万行级实战）

[[tencent-lego]] 是 [[prompt-context-harness-engineering]] 在企业级超大规模系统中最完整的落地案例。面对 100 万行核心代码、300 万行深度改造第三方库、13,824 × N 种维度组合路径的高风险后端系统，LEGO 团队构建了五层 Harness 架构：^[raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md]

**上下文层**：四层递进体系（Agent.md 项目宪法 → 安全纪律反例免疫 → 领域知识可复用模式库 → 专业 Skill），将 38,068 行 RFC 原文固化本地，建立竞品调研 Agent 团队和协议安全测试 Agent 团队实现并行自动化调研。

**约束层**：三层约束架构（权限安全基座 → 代码规则即编译器 → 流程约束），核心原则是"用结构化约束替代语言化期望"——明确的约束比模糊的期望更有效。

**反馈层**：三条并行反馈通道（自动采集 Hook + 踩坑日志 Pitfall Journal + CLAUDE.md 内联反馈），实现"踩坑 → 规则 → Skill"的进化闭环，并通过 A/B 实验验证规则有效性。

综合效率提升约 20%，知识资产累计 31 个 Skill、34 条踩坑规则。

### 腾讯 LEGO 项目（deer-flow）

登顶 GitHub Trending 的 deer-flow 明确自称 "Super Agent Harness"：模型"大脑"与执行环境物理隔离，提供独立 Docker/K8s 沙盒；能力边界沉淀为按需加载的 SKILL.md；底层用 LangGraph 状态机编排子智能体。^[raw/articles/2026-04-22_从玩具到生产力：用真实项目讲透 AI Agent 的 Harness Engineering.md]

### JK Launcher：个人开发者的 Harness 演化实证

[[bai-jiajie]] 的 JK Launcher 项目是 Harness Engineering 在**个人独立开发者**场景下的完整落地案例，展现了从 Prompt 到 Context 再到 Harness 的真实演化路径。^[raw/articles/2026-04-29_HarnessEngineering实践心得：如何高效驾驭AI？.md]

JK Launcher 是一个 Unity 项目管理工具（WPF .NET Framework 4.8），管理 SVN 更新、启动 Unity 引擎、替换 Library 缓存、处理冲突、跑定时任务等游戏开发日常操作。从 V3.0 到 V3.11，经历了 12 个大版本迭代。

**Prompt 时代（V3.0~V3.3）**：一问一答式编码，每次对话从零开始。痛点：每次需重述项目背景、代码风格不一致、版本一多就失控。

**Context 时代（V3.4~V3.8）**：编写详细设计文档（~1000 行的 V3.5_DESIGN_SPEC.md），引导 AI 按文档实现。痛点：文档管得了"做什么"管不了"怎么做"，知识散落在聊天记录/文档/人脑中。

**Harness 时代（V3.9~至今）**：从"把 Prompt 写得更好"转向"搭建 AI 能自己转起来的环境"，构建了完整 Harness 体系：

| 组件 | 说明 |
|------|------|
| 声明式规则（.cursor/rules/）| 编码约束固化为规则文件，AI 每次启动加载 |
| 技能封装（.cursor/skills/）| 编译/测试等重复操作封装为一键可调 Skill |
| 自动化验证（verify_all.ps1）| 14 项自动检查，改完代码跑验证 |
| 多 Agent 协作（.cursor/agents/）| 7 个角色各管各的，文档化交接 |
| 基线管理（verification_baseline.json）| 量化指标只升不降 |

#### 七 Agent 协作体系

| Agent | 模型 | 职责 |
|-------|------|------|
| PM Orchestrator | composer-2 | 流程总控，状态机驱动 |
| Requirement Analyst | composer-2 | 需求分析、多义性拆解 |
| Solution Architect | composer-2 | 模块划分、接口定义 |
| Gate Reviewer | composer-2 | 8 维度前置审查 |
| Developer Agent | claude-4.6-opus-high-thinking | 代码实现（唯一写代码的角色） |
| Code Reviewer | gpt-5.4-medium | 代码评审、需求对照 |
| QA Tester | composer-2 | 测试用例与验证 |

核心原则：**发现问题的 Agent 不能自己修**，回退必须通过 PM Orchestrator 路由。

#### Rules/Skills/Scripts 三层分离

从"所有约束都放 Rules"（14 条 always-applied，1010 行）到三层分离：
- **流程 Rule**（~90 行）：告诉 AI"改完代码必须跑验证"——注意力衰减下最不容易忘
- **Skill**（SKILL.md + .bat）：封装操作步骤（编译/测试/验证）
- **Script**（verify_all.ps1）：14 项机械化检查，正则/模式匹配

always-applied 规则从 14 条降到 4 条，总行数从 1010 行砍到 275 行。核心思路：**能用机器查的就别靠 AI 记**。

#### 问题五分类与 Harness 改进动作

| 层次 | 问题性质 | 改进手段 | 频率 |
|------|----------|----------|------|
| 编码细节 | 写错了 | Rule + Script | 高频 |
| 操作步骤 | 忘了做 | Skill | 中频 |
| 角色交互 | 流程乱了 | SubAgent 定义 | 低频 |
| 外部能力 | 碰不到 | MCP 集成 | 按需 |
| 系统架构 | 整个环节缺了 | 新组件 | 极低频 |

#### 人的角色变化

| 以前 | 现在 |
|------|------|
| 写代码 | 设计 Agent 的职责边界和输出规范 |
| Review 代码 | 构建自动化的检查项 |
| 跑测试 | 定义测试基线和守护策略 |
| 写文档 | 编写规则文件和 Skill 文档 |
| 管进度 | 设计状态机和回退规则 |
| Debug | 分析验证报告，判断规则缺了还是代码写岔了 |

#### 关键认知

- **"AI 编程的瓶颈从来不在 AI 有多聪明，而在我有没有给它搭好发挥的舞台"**
- **好马配好鞍**：规则是缰绳，验证是刹车，Agent 体系是分工
- **Harness = 知识压缩**：十年经验拆成 Rule/Skill/Script，AI 秒级吸收
- **"Human taste is captured once, then enforced continuously on every line of code"**

该案例的核心价值在于：它是**从纯客户端开发者视角**、**单人实践**、**全流程记录**的 Harness Engineering 实证，与腾讯 LEGO（企业级百万行团队实践）形成互补。^[raw/articles/2026-04-29_HarnessEngineering实践心得：如何高效驾驭AI？.md]

### 工程师角色迁移

Harness 的深层意义在于推动工程师角色从"亲手写出每一行代码"迁移到"定义目标、卡住边界、掌控节奏、验收结果"。Harness 不只是"把模型管住"，也在逼着人类工程师升级：从执行者变成控盘者。^[raw/articles/2026-04-22_从玩具到生产力：用真实项目讲透 AI Agent 的 Harness Engineering.md]

## 四阶段演化框架

从实践者视角，AI 应用的工程范式经历了四个递进演化阶段，每个阶段解决前一个阶段的核心问题：^[raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md]

1. **Prompt Engineering（来时的路）**：关注"怎么写好指令"——措辞、few-shot、思维链引导。在单轮对话中有效，但在 Agent 长时运行中，静态 prompt 无法 hold 住动态膨胀的信息量
2. **Context Engineering（管好 Agent 看到的一切）**：关注"Agent 每次推理时整个信息环境长什么样"——系统指令、工具定义、外部数据、对话历史、memory 等的总和。核心目标：让 Agent 在每一步推理时看到最精准、最少冗余的信息
3. **Spec-driven Development（先写契约，再让 Agent 动手）**：解决"Agent 怎么知道你到底想要什么"的问题——先写 spec（清晰的契约：要什么、不要什么、约束、验收标准），再让 Agent 基于规范实现。参见 [[spec-driven-development]]
4. **Harness Engineering（给 Agent 搭一个约束体系）**：解决 Agent 长期维护项目时熵增的问题——命名漂移、重复函数、架构约束被突破。通过机械化规则代替口头约定，构建"犯错→修复→沉淀"的飞轮

这一框架与 Context Engineering 概念的出圈密切相关——Shopify CEO Tobi Lütke 在 X 上的帖子让 Context Engineering 广受关注，Anthropic 也专门发文阐述。

## OpenAI 的 Harness Engineering 实践

OpenAI 在一篇文章中分享了内部实践：3 个工程师、5 个月、完全零手写代码，构建了约百万行代码的内部产品。团队的核心工作不是写代码，而是搭建 Harness。^[raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md]

### 四层架构

| 层次 | 核心思路 | 具体做法 |
|------|----------|----------|
| **约束层** | 机械化规则代替口头约定 | 强制分层架构，跨层依赖通过自定义 linter 和结构化测试来机械化检验——"如果一条架构规则值得写进文档，那它就值得用 linter 来强制执行" |
| **文档层** | AGENTS.md 当目录而非百科全书 | 从塞满所有规则（效果很差）精简到约 100 行，只作为索引指向仓库中结构化的 docs/ 目录——"给 Agent 一张地图，而不是一本千页手册" |
| **反馈层** | 构建"犯错→修复→沉淀"的飞轮 | Agent 卡住时不选择"再试一次"或人工介入，而是诊断"缺了什么能力"，让 Agent 自己把缺失的能力构建回仓库。每一次失败转化为基础设施改进 |
| **清理层** | 自动化对抗熵增 | 将"黄金原则"编码为仓库级规则，用后台 Agent 定期扫描偏差、自动提交清理 PR——把技术债变成每天的小额偿还 |

Octopus Deploy 的比喻很贴切：**Agent 是马，Harness 是缰绳**。马本身快速有力，但没有缰绳就只会横冲直撞。^[raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md]

### 阿里 price-center 项目（90% AI 代码率）

作者新安在阿里内部一个企业级 Java 应用（10 万+行代码，技术栈：Java 1.8 / Spring Boot / LiteFlow / HSF / Diamond / Tair）中，从零构建 Harness 体系，将 AI 代码率从 24.86% 提升至 90.54%。^[raw/articles/2026-05-08-harness-engineering-90pct-ai-coding.md]

**四要素架构**：规则体系（Rules）、技能体系（Skills）、知识库（Wiki）、变更管理（Changes），以 `.harness/` 目录作为物理载体。

**Application Owner Agent**：约 400 行的 Agent 定义文件，承担"Index & Map"职责，包含五大模块——角色与项目背景、配置中枢索引、七项核心职责、十阶段工作流程调度、沟通原则与硬性约束。

**上下文分层加载**：L1 会话常驻层（Agent 定义 + Rules，< 40% 填充率）、L2 阶段触发层（按阶段加载 Skill）、L3 按需查询层（Wiki 业务文档）。

**十阶段流水线**：需求分析 → 需求评审 → 编码实现 → 编码评审 → 单元测试编写 → 单元测试评审 → 代码推送 → CI 验证 → 部署验证 → 用户确认。5 个 Human-in-the-Loop 确认点，回退路径精确路由，评审循环上限（需求 3 轮、编码/测试 2 轮）。

**9 个 Skill 模块**：`request-analysis`、`coding-skill`（含 8 份分层编码 Spec）、`expert-reviewer`、`unit-test-write`、`unit-test-ci`、`deploy-verify`、`code-review`、`project-analysis`、`aone-ci-generate`。

**关键经验**：
- Harness 本身需要 Dry Run——用虚拟需求完整走一遍全流程来暴露缺陷
- 质量门禁必须可程序化验证——"一切不可被机器验证的约束，在 Agent 执行中都是无效约束"
- 分离执行与评判——评审 Agent 发现了编码 Agent 遗漏的渠道判断逻辑（潜在线上故障）
- 规范是活文档——"规范的每一行都对应一个历史失败案例"

**效果数据**（2026年3月 vs 4月）：

| 维度 | 无 Harness（3月） | 有 Harness（4月） |
|------|------------------|------------------|
| 项目 AI 代码率 | 24.86% | **90.54%** |
| 个人 AI 代码率 | 14.24% | **87.85%** |

### 医疗 AI 的 Harness 落地：WiseClaw 2.0

[[wiseclaw]] 是 [[zhizhen-tech]]（智诊科技）发布的面向医疗健康行业的 Agent OS 平台，底层延续 [[openclaw]] 在连接与调度上的能力，上层将 Harness 核心理念做成系统默认值。^[raw/articles/2026-05-08-medical-ai-harness-era.md]

医疗 AI 的核心门槛落在四个维度：**长时程**（服务以月/年为单位）、**可追溯**（建议来源、工具调用、知识版本可查）、**可执行**（接设备、接系统、接流程）、**可治理**（权限、脱敏、评测、审批、审计）。

WiseClaw 的 Harness 实现包括：
- **三层流水线**：Triage 分诊识别 → Clinical 临床执行 → Evaluator 校验拦截，关键节点可插入人工复核
- **健康档案驱动**：长期记忆基础，确定性读写客观信息，结构化沉淀服务信息
- **心跳引擎**：从会话驱动升级为时间/事件/数据共同驱动，主动提醒、持续跟进
- **全链路可观测**：对话、工具调用、知识引用、版本信息、流程节点结构化记录，形成完整 Trace

该案例标志着 Harness Engineering 从代码生成场景扩展到医疗健康等高风险行业。

> 返回 [[prompt-context-harness-engineering]]
