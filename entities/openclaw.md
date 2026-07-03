---
title: OpenClaw
created: 2026-04-25
updated: 2026-05-16
type: entity
tags: [tool, agent-framework, open-source, company]
sources:
  - raw/articles/2026-04-25_ai_pm_catwu.md
  - raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md
  - raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md
  - raw/articles/2026-04-25_一句话精准P视频，AI视频的Photoshop「Buzzy」来了！.md
  - raw/articles/2026-04-25_开源一个PPTSkill｜压进了我10年的设计经验.md
  - raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md
  - raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md
  - raw/articles/2026-04-27-anthropic-launch-secrets.md
  - raw/articles/2026-05-14_【深度拆解】OpenClawvsHermes：多Agent架构设计.md
  - raw/articles/2026-05-15_AI让生产效率不再是瓶颈，然后呢？｜AI跃迁者调研02-flomo少楠.md
  - raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md
  - raw/articles/2026-05-08-medical-ai-harness-era.md
  - raw/articles/2026-05-14_SkillsPluginsMCPAgent到底是什么.md
---

# OpenClaw

**类型**: 开源 Agent 框架（"龙虾"🦞）
**技术栈**: TypeScript / Node.js
**模型依赖**: Anthropic Claude API

## 概述

OpenClaw（昵称"龙虾"）是一个基于 Anthropic Claude API 的开源 Agent 框架，使用 TypeScript 编写，运行在单进程 Node.js 环境中。它直接基于 Anthropic SDK 构建，不依赖 LangChain 等中间件框架，遵循"薄抽象、显式控制流、贴近 SDK"的设计理念，追求工程上的确定性、可调试性和可扩展性。^[raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md]

OpenClaw 是 [[claude-code]] 之外最受欢迎的 Agent 框架之一，也是 [[hermes-agent]] 的主要对标和兼容对象。

## 历史与社区动态

### 创始人与爆火

OpenClaw（原名 Clawdbot）由奥地利开发者 **Peter Steinberger** 创立。2026年初爆火，GitHub 获得 24.7 万星。2026年2月，Peter Steinberger 加入 OpenAI，OpenClaw 移交开源基金会。^[raw/articles/2026-04-25_ai_pm_catwu.md]

### Anthropic 封堵事件

2026年4月4日，Anthropic 封堵了 OpenClaw 对 Claude API 的访问，通知时间不到 24 小时。Anthropic 产品负责人 [[cat-wu]] 在访谈中讨论了这一决定——Anthropic 自己的 Cowork 产品功能与 OpenClaw 存在重叠。^[raw/articles/2026-04-25_ai_pm_catwu.md]

在第二次访谈中，[[cat-wu]] 进一步解释了封堵原因：Claude 的需求增长太快，订阅额度是为第一方产品设计的。第三方工具的使用模式不同，给基础设施带来了额外压力。Anthropic 给用户提供了 credits 作为缓冲，但需要优先保障第一方产品和 API。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

## 架构设计

### 核心模块

OpenClaw 的架构由四个核心模块组成：

1. **Tool Layer（工具层）**
2. **MessageBus（消息总线）**
3. **SubagentManager（子 Agent 管理）**
4. **REPL 主循环**

### Tool Registry

工具注册系统是 OpenClaw 的基础设施。每个 Tool 由四个要素定义：

- `name`：工具名称
- `description`：功能描述
- `input_schema`：直接取自 `@anthropic-ai/sdk` 的类型定义
- `execute`：执行函数

ToolRegistry 本身是一个 `Map<string, Tool>`，提供注册、执行、获取定义和排除工具的能力。schema 使用运行时普通对象定义（非 Zod 等库），零额外依赖，直接对齐 SDK 类型。^[raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md]

