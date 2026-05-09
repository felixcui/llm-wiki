---
title: Self-Improving Agent
created: 2026-04-24
updated: 2026-04-30
type: concept
tags: [agent-framework, optimization, inference]
sources:
  - raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md
  - raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md
  - raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md
  - raw/articles/2026-04-30_开源「洁癖.skill」，让你的Agent越用越聪明。.md
---

# Self-Improving Agent（自进化 Agent）

Agent 通过内部闭环机制自动积累经验、沉淀能力，随使用时间增长而持续变强的设计模式。[[hermes-agent]] 是该模式的代表实现，通过 Memory、Skill 与 Nudge Engine 三子系统构成自我改进闭环。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## 三子系统闭环

Self-Improving Agent 的核心架构由三个子系统协作完成：

- **Memory**：记住用户偏好和环境事实（"我知道什么"）
- **Skill**：沉淀可复用的操作步骤（"我会做什么"）
- **Nudge Engine**：定时提醒 Agent 停下来反思，触发学习和沉淀

三者形成"干活→踩坑→反思→积累→下次少踩坑"的正向循环。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## Memory 系统

Memory 维护两个纯文本文件：`MEMORY.md`（环境事实、项目约定）和 `USER.md`（用户偏好、沟通风格），字符上限故意设紧（MEMORY 2200 chars、USER 1375 chars），迫使 Agent 挑重要信息记录，低质量内容自然被挤出。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

### 冻结快照保护前缀缓存

每次会话启动时，Memory 加载后立刻捕获快照，系统提示词中使用这份不可变的快照。新写入的内容只改磁盘，下一会话才刷新。这样系统提示词在会话内保持不变，可共享前缀缓存（Prefix Cache），省掉重复计费。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

### 声明式事实 vs 操作步骤分离

Memory 要求写成声明式事实（"User prefers concise responses" ✓），而非命令式指令（"Always respond concisely" ✗）。前者是偏好，可被当前上下文覆盖；后者是死命令，会限制 Agent 灵活性。操作步骤归 Skill 管，不存 Memory——一句话画清了两个系统的分工。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

### 设备侧记忆基础设施：OpenChronicle

[[openchronicle]] 提供了另一种 Memory 实现路径——将记忆从项目级文件提升为操作系统级基础设施。与 Hermes 的 `MEMORY.md`/`USER.md` 不同，OpenChronicle 通过 AX Tree（macOS Accessibility API）自动捕获跨应用的屏幕操作，按 7 个分类（人、项目、工具、话题、联系人、组织、事件）存为本地 Markdown，通过 MCP 协议暴露给任意 Agent 使用。^[raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md]

两种方案互补：Hermes 的 Memory 侧重项目内偏好与约定的紧凑存储，OpenChronicle 侧重跨应用、跨 Agent 的全局上下文连续性。两者共同指向 Self-Improving Agent 的核心需求——从"记住你说过什么"（认知层）到"记住你怎么做事"（行动层）。

## Skill 自动创建与自我修补

### 创建阈值

工具调用超过 5 次的复杂任务、踩过坑后修复的经验、用户纠正过的做法——这些都值得沉淀为 Skill。简单一次性任务不记，避免 Skill 库膨胀。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

### Fuzzy Patch + 安全扫描回滚

Skill 修补不是全量重写，而是用 fuzzy_find_and_replace 做模糊匹配局部修改（Agent 给出的 old_string 可能跟原文有格式差异）。每次修改后自动执行 `_security_scan_skill()`，不通过就回滚到原始内容。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

### 渐进式加载

Skill 索引（名字 + 一句话描述）默认注入系统提示词，Agent 判断相关时才通过 `skill_view` 加载完整内容。按需加载避免 Token 浪费和注意力稀释。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## Nudge Engine 与后台 Fork 审查

Nudge Engine 维护两个计数器（Memory 每 10 个用户回合、Skill 每 10 次迭代），到阈值后触发审查。触发后在后台 fork 一个独立 Agent 实例，拿着主对话快照异步执行记忆审查、技能审查和综合反思。输出重定向到 `/dev/null`，用户完全无感知，最多 8 次工具调用，review agent 自身的 nudge 被禁用避免无限递归。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## RL 训练闭环

除动态 Skill 沉淀（"外挂式进化"）外，Hermes 还支持 RL 训练（"内化式进化"）：基于 Agent 运行轨迹自动合成训练数据，经过质量筛选后在隔离环境中做小规模实验→正式训练→自动评估的完整闭环，最终通过改变模型权重实现领域内的局部最优解。^[raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md]

