---
title: Hermes Agent Architecture
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [agent-framework, architecture, open-source]
sources:
  - raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md
  - raw/articles/2026-04-22_万字保姆级教程：Hermes+KimiK2.6打造7x24hAgent军团.md
---

# Hermes Agent Architecture

> 架构子页面 — 详见主页面 [[hermes-agent]]。

## 五层架构

一条消息从输入到输出经过五层：^[raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md]

| 层 | 组件 | 职责 |
|---|---|---|
| **入口层** | CLI + 20+ 平台适配器 | 统一 MessageEvent（适配器模式） |
| **网关层** | GatewayRunner 常驻进程 | 连接管理、会话生命周期、Profile 隔离 |
| **执行层** | AIAgent (run_agent.py) | 上下文组装、模型推理、工具调度（项目心脏） |
| **扩展层** | 工具注册、Skill 系统、子 Agent 委托、MCP | 能力扩展 |
| **存储层** | SQLite + FTS5、MEMORY/USER.md、Skills 目录 | 持久化 |

### PTC（Programmatic Tool Calling）^[raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md]

模型写 Python 脚本，脚本内部通过 RPC 串行调用多个工具（web_search、read_file 等），将 N 次工具调用折成 **1 轮推理**。执行脚本时迭代预算退还（`execute_code` 单独计为零成本），预算全留给推理轮次。

### delegate_task（子 Agent 委托）^[raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md]

- **深度仅 1 层**（父→子），**并发上限 3 个**
- 子 Agent 拥有独立 50 轮迭代预算，禁用 5 个工具：`delegate_task`（防套娃）、`clarify`（不能反问用户）、`memory`（不污染共享记忆）、`send_message`（不直接对外发消息）、`execute_code`（子 Agent 不用 PTC 折叠）
- 父级每 30 秒心跳，用户中断时级联停止子 Agent
- 主 Agent 只看到任务描述和最终摘要，看不到子 Agent 中间过程

### Session 链（无损历史）^[raw/articles/2026-04-28-openclaw-vs-hermes-architecture.md]

上下文压缩时不覆盖旧对话，而是结束当前 session → 开新 session（摘要作为起点）→ `parent_session_id` 指回旧 session。形成链式追溯：模型层只看压缩版（省 token），SQLite 层保留完整原文（FTS5 全文可搜）。

## 三维设计：Prompt / Context / Harness

### Prompt Engineering：模型异构兼容

Hermes 采用动态拼装范式，基础结构与 [[openclaw]] 高度相似：定义 Agent 身份 → 加载 SOUL.md → 注入工具指南。差异在于：^[raw/articles/2026-04-25_深度解析HermesAgent如何实现\&quot;自进化\&quot;及其PromptContextHarness的设计实践.md]

- **强制工具使用指导**：针对不同模型的"性格差异"动态注入指令。Claude 天然强调工具使用；GPT/Codex 容易"只说不做"，需强调"执行而非描述"；Gemini/Gemma 需提醒使用绝对路径和并行调用。
- **跨平台配置兼容**：直接读取 OpenClaw 的 AGENT.md、SOUL.md、USER.md，以及 [[claude-code|Claude Code]] 的 CLAUDE.md、.cursorrules 等，实现零成本迁移。
- **多 IM 平台适配**：内置 WhatsApp、Slack 等平台的适配提示词。

### Context Engineering：比例阈值压缩 + 混合记忆

**上下文压缩**：采用相对阈值触发机制（非 OpenClaw 的绝对阈值），当上下文达到模型总窗口的 50% 时自动压缩。压缩策略为"头尾保留、中间摘要"。^[raw/articles/2026-04-25_深度解析HermesAgent如何实现\&quot;自进化\&quot;及其PromptContextHarness的设计实践.md]

**记忆架构**：采用"内部静态存储 + 外部动态扩展"的双层架构：
- **内部记忆**：通过 MEMORY.md / USER.md 等文件存储长期事实性知识，用 `<memory-context>` 标签包裹注入
- **外部记忆**：原生对接 Mem0、Honcho、Hindsight、Supermemory 等第三方记忆服务，支持向量检索和跨会话记忆

**上下文注入**：通过 `@` 符号实现资源即时挂载（如 `@file:main.py`、`@diff`、`@url:...`），将工具调用转化为上下文预加载，省去多轮推理。

### Harness Engineering：约束与运行保障

- **全生命周期 Hook**：提供 on_agent_start、on_tool_call、on_tool_result、on_pre_compress、on_memory_write、on_delegation 等10个生命周期钩子
- **14种错误分类体系**：将认证失败、超时、参数错误等异常标准化定义，每种预设自动恢复策略（重试、降级、修正）
