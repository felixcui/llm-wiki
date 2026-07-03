---
title: Claude Code 功能性特性
created: 2026-04-30
updated: 2026-05-09
type: entity
tags: [tool, agent-framework, ai-coding]
sources:
  - raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md
  - raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md
  - raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md
  - raw/articles/2026-04-27-anthropic-launch-secrets.md
  - raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md
  - raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md
  - raw/articles/2026-04-25_ai_pm_catwu.md
  - raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md
  - raw/articles/2026-05-09-claude-code-html-effectiveness.md
  - raw/articles/2026-05-09-claude-code-html-effectiveness-cn.md
  - raw/articles/2026-05-09-claude-md-8-tips.md
---

# Claude Code 功能性特性

> 本页拆分自 [[claude-code]]，详述其功能性特性。

## REPL 主循环

Claude Code 的核心交互方式是 REPL 模式：用户输入自然语言指令，Agent 解析意图后自主规划执行路径，调用工具完成任务后返回结果并等待下一条指令。这种模式支持多轮连续交互，适合复杂工程任务的渐进式完成。

## Tool 系统

Claude Code 内置了丰富的工具集，涵盖：

- **文件操作**：读取、写入、编辑文件（精确文本替换，要求唯一匹配）
- **命令执行**：Shell 命令执行，带超时保护和输出截断
- **代码理解**：目录浏览、代码搜索、依赖分析
- **Web 能力**：搜索和网页内容抓取

Tool 调用遵循 Anthropic SDK 的 function calling schema，与底层 API 直接对齐，无中间层转换。^[raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md]

## CLAUDE.md 配置

Claude Code 支持 `CLAUDE.md` 项目级配置文件，用户可以在其中定义项目特定的编码规范、工具使用偏好和约束条件。这个机制已被多个开源框架（如 [[hermes-agent]]）兼容采纳，成为 AI Coding 领域的准标准。^[raw/articles/2026-04-25_深度解析HermesAgent如何实现“自进化”及其PromptContextHarness的设计实践.md]

## 1M 上下文管理

随着 Claude Opus 4.6 将上下文窗口从 200k 扩展至 1M，Anthropic 官方博客系统性地提出了五种 Session 管理策略，对应不同的上下文维护目的：^[raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md]

### Context Rot 问题

1M 上下文更容易触发 **Context Rot**——注意力被更多 token 分散，早期不相关内容干扰当前判断。autocompact 恰好在模型注意力涣散时触发，相当于让"最不聪明的时候"决定什么重要。^[raw/articles/2026-04-22_如何管理Claude Code 100w上下文？Anthropic官方推荐的五种使用指南.md]

### 五种操作

| 操作 | 场景 | 核心要点 |
|------|------|----------|
| **Continue** | 当前上下文仍有效 | 最省心，下一步所需信息还在就别动 |
| **/rewind** | 遇到错误尝试时 | 双击 Esc 回退到失败前节点，擦除错误记忆，带上新理解重新提示 |
| **/clear** | 开始新任务时 | 自己写 brief 开新会话，换来亲手筛过的干净上下文 |
| **/compact** | 需要压缩但保留主线 | 有损操作，最好手动触发并带 hint 引导保留什么 |
| | **Subagents** | 只需要汇总结果时 | 新开独立上下文跑完活，只把结论带回来，中间输出不污染主会话 |

**关键决策原则**：下一步要用到的信息，当前 Context 里还在吗？在就 Continue；只需最终结论就开 Subagent；开始新任务就 /clear。

*Subagent 的详细实践参见 [[claude-code-practice]]。*

## /ultrareview 超级审查功能

Claude Code 新增 `/ultrareview` 命令，在云端并行启动多 Agent 编队进行代码审查，是 PR 合并前的深度体检。^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]

### 3+1 架构

- **3 个 Explorer Agent**：各自独立分析代码改动（可能分别关注安全漏洞、逻辑错误、性能隐患），拥有独立 context window
- **1 个 Critic Agent**：负责质检，拿到三个 Explorer 的全部产出逐条验证——能否复现、是否片面、有无遗漏场景

只有经过 Critic 验证的 Bug 才出现在最终报告中，误报率显著低于单次 `/review`。

### 云端沙箱执行

ultrareview 必须在云端运行：4 Agent 真正并行 + Critic 沙箱执行代码复现问题。

- 命令：`/ultrareview`（当前分支）或 `/ultrareview 1234`（GitHub PR）
- 费用：3 次免费（至 2026-05-05），之后 $5-$20/次

### 版本信息

- **v2.1.113**（4月17日）：首次加入 /ultrareview

## System Prompt 架构

Claude Code 的系统提示词采用**静态 + 动态两部分**的动态组装机制，由三个层次组成（身份层、能力层、配置层），总计约 1.8 万 token。会话内保持系统提示词不变以最大化 KV Cache 命中率，同时配合动态上下文注入、Auto-compact 和 Subagent 隔离等上下文工程策略。详见 [[claude-code-system-prompt]]。

## 产品策略与团队

### 功能交付速度

Claude Code 团队在产品负责人 [[cat-wu]] 和技术负责人 [[boris-cherny]] 的带领下，将功能交付周期从**半年压缩到一天**。CLI 版功能最先上线，采用 [[ai-native-pm]] 方法论指导产品开发。^[raw/articles/2026-04-25_ai_pm_catwu.md]

### 源码泄露事件（2026年3月）

2026年3月底，Claude Code 源码通过 npm 包泄露。原因：Bun 构建产生的 source map 文件因 `.npmignore` 缺少 `*.map` 规则被发布到 npm registry，暴露了约 2000 个文件、51 万行源码。值得注意的是，此次发布经过两层人工审查仍被遗漏。^[raw/articles/2026-04-25_ai_pm_catwu.md]

