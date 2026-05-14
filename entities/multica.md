---
title: Multica
created: 2026-04-28
updated: 2026-05-14
type: entity
tags: [open-source, agent-framework, tool]
sources: [raw/articles/2026-04-27-multica-interview.md]
---

# Multica

**Multica** is an open-source multi-agent collaboration platform founded by **Zhang Jiayuan**, a former TikTok engineer who previously built [DevvAI](https://devv.ai). The project gained 12K GitHub stars within one week of launch, signaling strong developer interest in multi-agent orchestration.^[raw/articles/2026-04-27-multica-interview.md]

## Team Composition

Multica operates with a lean hybrid team: **4 humans** plus **a dozen AI agents** working in concert. This reflects the platform's core thesis that [[ai-native-organization]] structures can achieve outsized output by treating agents as first-class team members rather than tools.^[raw/articles/2026-04-27-multica-interview.md]

## Core Thesis

> "The human is the slowest node in an AI team."

Multica's workflow follows a three-stage model:

1. **Task Dispatch** — Humans break work into issues/tasks
2. **Agent Autonomous Execution** — Agents self-organize and execute independently
3. **Human Review** — Humans review outputs rather than doing the work

This aligns with [[coordination-engineering]] principles where human oversight shifts from doing to directing.^[raw/articles/2026-04-27-multica-interview.md]

## Supported Agent Runtimes

Multica is runtime-agnostic and supports multiple agent backends:

- [[claude-code]]
- Codex
- [[openclaw]]
- [[hermes-agent]]
- Cursor

## Key Concepts

### Issue-Based Task Dispatch

Tasks are decomposed into discrete issues, similar to how engineering teams use GitHub Issues. Each issue becomes an autonomous work unit that an agent can pick up, execute, and return results for.

### Agent Idle Rate

Multica tracks **Agent Idle Rate** as a key efficiency metric — the percentage of time agents spend waiting for work versus actively executing. A lower idle rate indicates better task parallelization and dispatch quality.

### Coding Tool Evolution Stages

Multica's founder identifies four stages of AI coding tool evolution:

1. **Code Completion** — Autocomplete and inline suggestions
2. **AI Editor** — Full-file AI-assisted editing
3. **Local Coding Agent** — Autonomous agents that write and run code
4. **Task Dispatcher** — Multi-agent systems that decompose and parallelize work

Multica targets stage 4, representing a shift from single-agent to [[sub-agent-vs-agent-team]] paradigms.^[raw/articles/2026-04-27-multica-interview.md]

## See Also

- [[claude-code]] — Primary agent runtime supported by Multica
- [[openclaw]] — Open-source agent framework compatible with Multica
- [[hermes-agent]] — Agent runtime with skill system
- [[onyx]] — 另一个多 Agent 协作平台
- [[coordination-engineering]] — Human-agent workflow design principles
- [[sub-agent-vs-agent-team]] — Architectural tradeoffs in multi-agent systems
- [[ai-native-organization]] — Organizational structures optimized for AI-native teams
