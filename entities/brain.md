---
title: brain — Portable Memory Layer
created: 2026-04-29
updated: 2026-04-29
type: entity
tags: [tool, agent-framework, memory]
sources:
  - raw/articles/2026-04-29-ai-agent-memorystack.md
---

# brain

> A Rust-based portable memory layer for AI agents. Git-backed event log + SQLite FTS5 index + MCP interface. Supports Claude Code, Cursor, Codex, OpenClaw, Hermes, or any MCP/shell-compatible tool.

Repository: [github.com/codejunkie99/brain](https://github.com/codejunkie99/brain)

## Architecture

```
brain/
├── crates/
│   ├── brain-types     — events, payloads, idempotency keys
│   ├── brain-store     — git-backed event log via libgit2
│   ├── brain-index     — SQLite FTS5, BM25, projections
│   ├── brain-app       — orchestration, two-phase write, catch-up
│   ├── brain-mcp       — rmcp stdio server
│   ├── brain-cli       — `brain` binary
│   └── brain-tui       — full-screen ratatui dashboard
└── adapters/
    ├── claude-code     — MCP config + CLAUDE.md addendum
    ├── cursor          — MCP config + .mdc rule
    ├── codex           — MCP TOML + AGENTS.md addendum
    ├── openclaw        — system-prompt file
    └── hermes          — system-prompt addendum
```

Built with 7 Rust crates, ~9000 lines of code, and 143 regression tests.^[raw/articles/2026-04-29-ai-agent-memorystack.md]

## Core Design

### Three-Layer Storage

1. **Git (Source of Truth)**: Each event is a git commit. Git provides atomic writes, audit trail, push/pull sync, native merge, diff visualization, and free backup via any code hosting platform.
2. **SQLite FTS5 (Derived Index)**: Built on demand from Git. BM25-ranked search with weighted fields (title 10x, tags 5x, body 1x). Top-5 results in under 1ms for 100,000 events.
3. **Orchestration Layer**: Two-phase write (git first, then index) with catch-up reconciliation on open.

### Event System

10 typed event payloads, each with structured metadata:

- **Observe** — "I chose X over Y"
- **Claim** — "warfarin + ibuprofen is dangerous"
- **Lesson** — distilled pattern across episodes
- **Pref** — "I always use tabs"
- **SkillEdit** — "I modified skill X for reason Y"
- **Verify** — attestation on a prior claim
- **Archive** — hide without deleting
- **Redact** — scrub and roll back projections (like `git revert` for memory)
- **Import** — bulk load from external source
- **Audit** — system event

Each event carries full metadata: `event_id` (UUIDv7, time-sortable), `actor.kind` + `actor.id` + `actor.harness` (for cross-tool provenance), `chain_id`, `time_observed`, `time_recorded`, `layer`, `authority`, `signature_state`, `classification`, `schema_version`, `idempotency_key`.^[raw/articles/2026-04-29-ai-agent-memorystack.md]

### MCP Server (5 Tools)

| Tool | Purpose |
|------|---------|
| `ping` | Health check |
| `note` | Save persistent note to long-term memory |
| `ask` | Search long-term memory (top 5, prefix-matching) |
| `log` | Browse event history |
| `doctor` | Repair and rebuild index |

Tool descriptions are written prescriptively — telling the agent **WHEN** to call each tool, not just **WHAT** it does. This proved critical for agent adoption.^[raw/articles/2026-04-29-ai-agent-memorystack.md]

### Security

- **Secret Prefilter**: 3-pass regex detection (raw scan → zero-width strip → NFKC normalize) for API keys, tokens, PEM keys, JWT, database URIs, etc.
- **Forgery Defenses**: Trailer-block parsing (event_id only from last paragraph), blob-trailer cross-check, filename-in-parent skip
- **Subject Scrubbing**: Sensitive event types (Observe, Lesson, Redact) use event_id as commit subject instead of echoing user text

## Sync

Brain is just a git repo. Sync is `git push` / `git pull` wrapped as CLI commands, inheriting SSH keys, HTTPS credential managers, 2FA, macOS Keychain, and enterprise SSO for free.^[raw/articles/2026-04-29-ai-agent-memorystack.md]

## Cross-Tool Testing

A note written in Claude Code is immediately visible in Cursor, Codex, OpenClaw, and Hermes — all pointing to the same `~/.brain` directory. This is the core value proposition: shared memory across any tool, surviving session resets, model swaps, and harness changes.

## Related

- [[codejunkie99]] — Creator of brain
- [[portable-memory-layer]] — The concept this project embodies
- [[openchronicle]] — Alternative AI memory infrastructure (AX Tree-based)
- [[mcp-model-context-protocol]] — MCP protocol used for agent integration