[[cat-wu]] 对此回应：这是人为失误——有人在用 Claude 写 PR 更新包发布流程时出了差错。处理方式体现了 [[anthropic]] 的文化：将问题定性为流程问题而非个人问题，重点是从中学习并增加防护措施。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### 功能演进与模型能力

Claude Code 团队遵循"为未来模型做产品"的理念（参见 [[designing-for-future-models]]）。每次新模型发布，团队的第一件事是**删功能**——因为新模型能力直接覆盖了之前需要 harness 弥补的缺陷。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

**典型功能演进案例：**
- **To-do List**：早期模型做大规模重构时改 5 个调用点就停（实际 20 个），需要先列清单再逐个完成。Opus 4 后模型自己就能做到，to-do list 被删除
- **代码审查**：多个版本准确率不够，直到 Opus 4.5/4.6 达到可用水平。现在 [[anthropic]] 内部合并 PR 前必须先过 Claude 代码审查
- **/powerup**：内置教程引导功能，虽然违背了"产品应该直觉到不需要教程"的原则，但因功能太多、用户需要引导而保留^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### System Prompt 持续清理

每次新模型发布后，团队会通读整个 system prompt，逐段反思：模型还需要这个提醒吗？如果不需要了，就删掉。^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

### 产品演进路线图

[[cat-wu]] 将 Claude Code 的演进分为三个阶段：^[raw/articles/2026-04-27-anthropic-launch-secrets.md]

1. **单个任务成功**：给一个清晰的 prompt，模型稳定输出可合并的代码
2. **多任务并行**：2025 年底 multi-coding 已开始，用户同时跑约 6 个 Claude
3. **50 到几百个 Claude 并行**：需要远端执行，模型应能自我验证工作，且能从反馈中自我改进

### 竞品对比

#### vs GPT-5.5 / Codex

[[gpt-5.5]] 在 Terminal-Bench 2.0 上以82.7%领先 Claude Opus 4.7 的69.4%，但在 SWE-Bench Pro（58.6% vs 64.3%）和 BrowseComp（84.4% vs 90.1%）上仍落后。^[raw/articles/2026-04-25_GPT-5.5来了！我撤回了退订ChatGPT的决定.md]

#### vs DeepSeek V4

[[deepseek-v4]] 编码质量接近 Opus 4.6 非思考模式，但与思考模式仍有差距。^[raw/articles/2026-04-25_DeepSeekV4发布，全网最细解读&技术报告拆解.md]

#### vs 开源框架

[[openclaw]] 和 [[hermes-agent]] 等开源框架在架构上参考了 Claude Code 的 Tool/REPL 设计，但在持久运行、自进化能力、多模型兼容等方面提供了差异化功能。Claude Code 的核心优势在于与 Claude 模型的深度集成和商业级稳定性。

## HTML 输出格式

Claude Code 支持 [[html-agent-output-format|HTML 作为输出格式]]，替代传统的 Markdown 渲染。HTML 输出在视觉保真度、交互性和前端代码直接预览方面显著优于 Markdown，尤其在生成 UI 组件和完整页面时表现突出。^[raw/articles/2026-05-09-claude-code-html-effectiveness.md]^[raw/articles/2026-05-09-claude-code-html-effectiveness-cn.md]

## Loop 定时任务

Boris Cherny 强调 Loop（定时 Agent 任务）是 Claude Code 的未来方向——让 Agent 在无人监督下按计划执行任务。这使 Claude Code 从"交互式工具"进化为"自主执行平台"。^[raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md]

## Claude Design

Claude Code 集成了 Claude Design 设计系统，为 AI 生成的 UI 提供一致的设计语言和组件规范。

## Routines、Batch、Hooks

Claude Code 持续扩展其自动化能力：
- **Routines**：可复用的多步骤工作流模板
- **Batch**：批量处理模式，并行执行多个任务
- **Hooks**：生命周期钩子，在特定事件（如工具调用前后）触发自定义逻辑^[raw/articles/2026-05-09-boris-cherny-sequoia-engineering-future.md]

## CLAUDE.md 进阶实践

### 200 行上限

[[boris-cherny]] 建议 CLAUDE.md 控制在 200 行以内。过长的配置文件会导致模型注意力分散，反而降低输出质量。^[raw/articles/2026-05-09-claude-md-8-tips.md]

### 渐进式上下文三层披露

推荐采用三层递进的信息架构：
1. **全局摘要**（10-20行）：项目概述、核心约束、关键技术栈
2. **详细规范**（50-100行）：编码规范、目录结构、命名约定
3. **按需深入**（可选）：特定模块的详细说明，通过引用方式按需加载^[raw/articles/2026-05-09-claude-md-8-tips.md]

### 本地 CLAUDE.md

支持项目级（提交到仓库）和个人级（本地 `~/.claude/CLAUDE.md`）两级配置。个人级配置不应提交到仓库，用于存放个人偏好和本地环境信息。^[raw/articles/2026-05-09-claude-md-8-tips.md]

### MEMORY.md 长期记忆回路

CLAUDE.md 可引用 MEMORY.md 文件形成长期记忆回路——Agent 在任务执行过程中将关键发现和经验写入 MEMORY.md，后续会话自动加载这些积累的知识。这是 Claude Code 实现跨会话学习的重要机制。^[raw/articles/2026-05-09-claude-md-8-tips.md]

## 相关条目

- [[html-agent-output-format]] — HTML 作为 Agent 输出格式
- Claude Design — Claude Code 的设计系统
- [[mcp-model-context-protocol]] — 模型上下文协议
- [[claude-code-practice]] — Claude Code 使用实践
