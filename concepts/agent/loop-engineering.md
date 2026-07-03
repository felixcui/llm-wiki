---
title: Loop Engineering（循环工程）
created: 2026-06-24
updated: 2026-06-24
type: concept
tags:
  - agent-framework
  - practice
  - coordination-engineering
sources:
  - raw/articles/2026-06-15_Codex和ClaudeCode负责人都不写提示词了，AI圈爆火的Loop到底是什么.md
  - raw/articles/2026-06-16_AIAgentLoop到底是什么？多数人一上来就用错了丨Greg.md
  - raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md
  - raw/articles/2026-06-19-HarnessEngineeringvs-LoopEngineering-md.md
  - raw/articles/2026-06-20_agent工程究竟有多少层loop-langchain团队的loopengineering实战经验.md
  - raw/articles/2026-06-21_TheAgentLoopArchitecture.md
  - raw/articles/2026-06-22_OpenClaw创始人吹的Loop工程到底是个撒？不用写提示词了？.md
  - raw/articles/2026-06-23_面向产品经理的循环工程（LoopEngineering）.md
  - raw/articles/2026-06-17_LoopEngineering，从Prompt工程师到Loop架构师的14步路线图.md
  - raw/articles/2026-06-17_读完AgentLoop工程手册，我有8个还没想明白的问题.md
confidence: high
---

# Loop Engineering（循环工程）

**Loop Engineering** 是 2026 年 6 月由 Google AI 总监 [[boris-cherny|Addy Osmani]] 正式命名的概念，指**设计一套能自动指挥 AI 的循环系统**——让 Agent 自己「行动→观察→修正→再行动」，而不需要人每一轮手动推动。Claude Code 负责人 Boris Cherny 和 OpenClaw 作者 Peter Steinberger 在此之前已各自实践这一方法。^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]

## 概念引爆

三个互不打招呼的一线人物几天内指向同一件事^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]：
1. **Peter Steinberger**（OpenClaw 作者）：在 X 发文「不要再去提示你的编程 Agent 了，你该去设计那些提示你 Agent 的循环」，浏览量 830 万
2. **Boris Cherny**（Claude Code 负责人）：播客访谈中说「不跟 Agent 对话，跟 loop 对话，让 loop 替我来 prompt」
3. **Addy Osmani**（Google Cloud AI 总监）：6 月 7 日写博客正式命名为 Loop Engineering

## 演进脉络：四层递进

Loop Engineering 是 Prompt → Context → Harness → **Loop** 演进链的第四层^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]：

| 阶段 | 管什么 | 类比 |
|------|--------|------|
| Prompt Engineering | 给模型的那句话怎么写 | 司机听谁的指令 |
| Context Engineering | 模型脑子里该装哪些信息 | 司机能看到什么路牌 |
| Harness Engineering | 单次 Agent 的整套装备 | 司机的车和仪表盘 |
| **Loop Engineering** | 按计划自动循环跑，自己 prompt 自己 | **自动驾驶** |

核心洞察：**这条线只有一个方向——你离亲手干活越来越远，离设计系统越来越近。**^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]

## 一个 Loop 的五个动作

花叔总结的最简洁框架^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]：

> **发现 → 交付 → 验证 → 记下来 → 决定下一步**

面向产品经理的视角同样适用五个部分：**触发器、动作、证据、记忆和停止条件**^[raw/articles/2026-06-23_面向产品经理的循环工程（LoopEngineering）.md]。

Addy Osmani 给出了六个构建组件^[raw/articles/2026-06-15_Codex和ClaudeCode负责人都不写提示词了，AI圈爆火的Loop到底是什么.md]：

| 组件 | 说明 | 工具对应 |
|------|------|----------|
| **定时任务** | 什么时候开始干活 | Codex Automations、OpenClaw HEARTBEAT、Hermes cron |
| **隔离工作区** | 给每个 Agent 独立分支 | Git Worktree |
| **技能** | 项目知识固化在文件里 | SKILL.md、AGENTS.md |
| **连接器** | 连接外部系统获取信息 | MCP 协议 |
| **双 Agent** | 写代码的和审代码分开 | Sub-agent（reviewer） |
| **状态文件** | 跨会话的记忆，活在对话之外 | Markdown 文件、看板、Linear |

## `/goal` 与 `/loop`：两种循环模式

花叔给出了最清晰的区分^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]：

- **`/goal`**（进度驱动）：朝着终点跑，到了就停。适合有明确终点的活——修 bug 修到测试全绿、做功能做到验收清单全勾
- **`/loop`**（时间驱动）：踩着节拍一圈圈转，你不喊停它不停。适合需要持续盯、轮询状态、周期重复的活

用反了最亏：拿 `/loop` 干有终点的活会空转烧 token，拿 `/goal` 盯左右不了的外部状态会永远转不出来。

## 核心原则：写代码的 AI 不能给自己的代码打分

这是花叔认为全文最重要的观点^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]：让一个 Agent 评价自己刚产出的东西，它几乎总是自信地夸。解法是把 Agent 劈成两个——一个负责生成，一个专门负责挑刺，后面这个要调教得多疑一点。

Claude Code 的 `/goal` 正是这一原则的落地：每一轮由另一个独立的快模型判断完成条件满足没，生成和评判分家^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]。

## Harness 是 Loop 的地基

AI技术立文强调^[raw/articles/2026-06-19-HarnessEngineeringvs-LoopEngineering-md.md]：**Loop Engineering 位于 Harness 的上一层**。Loop 不过是 Harness 加上定时器和自我反馈。同一模型搭配不同 Harness 会表现迥异，差的 Harness 只会让循环加速产出低质量结果。

