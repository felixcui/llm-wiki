---
title: DESIGN.md Specification
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [tool, ai-coding]
sources: [raw/articles/2026-04-28-design-md.md]
confidence: medium
---

# DESIGN.md Specification

**DESIGN.md** is a Markdown-based design system format that AI assistants can natively parse, introduced by Google's Stitch project (`github.com/google/skills`, `google-labs-code/stitch-skills`). It replaces traditional design artifacts — Figma exports, JSON config files, and proprietary design tool formats — with a structured but human-readable format optimized for AI-first development workflows.^[raw/articles/2026-04-28-design-md.md]

## Motivation

In AI-native development, design handoffs between designers and AI coding agents create friction. Traditional formats (Figma, Sketch, JSON schema) require parsers, plugins, or manual translation. DESIGN.md encodes design intent in plain Markdown that any LLM can read directly, eliminating the translation layer.^[raw/articles/2026-04-28-design-md.md]

This approach aligns with [[spec-driven-development]], where machine-readable specifications drive the development process rather than serving as documentation after the fact.

## 9 Core Sections

A complete DESIGN.md file contains the following sections:

| # | Section | Purpose |
|---|---------|---------|
| 1 | **Visual Theme** | Overall aesthetic direction (minimal, brutalist, playful, etc.) |
| 2 | **Color Palette** | Primary, secondary, accent colors with hex values and usage rules |
| 3 | **Typography** | Font families, sizes, weights, line heights, spacing scale |
| 4 | **Components** | UI component definitions with variants and states |
| 5 | **Layout** | Grid system, spacing, alignment rules |
| 6 | **Depth** | Shadows, z-index, layering rules |
| 7 | **Do's and Don'ts** | Explicit usage guidelines and anti-patterns |
| 8 | **Responsive Behavior** | Breakpoint-specific adaptations |
| 9 | **AI Prompt Guide** | Instructions for AI agents interpreting this design |

The AI Prompt Guide section is unique to DESIGN.md — it provides agents with hints on how to interpret ambiguous design decisions and make reasonable choices when the spec doesn't cover a specific case.^[raw/articles/2026-04-28-design-md.md]

## HyperDesign

**HyperDesign** is an open-source tool that auto-generates DESIGN.md files from existing design systems. It can ingest Figma files, style guides, or component libraries and produce a compliant DESIGN.md output.^[raw/articles/2026-04-28-design-md.md]

## Relation to Agent Skills

DESIGN.md integrates with the [[agent-skill-specification]] ecosystem. Google's `stitch-skills` package uses DESIGN.md as input for AI agents that generate UI code. Tools like [[claude-code]] can reference a project's DESIGN.md to maintain design consistency across AI-generated code changes.^[raw/articles/2026-04-28-design-md.md]

This is an instance of [[prompt-context-harness-engineering]] — structuring contextual information (in this case, design specifications) so that AI agents can use it effectively without manual prompting each time.

## Benefits

- **No parser needed** — Any LLM reads Markdown natively
- **Version-controllable** — Plain text works with git diff and review
- **Incremental** — Sections can be added as the design matures
- **AI-first** — Designed for agent consumption, not just human readability

## See Also

- [[spec-driven-development]] — Broader methodology of specification-driven AI coding
- [[prompt-context-harness-engineering]] — Engineering context for AI agent consumption
- [[agent-skill-specification]] — Open standard for agent skill packaging
- [[claude-code]] — Agent runtime that can leverage DESIGN.md files
