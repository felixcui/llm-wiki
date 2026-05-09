---
title: codejunkie99 (@Av1dlive)
created: 2026-04-29
updated: 2026-05-09
type: entity
tags: [person, agent-framework]
sources:
  - raw/articles/2026-04-29-ai-agent-memorystack.md
  - raw/articles/2026-05-09-ai-engineer-roadmap-2026.md
---

# codejunkie99 / @Av1dlive

> AI agent builder, creator of [[brain]] — a Rust-based portable memory layer for AI agents.

## Overview

codejunkie99 (X handle @Av1dlive) is an AI agent builder who created [[brain]], an open-source portable memory layer for AI agents. The project was inspired by insights from Harrison Chase (LangChain CEO) on "memory should be open" and Taranjeet from Mem0 on "memory lock-in."

The work represents approximately 9 weeks of development across 7 Rust crates, 5 adapter directories, ~9000 lines of code, and 143 regression tests.^[raw/articles/2026-04-29-ai-agent-memorystack.md]

## Key Contributions

- **Portable Memory Layer**: Recognized that memory should be typed, indexable, auditable, syncable, and secure — not just open or portable. Built a complete stack around this philosophy.
- **Git-backed Event Log**: Used Git as the source of truth for memory, with each event corresponding to a commit. This provides built-in audit trails, push/pull sync, native merge, and free backup via code hosting platforms.
- **MCP Integration**: Exposed memory as 5 MCP tools, enabling any MCP-compatible agent (Claude Code, Cursor, Codex, OpenClaw, Hermes) to read/write to the same memory store.
- **Defense-in-Depth Security**: Implemented a 3-pass secret prefilter (RegexSet with NFKC normalization, zero-width character stripping), forgery defenses (trailer-block parsing, blob-trailer cross-check, filename-in-parent skip), and subject scrubbing for sensitive event types.

## Development Philosophy

- **Typed Memory is Queryable Memory**: Events have rich type metadata (Observe, Claim, Lesson, Pref, SkillEdit, Verify, Archive, Redact, Import, Audit) with structured payloads, enabling SQL-based retrieval rather than string matching on markdown blobs.
- **Tool Description > Tool Implementation**: Discovered that MCP tool descriptions written prescriptively ("CALL THIS WHENEVER...") get invoked far more than generic descriptions ("Save a note").
- **Test UX, Not Just Unit Correctness**: Learned the hard way — a source-pollution bug persisted until week 9 because no test verified "a note that doesn't mention X shouldn't match a search for X."

## Related

- [[brain]] — The portable memory layer project
- [[portable-memory-layer]] — The concept of model-agnostic, tool-agnostic persistent memory
- [[openchronicle]] — Another AI memory infrastructure project (AX Tree-based)

## AI Engineer Roadmap 2026

codejunkie99 发布了一份为期 17 周、6 个阶段的 [[ai-engineer-roadmap|AI 工程师路线图]]，系统性地指导开发者从零构建 AI Agent 能力。^[raw/articles/2026-05-09-ai-engineer-roadmap-2026.md]

### 关键洞察：Harness 决定性

路线图中最具影响力的发现是：**同一模型在不同 harness 下结果差异巨大**。具体数据：Opus 4.5 在 Claude Code 中达到 78% 的任务完成率，但在 Smolagents 中仅 42%。这直接印证了 Martin Fowler 的 Agent = Model + Harness 公式中 Harness 的决定性作用。^[raw/articles/2026-05-09-ai-engineer-roadmap-2026.md]

### 1500 行 mini-harness

codejunkie99 提出用约 1500 行代码构建 mini-harness 即可显著提升 Agent 表现，降低了 Harness Engineering 的入门门槛。这一实践与 [[claude-agent-sdk]] 的设计理念相互呼应。^[raw/articles/2026-05-09-ai-engineer-roadmap-2026.md]
