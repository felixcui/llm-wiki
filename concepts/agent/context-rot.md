---
title: Context Rot（上下文腐烂）
created: 2026-04-22
updated: 2026-05-07
type: concept
tags: [inference, data, agent-framework]
sources:
  - raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md
  - raw/articles/2026-04-27-ai-programming-tipping-point.md
  - raw/articles/2026-04-30_开源「洁癖.skill」，让你的Agent越用越聪明。.md
  - raw/articles/2026-05-07-aicoding-guide.md
---

# Context Rot（上下文腐烂）

长会话中上下文质量随长度增加而缓慢衰减的现象。注意力被更多 Token 分散，早期不相关内容开始干扰当前任务判断。窗口越大（如 1M context），能塞进去的东西越多，模型判断什么重要什么不重要的压力也越大，Context Rot 反而更容易触发。^[raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md]

## Claude Code 的五种 Session 管理操作

Anthropic 官方博客提出五种会话管理策略，每种对应不同目的：^[raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md]

### 1. Continue（继续）

最省心的操作。当前上下文仍在起作用时直接继续，不做任何变更。判断标准：下一步要用到的信息，当前 context 里还在吗？还在就继续。

### 2. Rewind（回退）

双击 Esc 或 `/rewind`，回退到之前任意一条消息，后面的记录全部丢弃。比"追一句纠正消息"效果好得多——追纠正会留下"失败尝试+纠正"两层信息，rewind 则直接擦掉错误记忆，Claude 看到的是干净的、带最新理解的指令。

配套用法：配合 "summarize from here" 或 `/rewind`，让 Claude 先把踩坑经验总结成 handoff 再 rewinding，相当于给未来的自己留便条。

### 3. /clear（清空开新会话）

自己写 brief 开新会话。如"我们在重构 auth 中间件，约束是 X，相关文件是 A 和 B，已排除 Y 方案"。比 compact 累（要自己敲），但换来的是一份亲手筛过的干净上下文。原则：开始新任务就开新 session，不要因惰性在旧会话里继续。

### 4. /compact（压缩）

把前面的对话交给模型自己总结，有损但省事。可加提示引导：`/compact focus on the auth refactor, drop the test debugging`。

### 5. Subagents（子 Agent）

只需要最终结论时开 subagent，还需要中间输出就别开。subagent 用独立上下文窗口跑完所有活，只把结论带回来。典型场景：在大代码库中搜索关键字并归纳。

## Auto-compact 陷阱

Auto-compact 自动压缩翻车的核心原因：^[raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md]

1. **模型不知道你下一步要干嘛**：压缩时可能把后面需要的关键信息当作无关内容丢弃
2. **触发时机恰好是模型最不聪明的时候**：autocompact 在 context rot 已经发生时触发，相当于让注意力涣散的人决定什么重要
3. **典型案例**：长调试会话后 autocompact 总结出调试要点，但用户下一步要修的 warning 恰好在压缩时被丢弃

建议：在 `/config` 中将 Auto-compact 设为 false，改为手动 `/compact` 并带上 hint。

## Token 预算管理

[[claude-code]] 的上下文通常包括：系统提示符、CLAUDE.md、当前对话、每次工具调用及其输出、已读取的文件。上下文从 200k 扩展到 1M 后，管理策略更加重要：^[raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md]

- 200k 时代：关闭 auto compact 省 ~40k 空间、精简 MCP 工具、CLAUDE.md 尽量简洁
- 1M 时代：有更多腾挪空间，每次会话管理决策更值钱，但 context rot 风险也更大

## /usage 命令

Anthropic 新增的 `/usage` 斜杠命令，用于查看 Claude Code 的使用情况，辅助会话管理决策——帮助用户判断当前会话是继续、压缩还是清空。^[raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md]

## 决策速查表

| 场景 | 推荐操作 |
|------|---------|
| 当前 context 仍在起作用 | Continue |
| Agent 尝试失败，想换个方向 | Rewind 到失败前 |
| 开始完全不同的新任务 | /clear |
| 上下文快满了但任务还在继续 | /compact + hint |
| 需要大量中间读取，只要最终结论 | Subagent |

^[raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md]

## 大型项目中的上下文衰减

在大型代码库（40 万文件级别）中，消费级 AI 工具的架构理解能力下降约 77%。关键发现：所有主流模型在 context 使用率上升时，注意力都是持续衰减的——「不是超了才错，而是越满越飘」。^[raw/articles/2026-04-27-ai-programming-tipping-point.md]

