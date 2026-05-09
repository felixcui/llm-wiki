---
title: 付志远 (zhiyuanfu)
created: 2026-05-08
updated: 2026-05-08
type: entity
tags: [person, practice]
sources:
  - raw/articles/2026-05-08-veteran-developer-agent-journey.md
---

# 付志远 (zhiyuanfu)

腾讯程序员，十年开发经验。在 AI Agent 工程化领域有深入实践，以"24h 打工人"系统闻名——一个能 24 小时无人值守运行的 Agent 调度系统。

## 核心贡献

### "24h 打工人"系统

构建了一个基于文件系统 + 轮询调度的 Agent 管理层（非重新造 Agent，而是管理现有 Agent 工具如 [[codex]]、[[claude-code]]、Gemini CLI 的调度层），支持 20-30 个并发任务稳定执行。核心特性包括：^[raw/articles/2026-05-08-veteran-developer-agent-journey.md]

- **文件队列 + JSON 状态管理**：任务以文件形式入队，状态持久化到 JSON
- **智能并发**：组间并发（前端/后端同时跑）、组内串行（同项目排队）、失败隔离
- **工具自动切换**：配额耗尽自动从 Codex 切到 Gemini CLI 再到 Claude
- **Agent 自举**：系统曾自行修复自身 bug，前提是完善的 SDD 流程和 constitution.md

### 工程理念

- **代码优先于 Prompt**：80% 的"AI 需求"不需要 AI，10 行 Bash 脚本即可解决。提出决策层级：目标 → 代码 → CLI → Prompt → Agent
- **脚手架 > 模型**：模型升级成本 +300% 效果 +20%，脚手架升级成本 +50% 效果 +200%
- **从 Task-Driven 到 Goal-Driven**：Task-Driven 解决执行问题，Goal-Driven 解决迭代问题
- **Agent 系统五层地基**：目标表达、能力单元、运行时状态、治理边界、评估反馈

## 相关概念

- [[goal-driven-agent]] — 其 Goal-Driven 理念的系统化
- [[vibecoding]] — 他曾经历 Vibe Coding 翻车，转而采用 SDD
- [[spec-driven-development]] — 其系统的核心方法论
- [[agentic-engineering]] — 其实践体现了这一理念
- [[agent-technical-debt]] — 其 Observability 和 Control Plane 实践直接应对技术债
