---
title: 来新璐
created: 2026-05-15
updated: 2026-05-15
type: entity
tags: [person, agent-framework, open-source]
sources:
  - raw/articles/2026-05-14_探秘ClaudeCode，搞懂AgentHarness｜对谈来新璐.md
---

# 来新璐

ShareAI 开源社区发起人，23 岁（2026 年），毕业于河南工业大学。撰写维护的《Learn Claude Code》教程在 GitHub 上获得超过 50K Star。创立 Komputer Blue（KB），完成数百万美金融资。

## 核心观点

### "模型以外都是 Harness"

> 模型是聪明的大脑，但它没有身体和手脚，只能思考，没办法行动。Harness 就是给它装上机甲。

### Agent Harness 三层框架

来新璐将 [[harness-engineering-deep-dive|Harness]] 拆解为三层：

1. **执行层（会跑）**：CLI、代码编写、工具注册、MCP 扩展 —— 给模型提供 action 能力
2. **状态层（跑久）**：System Prompt、Skills、Memory、上下文卸载策略 —— Agent 接力赛的关键
3. **治理层（跑稳）**：多 Agent 组织架构、权限隔离、信息提供和隔离 —— 管理 100 个 Agent 的治理问题

### 关键判断

- **"Bash is all you need"** 是共识 —— CLI 比 [[mcp-model-context-protocol|MCP]] 更通用
- **更多 Context，更少 Control** —— Claude Code 的哲学是给模型充分的自由
- **Prompt Flow 范式正在过时** —— LangChain/LangGraph 基于提示词节点做流转的方法论不再适用
- **"Agent 即模型，模型即 Agent"** —— 新的 Agent Native 范式

## Komputer Blue 产品线

围绕 Agent 三层构建的完整工具链：

| 产品 | 定位 |
|------|------|
| Komputer | KB 级虚拟 Unix 计算机（数据结构实现），为 Agent 提供生活环境 |
| Kruntime | Agent runtime，提供开发 Agent 对象的接口和语法糖 |
| Kwatch | 观测层，追踪 Agent 在什么情况下卡住 |
| KRL | 导出数据做强化学习，训练自己的模型 |

核心创新：用 TypeScript 重写虚拟 bash、虚拟磁盘文件系统、前后台进程、虚拟时钟、局域网组网 —— 全部在内存中运行，体积仅 KB 级。

## Claude Code 记忆机制解读

- **Turn stop hook**：每次交互后 fork 一个 Agent，复用 KV Cache，判断需保存的信息，更新 Markdown 文件
- **Auto-dream**：每隔一天、session 数 > 5 时，启动更深层记忆整理 —— "像做梦一样"回顾最近会话

与 [[self-improving-agent|自进化 Agent]] 和 [[portable-memory-layer|记忆系统]] 设计理念高度相关。