上下文衰减是 [[comprehension-debt]]（理解债）的物理层触发因素。当 AI 因上下文过载而违反隐性架构约束时，生成的代码虽然能通过测试，但开发者在未来维护时将面临更高的认知成本。

## 本质

会话的上下文管理（参见 [[prompt-context-harness-engineering]]），本质上是在帮模型把注意力维持在高水位。1M context 给了更多腾挪空间，也让每一次管理决策变得更值钱。

## 上下文保洁实践

除会话内的 Token 管理外，跨会话的文档体系维护是防止上下文腐烂的另一关键维度。[[self-improving-agent]] 中的「洁癖.Skill」（Neat.Skill）提供了一套可操作的上下文保洁实践。^[raw/articles/2026-04-30_开源「洁癖.skill」，让你的Agent越用越聪明。.md]

### 文档腐败是隐藏的 Context Rot 源头

Agent 所依赖的上下文不止单次对话的聊天记录，还包括项目中的各种文档、约束和记忆。当项目代码迭代了七八轮而文档仍停留在 1.0.0 版本时，Agent 会基于过时信息做出错误决策——这正是 Context Rot 的文档版表现。一条过期记忆比没有记忆更糟糕，因为 Agent 会信任它并基于错误前提行事。

### 保洁周期建议

实践中建议采用的保洁节奏：

- **每次任务收尾**：运行一次保洁操作（如 `/neat`），将当前变更同步到所有文档层。类比游戏存档——「这次要结束了，要存档了」。
- **400K Token 窗口**：在长会话中，约 400K Token 时进行一次文档存档并新开窗口。即使 [[claude-code]] 支持 1M 上下文，实践中 500K 左右注意力就开始异常。
- **新开窗口无需提示**：只要文档维护到位，新会话直接说任务即可，Agent 能精准响应——这是保洁效果的终极验证。

### 三层保洁 vs. 单层记忆

常见的 AutoDream 等功能只维护 Agent 记忆层（第一层），而完整的文档保洁需要覆盖三层：Agent 记忆 / CLAUDE.md / docs/ 目录。只保洁记忆不保洁文档，相当于只擦黑板不更新教材——新来的 Agent 读到的仍是过时的课程大纲。

### 与上下文管理操作的关系

[[claude-code]] 的五种 Session 管理操作（Continue、Rewind、Clear、Compact、Subagent）处理的是单次会话内的上下文腐烂；洁癖.Skill 处理的是跨会话的文档体系腐烂。两者互补：前者解决「这次对话太长如何管理」，后者解决「项目文档如何不烂」。组合使用效果最佳——任务收尾时先保洁文档再新开窗口。

## 防腐实践模式

除了会话管理和文档保洁，AI 编程实践中还有一些有效的上下文防腐策略：^[raw/articles/2026-05-07-aicoding-guide.md]

### Think before Code（先思考再编码）

在让 AI 动手写代码之前，先让它在脑中"想清楚"再输出。具体做法是在 prompt 中要求 AI 先输出方案概述或伪代码，确认方向正确后再进入实现阶段。这相当于在 [[claude-code]] 的 Plan Mode 中增加了结构化的思考环节，避免 AI 在方向错误的情况下生成大量无效代码，浪费上下文窗口。

### 反问模式（Ask Back）

当用户的需求描述不够清晰时，让 AI 主动反问而非自行假设。例如，在 [[agents-md]] 中配置"遇到模糊需求时，列出需要澄清的问题清单再继续"。反问模式能有效防止 AI 基于错误假设生成代码，而这些错误假设一旦进入上下文，后续修正会消耗大量 token 并加剧 Context Rot。

### 边界条件约束

在上下文中明确列出边界条件和"不要做什么"，比只说"做什么"更能防止 AI 跑偏。实践中建议在 [[spec-driven-development]] 的 spec 文件中设立专门的"边界条件"和"非目标"章节。边界条件越具体，AI 在长会话中偏离的概率越低。

### 多轮 Review 策略

对于复杂任务，不要在一次长会话中完成所有工作。推荐采用"短会话 + 多轮 review"策略：每完成一个功能模块就新开会话，用前一个会话的输出作为新会话的输入。这从根本上避免了超长会话中的注意力衰减，也与 [[harness-engineering-deep-dive]] 中"上下文加载是七层模型中最难的点"这一洞察一致。
