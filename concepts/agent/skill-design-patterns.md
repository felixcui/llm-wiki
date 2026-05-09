---
title: Skill Design Patterns
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [agent-framework, tool]
sources: [raw/articles/2026-04-28-workflow-skill-patterns.md, raw/articles/2026-04-28-google-agent-skill-toolkit.md]
---

# Skill Design Patterns

**Skill Design Patterns** are reusable architectural patterns for writing workflow-type Agent Skills — structured instruction sets that give AI agents domain-specific capabilities beyond their base model training. The patterns emerged from practical experience building production workflow skills across multiple agent platforms.^[raw/articles/2026-04-28-workflow-skill-patterns.md]

## Background

The [[agent-skill-specification]] defines an open standard (agentskills.io) for packaging agent capabilities. Skills are typically loaded by tools like [[claude-code]], [[superpowers]], [[openclaw]], and others via a [[skvm]] (Skill Virtual Machine) or equivalent runtime.^[raw/articles/2026-04-28-workflow-skill-patterns.md]

Key skill collections in the ecosystem:

| Source | Skills | Focus |
|--------|--------|-------|
| obra/superpowers | 14 workflow skills | General coding workflows |
| OpenAI Codex skills | Official | Codex-specific tasks |
| Google Labs code/stitch-skills | Toolkit | Design & UI workflows |
| trailofbits/skills | Security audit | Security-focused review |
| VoltAgent/awesome-agent-skills | 500+ index | Community skill catalog |

## 5 Core Patterns

### 1. Linear Workflow

Sequential steps executed in order. Each step produces output that feeds into the next.

- **Use case:** TDD skill (write tests → run tests → implement → refactor)
- **Key trait:** Predictable, easy to debug, good for well-understood processes

### 2. Decision Tree + Lazy Loading

Branch execution based on conditions encountered during the workflow. Detailed instructions are loaded only when a specific branch is taken.

- **Use case:** Debugging skill (classify bug type → load specific diagnostic steps)
- **Key trait:** Saves context window space; avoids overwhelming the agent with irrelevant detail

### 3. Iterative Loop

A generate → evaluate → refine cycle that repeats until convergence criteria are met.

- **Use case:** Code review skill (draft review → check against rules → refine feedback)
- **Key trait:** Must define explicit convergence criteria to prevent infinite loops

### 4. Baton Handoff

State transfer across sessions via files, context summaries, or structured artifacts.

- **Use case:** Multi-session research skill (session 1 gathers data → session 2 synthesizes)
- **Key trait:** Enables long-running workflows that exceed single conversation limits

### 5. Multi-Phase + Checkpoints

Phased execution where each phase ends with a review gate that must be passed before proceeding.

- **Use case:** Architecture skill (design phase → review gate → implementation phase → review gate)
- **Key trait:** Provides natural breakpoints for human oversight^[raw/articles/2026-04-28-workflow-skill-patterns.md]

## Anti-Laziness Techniques

A common failure mode for agent skills is the model taking shortcuts. Effective skills counter this with:

- **Strict tone** — Imperative language with no hedging ("MUST run tests", "DO NOT skip")
- **Excuse-refutation tables** — Pre-empt common reasons the model might skip steps and explicitly forbid them
- **Quantitative thresholds** — Require specific numbers (e.g., "at least 3 test cases", "coverage > 80%")
- **Negative instructions** — Explicitly list what NOT to do, not just what to do^[raw/articles/2026-04-28-workflow-skill-patterns.md]

## 3-Layer Knowledge Architecture

Effective skills follow a layered information architecture:

| Layer | Location | Size | Purpose |
|-------|----------|------|---------|
| Frontmatter | YAML header | ~100 tokens | Metadata, triggers, routing |
| SKILL.md body | Main file | 2K–5K tokens | Core instructions, patterns |
| references/ | Subdirectory | On-demand | Extended docs, examples, checklists |

This keeps the base invocation cost low while making deep knowledge available when needed.^[raw/articles/2026-04-28-workflow-skill-patterns.md]

## Token Budget

Total context cost per skill invocation should stay under **~10K tokens** to leave sufficient room for the agent's conversation context, code, and other skills. This budget drives the lazy loading and layered architecture decisions above.

## See Also

- [[agent-skill-specification]] — Open standard for agent skill packaging
- [[claude-code]] — Agent runtime with native skill support
- [[superpowers]] — Collection of 14 workflow skills
- [[openclaw]] — Open-source agent framework
- [[skvm]] — Skill Virtual Machine runtime concept
