---
title: Portable Memory Layer
created: 2026-04-29
updated: 2026-04-29
type: concept
tags: [agent-framework, memory]
sources:
  - raw/articles/2026-04-29-ai-agent-memorystack.md
---

# Portable Memory Layer

> A model-agnostic, tool-agnostic persistent memory layer that sits beneath all AI agents, ensuring memory survives session resets, model replacements, and harness changes.

## Problem Statement

AI agent builders face a fundamental challenge: memory is typically locked to a specific tool, model, or session. A preference set in Claude Code is invisible to Cursor. A lesson learned in one session is forgotten after reset. When models are replaced or harnesses change, accumulated knowledge is lost.

## Core Principles

A portable memory layer, as articulated by [[codejunkie99]] (creator of [[brain]]) and inspired by Harrison Chase (LangChain CEO) and Taranjeet (Mem0), should satisfy these requirements:^[raw/articles/2026-04-29-ai-agent-memorystack.md]

1. **Portable**: Same memory across all tools (Claude Code, Cursor, Codex, OpenClaw, Hermes)
2. **Durable**: Survives session resets, model replacements, harness changes
3. **Typed**: Structured event types with typed payloads, not opaque text blobs
4. **Indexable**: Fast full-text and structured search
5. **Auditable**: Complete history of what was remembered, when, and by whom
6. **Syncable**: Push/pull across devices using standard git mechanisms
7. **Secure**: Secrets filtered before storage, forgery-resistant commit validation

## Architecture Pattern

The reference architecture (from [[brain]]) uses a three-layer design:^[raw/articles/2026-04-29-ai-agent-memorystack.md]

```
Git (Source of Truth, Audit Trail)
    ↓ derived from
SQLite FTS5 (Fast Search Index, BM25)
    ↓ built on
Orchestration Layer (Two-Phase Write, Catch-up)
    ↓ exposed via
MCP Protocol (5 Tools: ping/note/ask/log/doctor)
    ↓ consumed by
Adapters (Claude Code / Cursor / Codex / OpenClaw / Hermes)
```

### Why Git as Storage Layer

Alternatives considered and rejected:
- **SQLite-only**: Fast but lacks replication, conflict resolution, and audit trail
- **Markdown files**: Human-readable but slow to parse, race conditions on updates
- **DuckDB/PostgreSQL**: Overkill for single-user memory, can't sync across laptops
- **Git**: Each event = one commit = one file. Built-in audit log, push/pull sync, native merge, free backup

### Why SQLite FTS5 as Index

Git search is slow (`git log -S` takes seconds for 10K events). SQLite FTS5 with BM25 ranking provides top-5 results in under 1ms for 100K events. The index is disposable — can be rebuilt from git at any time.

## Event Types

A typed memory system defines distinct event types with structured payloads:

| Event Type | Purpose | Key Fields |
|-----------|---------|------------|
| Observe | "I chose X over Y" | subject_kind, content |
| Claim | Factual assertion | schema_type, key, content, supersedes |
| Lesson | Distilled pattern | body, source_event_ids |
| Pref | Preference | category, key, value |
| SkillEdit | Skill modification | skill_id, reason, diff |
| Verify | Attestation | claim_event_id, verdict |
| Archive | Hide without delete | target_event_id, reason |
| Redact | Roll back projection | target_event_id, reason |
| Import | Bulk load | source, count, strategy |
| Audit | System event | action, detail |

## MCP Integration

The portable memory layer exposes its capabilities through [[mcp-model-context-protocol]], making it consumable by any MCP-compatible agent. Key insight: **tool descriptions matter more than implementations** — prescriptive descriptions ("CALL THIS WHENEVER: before picking a library, pattern, or config value") dramatically increase agent adoption compared to generic descriptions.^[raw/articles/2026-04-29-ai-agent-memorystack.md]

## Relation to Other Concepts

- **[[harness-engineering-deep-dive]]**: Memory is Layer 2 in the Harness seven-layer model. A portable memory layer is the production-grade implementation of this layer.
- **[[agent-technical-debt]]**: The Context Lake module in the seven-agent-infrastructure framework requires persistent, cross-tool memory to avoid decision-trail debt.
- **[[openchronicle]]**: Another memory infrastructure approach using AX Tree instead of Git + SQLite.
- **[[self-improving-agent]]**: Self-evolution requires persistent memory to accumulate lessons across sessions.