## 组织级进化：RDSHermes Skill Hub

开源版的 Skill 存储在本地 `~/.hermes/` 目录下，经验积累是单点的。RDSHermes 将 Skill 存储搬到云端 Skill Hub，预装智能巡检、慢 SQL 诊断、索引优化等专业技能。一个 DBA 踩过的坑，团队所有人的 Agent 都能绕过——Skill Hub 解决冷启动，自进化解决越用越强。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## 安全机制

- Memory 内容扫描：检测 prompt injection、凭证泄露等威胁模式
- Skill 安全扫描：自创和 Hub 安装的 Skill 走同一套扫描，不通过就回滚
- RDSHermes 加密托管：AK/SK 由网关代理鉴权，密钥不落盘^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## 与其他框架的对比

### OpenClaw vs Hermes 的设计哲学分野

[[openclaw]] 的 Skill 是手写的 Markdown 文件——写多少会多少，不写就不会。[[hermes-agent]] 做了一件 OpenClaw 架构上做不了的事：Agent 干完活后自动把踩坑经验提炼成可复用的 Skill，下次遇到同类问题直接调用。这不是功能差异，是设计哲学的分野——一个靠人喂，一个自己长。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

OpenClaw 的 Skill 是手写的配置文件，用了一年还是那份手写的配置文件；Hermes 的 Skill 是越用越厚的经验资产——每一次踩坑都在加固护城河。HN 上有个帖子叫 "Data Is the Final Moat"——当模型智能被商品化、Agent 框架被开源，真正的护城河是 Agent 在工作中积累的领域知识。

| 特性 | [[hermes-agent]] | [[openclaw]] | [[claude-code]] |
|------|-----------------|-------------|----------------|
| Skill 来源 | 自动生成 + 手动 | 手写 / 社区安装 | CLAUDE.md / Agent Skills |
| 经验沉淀 | 运行轨迹自动提炼 | 无自动机制 | 无自动机制 |
| Memory 管理 | 容量受限 + 主动淘汰 | 纯追加，易膨胀 | CLAUDE.md 手动维护 |
| 后台审查 | Fork Agent 异步审查 | 无 | 无 |

## 进化效果量化

以 K8s 部署场景为例，三次会话的对比：

| 维度 | 第 1 次（冷启动） | 第 2 次（Skill 复用） | 第 3 次（全协同） |
|------|-------------------|----------------------|-------------------|
| 工具调用 | 12 次 | 9 次 | 6 次 |
| 错误数 | 2 | 1 | 0 |
| Memory | 无 | 触发写入 | 系统提示词注入 |
| Skill | 触发创建 | 复用 + 自我修补 | 复用已修补版本 |

^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## 经验外部化路线对比

当前主流 Agent 框架将运行经验外化为持久资产有三条不同路线：^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

| 维度 | Claude Skills | Codex | Hermes Skills |
|------|--------------|-------|---------------|
| **经验载体** | CLAUDE.md / Agent Skills | 仓库中的 CLAUDE.md + 工作流 | 动态 SKILL.md 文件 |
| **经验来源** | 团队经验 → 工作单元 | 工程经验 → 仓库流程 | 运行时经验 → 过程资产 |
| **维护方式** | 人工编写 + 社区共享 | 人工编写 + 版本控制 | Agent 自动创建 + 自我修补 |
| **粒度** | 项目级 / 团队级 | 仓库级 / 项目级 | 任务级 / 模式级 |
| **自动沉淀** | 无 | 无 | 有（Nudge Engine 驱动） |

三条路线各有取舍：Claude Skills 和 Codex 依赖人工提炼，质量可控但无法自动积累；Hermes 能自动沉淀但需要安全机制防止错误经验固化。^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

## 自动沉淀的风险与防护

Agent 自动沉淀经验并非没有代价——**错误经验也会被自动沉淀**。如果 Agent 在某次任务中走了一条错误路径并据此创建了 Skill，后续同类任务都会加载这个错误 Skill，导致系统性偏差。^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

### Skill 进入生产系统的五道门

为防止错误经验扩散，Hermes 设计了五道门禁：^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

1. **结构门**：Skill 必须符合 SKILL.md 规范格式（目标、适用场景、执行规范等）
2. **来源门**：四级信任策略——built-in（最高信任）> trusted（团队审核）> community（社区贡献）> agent-created（最低信任，需额外审查）
3. **评估门**：后台审查 Agent 交叉验证 Skill 内容合理性
4. **版本/回滚门**：每次修改前自动备份，安全扫描不通过即回滚
5. **权限门**：不同信任级别的 Skill 享有不同的执行权限范围

