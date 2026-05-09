---
title: OpenChronicle
created: 2026-04-27
updated: 2026-04-27
type: entity
tags: [open-source, tool, agent-framework]
sources:
  - raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md
---

# OpenChronicle

**GitHub**: `github.com/Einsia/OpenChronicle`
**类型**: 开源 AI 记忆基础设施
**团队**: Vida（[[calvin]] 主导）

## 概述

OpenChronicle 是 [OpenAI Chronicle](https://openai.com) 的开源替代品，定位为**设备侧 AI 记忆基础设施**——独立处理、保存用户记忆，不与任何模型或 Agent Harness 强绑定。记忆所有权归属用户而非模型厂商，用户可接入本地模型保护隐私，也可让 [[claude-code]]、Codex、OpenCode 等任何具备 tool-using 能力的 Agent 接入。^[raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md]

OpenAI 于 2026-04-21 为 Codex 上线 Chronicle（仅 Pro 订阅 + macOS），Calvin 一天后（04-22）发布 OpenChronicle，当日冲上 X trending 第一。^[raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md]

## 核心技术选择

### AX Tree 优先 + 截图兜底

不走主流的"截图 OCR + RAG"路线，而是采用 macOS 系统层的 Accessibility API（AX Tree）作为主信号源。AX Tree 将屏幕内容结构化为树状文本描述，直接提供应用名、焦点位置、编辑文字、URL、按钮文本等结构化信息，无需走视觉管道。^[raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md]

AX Tree 三大优势：
- **便宜**：文本处理成本远低于图片 OCR
- **更准**：对意图信号的识别比 OCR 强很多
- **结构化**：存入数据库、做检索更方便

局限：Word、飞书等内部渲染绕开系统辅助通道的应用，AX Tree 只能读到顶栏和标题。因此采用**混合方案**——AX Tree 优先，截图兜底。^[raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md]

### 触发式捕获

截图基于触发器（光标运动、页面滚动、应用切换），而非定时轮询。避免看电影等场景下记忆频繁触发导致 Token 过量消耗与记忆污染。^[raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md]

### 成本估算

- 轻度使用（8h 工作日，静默记录）：**~$0.50/天**
- 重度使用（高频调用、深度交互）：**$3–5/天**
- 本地模型部署：仅需电费^[raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md]

## 记忆结构

所有应用操作进入同一个本地记忆池，按 7 个分类扁平存为 Markdown 文件：人、项目、工具、话题、联系人、组织、事件。通过 **MCP 协议**对外暴露为工具调用，Memory Layer 不再绑定某个产品。^[raw/articles/2026-04-27_记忆，是Agent基建｜对话Calvin@Vida.md]

这意味着在 Cursor 里做的设计决策、飞书里讨论的架构方向、Slack 里收到的需求反馈，可以在另一个对话窗口里被 Claude Desktop 直接承接。

## 支持平台

通过 MCP 一键集成：
- [[claude-code]]
- Claude Desktop
- Codex
- OpenCode

## 与 [[self-improving-agent]] 的关系

OpenChronicle 提供的设备侧记忆基础设施是 [[self-improving-agent]] Memory 子系统的一种**外部化实现**。Hermes 等框架将 Memory 维护为项目级文件（`MEMORY.md`/`USER.md`），而 OpenChronicle 将记忆提升为操作系统级基础设施——跨应用、跨 Agent、跨模型，实现"记住你怎么做事"而非仅"记住你说过什么"。