**内置工具一览**：
- **文件操作**：ReadFile、WriteFile（自动创建父目录）、EditFile（强制唯一匹配，出现0次报错、超过1次拒绝写入）、ListDir
- **命令执行**：ExecTool，三层防护——危险命令正则黑名单（rm -rf /、fork bomb 等）、30秒超时 + 2MB maxBuffer、超过10,000字符的智能截断（保留首尾各5,000字符）
- **Web 能力**：WebSearchTool（封装 Brave Search API）、WebFetchTool（内置纯正则 HTML 转 Markdown）

### MessageBus

消息总线负责 Agent 内部及 Agent 间的消息分发。采用显式控制流设计，系统边界清晰，运行路径容易追踪。

### SubagentManager

子 Agent 管理器支持在主 Agent 内创建和管理子 Agent 实例。关键设计：

- 通过 `ToolRegistry.exclude()` 为子 Agent 生成受限工具集，排除 `spawn`（避免递归创建子 Agent）和 `message`（避免直接向用户发消息）等工具
- `exclude()` 返回新的 ToolRegistry 实例，不修改原注册表
- 子 Agent 持有主 Agent 工具集的安全子集

## 上下文管理

OpenClaw 采用绝对阈值触发机制进行上下文压缩：当上下文 Token 数达到固定边界（如总窗口 20K 中的 18K）时自动触发压缩。压缩策略为"头尾保留、中间摘要"——保护头部系统指令和尾部近期对话，对中间工具调用过程进行摘要压缩。^[raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md]

## 与 Claude Code 的对比

| 维度 | Claude Code | OpenClaw |
|------|-------------|----------|
| 开源 | 否（商业产品） | 是 |
| 技术栈 | 闭源 | TypeScript / Node.js |
| 模型 | 仅 Claude | 仅 Claude（基于 Anthropic SDK） |
| 扩展性 | 官方维护 | 社区驱动，可自由修改 |
| 部署 | 云端 | 可本地部署 |
| 多平台接入 | CLI | 支持多种 IM 平台（WhatsApp、Slack 等） |

## 与 JiuwenClaw / Team Skills 的关系

OpenClaw 生态与华为支持的 openJiuwen 社区的 JiuwenClaw 存在密切关联。JiuwenClaw 提出的 Team Skills 标准扩展了 Agent Skills 开放标准，可以在 Claude Code、Cursor 等支持多智能体协同的平台上零适配运行。^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]

## Harness 工程取向：先把 Agent 管住

在 [[prompt-context-harness-engineering]] 三维框架中，OpenClaw 代表"受控运行时"取向。其 Harness 工程的核心特点包括：^[raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md]

### MCP/工具链：能力规范与安全边界

- Skills 是"方法稳定器"，让模型不要过于发散；MCP/工具链是"能力本身"
- 把 Tools、Plugins、Gateway、外部能力接入放进有边界感的系统里
- 目标：让模型在一个被约束的能力平面里稳定地调工具
- 当外部 API 失败时，作为运行时事件处理，而非让模型混淆"自己理解错了"还是"上游接口挂了"

### Skills 受控加载

- 第三方 Skills 被视为不可信，plugin skills 只是低优先级路径
- 同名 skill 会被 bundled/managed/agent/workspace skill 覆盖
- workspace 和 extra-dir 的 skill discovery 只接受解析后 realpath 仍在配置根目录内的 skill，避免路径穿越

### Runtime：任务持续推进

OpenClaw 的 Runtime 承担将 Agent 行为从零散动作组织成能推进任务的流程，包括可观测性和中断重试逻辑。

### 记忆系统

OpenClaw 对记忆的态度很克制，本质上是"可替换能力位"——实现最基础的，用户可按需替换。对比 [[hermes-agent]] 的完整记忆体系，OpenClaw 的 MEMORY.md 采用纯追加模式，几个月后容易膨胀成几万行。

### 工程目标

OpenClaw 的工程方向很明确：怎么让 Agent **安全、稳定、受控**地执行任务？志在成为企业 Agent 标配。^[raw/articles/2026-04-24_Harness到底是什么？看看OpenClaw、Hermes、ClaudeCode的演绎吧.md]

## 深度访谈观点（2026-04-29）