## hermes-agent-self-evolution 仓库

Nous Research 维护了专门的 [hermes-agent-self-evolution](https://github.com/NousResearch/hermes-agent-self-evolution) 仓库，用于系统性优化 Skill 质量：^[raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md]

- **技术方案**：使用 **DSPy**（Declarative Self-improving Python）框架 + **GEPA**（Generative Evolutionary Prompt Adaptation）算法自动优化 Skill 内容
- **质量保障**：所有 Skill 变更必须走 **PR Review** 流程，不接受自动合并
- **核心理念**：Skill 优化可以是自动化的，但上线必须是人工审核的——自动化负责探索，人工负责把关

## 自我进化的下一步方向

Hermes 的"自动创建"和"自我修补"已经跑通，接下来几个值得探索的方向：^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

1. **生命周期管理**：在 YAML frontmatter 中加入 `last_used`、`use_count`、`success_rate`，实现自动降权、归档和过时检测
2. **技能组合**：自动识别经常一起用的 Skill 合成工作流（如 flask-k8s-deploy + nginx-reverse-proxy → full-stack-deploy），从"记住"进化到"思考"
3. **创建透明度**：Skill 创建后给用户简短通知，让用户能审核和纠正
4. **团队治理**：写操作需二次确认，每次会话可追溯、可审计——[[rdshermes]] 已在这一方向上落地实践

## 上下文保洁模式：洁癖.Skill

Agent 越用越笨的核心原因之一，是项目文档和记忆文件的长期疏于维护——上下文腐败（[[context-rot]]）不仅来自单次会话内的 Token 膨胀，更来自跨会话的文档体系混乱。数字生命卡兹克开源的「洁癖.Skill」（Neat.Skill）正是为此设计的上下文保洁方案。^[raw/articles/2026-04-30_开源「洁癖.skill」，让你的Agent越用越聪明。.md]

### 项目知识三层模型

该 Skill 基于一个核心洞察：一个项目中的知识分三层，每层职责不同：

1. **Agent 记忆系统**：过去的聊天记录、项目隐性知识。AutoDream 等工具仅维护这一层。
2. **CLAUDE.md**：项目根目录下的约束文件，给 AI 自己看——项目约定、结构、红线清单等。
3. **docs/ 目录 + README**：给他人（Agent、同事、下游开发者）看的文档——接入指南、架构说明、运维手册。

AutoDream 只管第一层（记忆文件），不管第二、三层（CLAUDE.md 和 docs/），这是它在实践中效果有限的原因。洁癖.Skill 的设计目标是对三层同时保洁。

### 核心原则：合并优于追加，删除优于保留

这与大多数人的直觉相反——信息多不是优势，信息准才是。一条过期的记忆比没有记忆更糟糕，因为 Agent 读到过期信息时，会基于错误的前提做事；没有信息时，它至少知道自己不知道，会主动询问。

### 五步保洁流程

每次运行 `/neat` 或「审查一下」，Skill 按顺序执行五步：

1. **强制机械式盘点**：列出项目所有 `.md` 文件，逐个读取，不遗漏最关键但容易被忽略的文档。
2. **变更影响矩阵识别**：不只看对话中的新事实，还分析新事实波及的文档层级。关键检查：本次变更是否跨项目影响？
3. **按序修改**：先改 `docs/` → 再改 `CLAUDE.md` → 最后整理记忆。修改顺序本身是一种优先级策略——公共文档先行，私有记忆最后。
4. **自检清单**：如「新增的环境变量在 runbook 和 CLAUDE.md 都出现了吗？」「是否有相对时间遗留？」等。
5. **变更摘要输出**：向用户报告具体改了什么，与 [[hermes-agent]] 的 Skill 变更通知机制形成互补——一个自动触发，一个用户驱动。

### 存档式使用模式

洁癖.Skill 被类比为游戏的「存档机制」：每次任务收尾时运行 `/neat`，将当前知识状态持久化到文档中，下次新会话可直接继续，无需重复交代上下文。这让 Agent 的知识体系从「依赖对话上下文」转变为「依赖持久化文档」——对话可以随时关闭，但文档永远在。

这种模式与 [[self-improving-agent]] 的 Nudge Engine 后台审查形成对比：Nudge Engine 是定时自动触发，洁癖.Skill 是任务完成时用户主动触发。两者目标一致——防止 Agent 越用越笨——但触发时机和覆盖范围不同，可互相补充。