14 步路线图的构建顺序^[raw/articles/2026-06-19-HarnessEngineeringvs-LoopEngineering-md.md]：
1. 干净 Harness 上让一次手动运行可靠
2. 加上下文和权限
3. 加审查子 Agent
4. 加记忆
5. **然后，只有在那之后，再套上循环**

> 循环得到了光环，harness 完成了工作。自改进从来不是模型的属性，而是你围绕模型构建的 harness 的属性。^[raw/articles/2026-06-19-HarnessEngineeringvs-LoopEngineering-md.md]

## Agent Loop 三层架构

INNGEST CTO 提出了生产级 Loop 的三层架构^[raw/articles/2026-06-21_TheAgentLoopArchitecture.md]：

| 层 | 说明 | 关键能力 |
|---|------|----------|
| **Loop** | cron + LLM 决策者，按计划触发、评估状态、决定下一步 | 定时触发、LLM 决策 |
| **Skill** | 耐久 workflow，可重试、可组合、可独立部署 | 多步骤、容错、复利 |
| **Orchestrator** | 调度引擎：管理 cron、重试、并发、运行历史 | 耐久执行、step 级 checkpoint |

耐久性是基础——不能在进程重启后继续运行的 loop 就不算 loop。`while True` 给不了这些能力：进程崩溃会丢失所有进行中的状态，导致重复执行、重复通知^[raw/articles/2026-06-21_TheAgentLoopArchitecture.md]。

Agent 可以编写自己的 Skill 并注册到编排引擎中——这就是 **orchestration-aware agent**（具备编排感知能力的 Agent）。Agent 是短暂的，它创建的耐久 Skill 是持久的^[raw/articles/2026-06-21_TheAgentLoopArchitecture.md]。

## 四笔代价

花叔总结了 Loop Engineering 的四笔隐性成本^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]：

1. **验证债**：没人盯着的循环，也是没人盯着在犯错的循环，跑得越欢错得越离谱
2. **理解烂掉**：它越快交付你没亲手写的代码，你脑子里以为的代码和仓库里真实的代码差距就越大
3. **Token 失控**：自主循环烧多少 token 很难预测，账单有时很吓人
4. **认知依赖**：你会忍不住把 AI 吐出来的东西照单全收，慢慢就不想自己有判断了

> 同一个循环，两个人能用出完全相反的结果。一个人用它在自己吃得很透的活上跑得更快；另一个人用它来逃避「吃透」这件事本身。^[raw/articles/2026-06-18_《LoopEngineering橙皮书》发布！免费，开源.md]

## 务实观点：人留在环里

Greg Isenberg 播客嘉宾 Ras Mic 提出了清醒的实践框架^[raw/articles/2026-06-16_AIAgentLoop到底是什么？多数人一上来就用错了丨Greg.md]：

- **Human-in-the-loop**：人指挥、人治理、人批准——AI 建人看方向
- **Agentic loop**：人只进场一次，Agent 自驱工作——速度上去但偏航概率也上去
- 两者差别不在于「有没有 AI」，而在于**反馈链条由谁持有**

落地的核心判断标准：**这个 loop 有没有固定反馈引擎？** 代码评审有 Greptile 打分，SEO 批量页有公式和模板——这些能用 loop。但「做一个用户愿意留下来的新产品」的反馈源是一堆模糊的人类感受，没有外部信号给方向，loop 跑得越勤返工概率往往越高^[raw/articles/2026-06-16_AIAgentLoop到底是什么？多数人一上来就用错了丨Greg.md]。

超过 1000 行的改动，连好用的 loop 也会失真——改动面一大依赖关系暴涨，任何评审 agent 都会开始抓不稳重点^[raw/articles/2026-06-16_AIAgentLoop到底是什么？多数人一上来就用错了丨Greg.md]。

## 与现有概念的关系

Loop Engineering 整合并建立在多个已有工程实践之上：

- [[harness-engineering-deep-dive]] — Loop 的地基，Harness 决定 Loop 的上限
- [[agentic-engineering]] — Loop 的方法论保障，保证质量、安全与责任
- [[vibecoding]] — Loop 的入口场景，快速原型适合用 loop 加速
- [[skill-design-patterns]] — Loop 中可复用的工作流单元
- [[agents-md]] — Loop 中为 Agent 提供项目导航的配置标准
- [[prompt-context-harness-engineering]] — P/C/H 三维框架中 Loop 对应的演进层级
- [[coordination-engineering]] — Loop 中多 Agent 的协调与编排

## Satya Nadella：护城河不是模型，而是 loop

微软 CEO Satya Nadella 提出了两种资本框架^[raw/articles/2026-06-21_TheAgentLoopArchitecture.md]：
- **人力资本**：团队多年积累的知识和判断力
- **Token 资本**：基于基础模型构建出的 AI workflow、决策模式和已学习 Skill

两者一起复利：每个被改进的 workflow 产生更好的信号 → 更准确的 AI 行为 → 释放人的注意力处理更高判断的工作。**一家公司应该能替换掉一个通用型模型，而不失去它的学习系统里积累出的组织经验。**^[raw/articles/2026-06-21_TheAgentLoopArchitecture.md]

## 相关概念

- [[harness-engineering-deep-dive]] — Loop 的底层基础设施
- [[harness-engineering-patterns]] — 六种核心模式（持久化指令、上下文组装、分层记忆等）
- [[agentic-engineering]] — Agent 工程方法论
- [[vibecoding]] — 降低门槛的互补概念
- [[claude-code]] — 典型支持 Loop Engineering 的工具（`/goal`、`/loop`）
- [[codex]] — OpenAI 的编程 Agent（Automations、Goal 模式）
- [[managed-agents]] — Claude Code 的 Loop 管理模式
- [[context-rot]] — 长循环中需要对抗的上下文衰减问题