参见深度访谈：^[raw/articles/2026-04-29_深度访谈｜OpenClaw引爆Agent元年，AIAgent在企业内如何规模化应用？.md]

### 爆火驱动力

由阿里云专有云 Agent 负责人 [[feng-chengcheng]] 和通义科学家 [[yao-liuyi]] 分析：
- **本质**：AI 交互范式从"对话式 AI"到"执行式 AI"的质变
- **三要素**：模型能力到了、产品体验顺了、开源传播快了
- **产品革新**：接入 IM 渠道、心跳机制、定时任务机制

### 从"试试"到"用起来"的三道墙

冯成成提出用户从尝试到真正使用的三大障碍：
1. **场景墙**：用户不知让 Agent 干什么，需从"问问题"转向"交任务"
2. **成本墙**：Token 消耗的经济压力，需选高价值场景
3. **信任墙**：担心误操作和隐私泄露，最难跨越

### 与 HermesAgent 的架构对比

- OpenClaw：**"人教 Agent 做事"** — 简单、可控、易审计，通过 Skills 扩展能力，但缺乏自我进化
- [[hermes-agent]]：**"Agent 自己学会做事"** — 完整记忆框架（短/中/长期 + 技能记忆体），Skill 可在执行中沉淀
- 适用场景：OpenClaw 适合确定性强、流程清晰的任务；HermesAgent 适合目标模糊、需探索优化的任务
- 长期趋势：两者应融合，同时具备稳定可控的执行底座和持续学习能力

## 应用场景

OpenClaw 适用于：
- 需要 Claude API 但不依赖 Claude Code 商业产品的开发者
- 需要自定义 Agent 行为和工具集的场景
- 多 Agent 协作任务的实验与开发
- 本地部署和隐私敏感环境
- 第三方 IM 平台集成

## 行业应用

### 医疗健康：WiseClaw 2.0

[[wiseclaw]] 是 [[zhizhen-tech]]（智诊科技）基于 OpenClaw 构建的医疗健康行业 Agent OS 平台，底层延续 OpenClaw 在连接与调度上的能力，上层将 [[prompt-context-harness-engineering]] 核心理念做成系统默认值。该平台已与 300+ 三甲医院、500+ 头部医疗企业合作，标志着 OpenClaw 生态从通用开发场景扩展到医疗等高风险行业。^[raw/articles/2026-05-08-medical-ai-harness-era.md]

## 多 Agent 架构：基于会话系统

OpenClaw 采用**基于会话系统**实现多 Agent 架构。核心问题是：父 Agent 如何把任务交给子 Agent？OpenClaw 的答案是**先创建一个子会话，再纳入现有的会话、运行网关、运行时和权限策略体系**。子 Agent 不是一个临时函数调用，而是一个带会话标识、生命周期状态和工具策略的子会话。^[raw/articles/2026-05-14_【深度拆解】OpenClawvsHermes：多Agent架构设计.md]

### 子任务执行链路

```
1. 父 Agent 发起子会话创建请求
2. 系统校验运行时、深度、并发数量、沙箱策略和目标 Agent
3. 生成新的子会话标识
4. 根据父子关系计算层级、角色和控制范围
5. 子会话的父子关系、角色、工作目录写入会话状态
6. 运行网关启动子会话
7. 子任务注册机制接管等待、结果捕获、事件回传和清理
```

### 关键设计特点

- **统一入口创建子运行时** — 同时处理运行时选择、上下文隔离、沙箱策略、线程绑定、模型配置
- **角色信息写入会话元数据** — 层级、角色、控制范围、是否允许继续派生子 Agent
- **双层权限控制** — 提示词层面约束行为 + 运行时层面裁剪工具
- **事件链路结果回传** — 子 Agent 完成后通过事件投递回目标会话，区分内部链路和用户交付链路
- **默认单层并行展开** — 不放开深层递归委派，先保证一层并行可控

^[raw/articles/2026-05-14_【深度拆解】OpenClawvsHermes：多Agent架构设计.md]
