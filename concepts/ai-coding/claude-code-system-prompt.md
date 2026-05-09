---
title: Claude Code System Prompt 架构
created: 2026-04-26
updated: 2026-04-26
type: concept
tags: [agent-framework, ai-coding]
sources:
  - raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md
  - raw/articles/2026-04-22_深度解析ClaudeCode在PromptContextHarness的设计与实践.md
  - raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md
---

# Claude Code System Prompt 架构

> 本页拆分自 [[claude-code]]，详述其系统提示词的三层架构与上下文工程实现。

## 动态组装机制

Claude Code 的系统提示词并非静态文件，而是采用**静态 + 动态两部分**的动态组装机制：^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

- **静态部分**：Agent 身份定义、基础行为规则
- **动态部分**：根据会话状态按需注入的工具指南、CLAUDE.md 配置、Memory 快照等

动态组装的一个关键设计是**会话内保持系统提示词不变**，以利用 KV Cache 的前缀缓存（Prefix Cache）机制，避免每轮 API 调用重新计费。新写入的内容只改磁盘，下一个会话才刷新。这一设计理念与 [[hermes-agent]] 的 MemoryStore 冻结快照机制异曲同工。

## 详细架构（源码级解析）

Claude Code 的系统提示词由三个层次组成，总计约 1.8 万 token：^[raw/articles/2026-04-22_深度解析ClaudeCode在PromptContextHarness的设计与实践.md]

**第一层：身份层（Identity）**
- Claude Code 的自我定义、交互模式（REPL）、多轮对话策略
- 软技能规范：直接回答、不回避问题、使用恰当语气

**第二层：能力层（Capabilities）**
- 6 个内置 AgentTool 的详细使用指南：
  1. **Bash** — Shell 命令执行（最长 120s，带超时保护和输出截断）
  2. **Read** — 文件读取（精确行号，最大 2000 行）
  3. **Write** — 文件写入（全量替换，自动创建目录）
  4. **Edit** — 精确文本替换（9 种模糊匹配策略，自动语法检查）
  5. **MultiEdit** — 多文件批量编辑
  6. **Search** — 内容搜索（Ripgrep 引擎，支持正则和 glob）
- 每个工具都有完整的参数定义、使用约束和边界说明
- 工具调用遵循 Anthropic SDK 的 function calling schema，与底层 API 直接对齐^[raw/articles/2026-04-25_800行代码实现OpenClaw的Tool、消息总线、子Agent管理架构.md]

**第三层：配置层（Context）**
- CLAUDE.md 项目级配置
- 当前工作目录和文件结构
- Git 状态信息
- 会话历史摘要

## 上下文工程（Context Engineering）

Claude Code 在上下文管理方面有精细设计：^[raw/articles/2026-04-22_深度解析ClaudeCode在PromptContextHarness的设计与实践.md]

- **动态上下文注入**：根据当前任务状态，按需加载相关文件、工具输出和配置
- **Prefix Cache 优化**：系统提示词会话内保持不变，最大化 KV Cache 命中率
- **Auto-compact**：当上下文接近模型窗口上限时自动压缩，采用"头尾保留、中间摘要"策略
- **Subagent 隔离**：子 Agent 在独立上下文窗口中运行，只将最终结果返回主会话

## Harness Engineering 实现

作为 [[prompt-context-harness-engineering]] 三维框架的标杆实现，Claude Code 的 Harness 层设计特点包括：^[raw/articles/2026-04-22_深度解析ClaudeCode在PromptContextHarness的设计与实践.md]

- **REPL 主循环**：Read-Eval-Print Loop 交互模式，支持多轮连续交互
- **工具沙箱**：每个工具调用有独立的超时、权限和输出限制
- **错误分类体系**：标准化错误处理，区分可恢复和不可恢复异常
- **会话管理**：Continue、/rewind、/clear、/compact、Subagents 五种操作
- **与 Claude 模型深度集成**：模型之外那一整套工程壳子已做到相当成熟，Anthropic 官方工程文章直接将 Claude Code 称为一种优秀的 Harness 实现

## Claude Code 的三维工程总结

| 维度 | Claude Code 实现 | 关键设计 |
|------|----------------|---------|
| Prompt Engineering | 三层系统提示词架构 | 静态身份 + 动态能力 + 按需配置 |
| Context Engineering | Prefix Cache + 动态注入 | 会话内系统提示词不变，按需加载资源 |
| Harness Engineering | REPL + 工具沙箱 + 会话管理 | 商业级稳定性的运行时保障 |

## 相关页面

- [[claude-code]] — Claude Code 产品总览
- [[prompt-context-harness-engineering]] — Prompt/Context/Harness 三维分析框架
- [[hermes-agent]] — 参考 Claude Code 架构的开源自进化 Agent 框架
- [[context-rot]] — 上下文腐烂问题与 1M 上下文管理策略
