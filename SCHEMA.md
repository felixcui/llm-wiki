# Wiki Schema

## Domain
AI/ML 前沿资讯与技术知识库 — 覆盖大模型、AI 工具产品、Agent 系统、AI Coding、公司/人物动态及技术原理。

## Conventions
- File names: lowercase, hyphens, no spaces (e.g., `claude-code.md`)
- Every wiki page starts with YAML frontmatter (see below)
- Use `[[wikilinks]]` to link between pages (minimum 2 outbound links per page)
- When updating a page, always bump the `updated` date
- Every new page must be added to `index.md` under the correct section
- Every action must be appended to `log.md`
- **Provenance markers:** On pages that synthesize 3+ sources, append `^[raw/articles/source-file.md]`
  at the end of paragraphs whose claims come from a specific source.

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
# Optional quality signals:
confidence: high | medium | low
contested: true
contradictions: [other-page-slug]
---
```

### raw/ Frontmatter
```yaml
---
source_url: https://example.com/article
ingested: YYYY-MM-DD
sha256: <hex digest of the raw content below the frontmatter>
---
```

## Tag Taxonomy

### 模型 (Models)
- `model` — 通用模型标签
- `architecture` — 模型架构（Transformer、MoE 等）
- `benchmark` — 评测基准
- `training` — 训练方法（预训练、微调、RLHF）

### 产品/工具 (Products & Tools)
- `tool` — AI 工具/产品
- `agent-framework` — Agent 框架（Claude Code、Hermes 等）
- `ai-coding` — AI 编程工具
- `media-generation` — 内容生成（PPT、视频、音频）

### 组织/人物 (People & Orgs)
- `person` — 人物
- `company` — 公司/组织
- `organization` — 组织形态/方法论
- `lab` — 研究机构
- `open-source` — 开源项目

### 技术概念 (Techniques)
- `optimization` — 优化技术
- `fine-tuning` — 微调方法
- `inference` — 推理优化
- `alignment` — 对齐/安全
- `data` — 数据相关
- `reinforcement-learning` — 强化学习
- `computer-vision` — 计算机视觉

### 元信息 (Meta)
- `comparison` — 对比分析
- `timeline` — 时间线
- `controversy` — 争议话题
- `prediction` — 预测/展望
- `coordination-engineering` — 协调工程
- `practice` — 实践案例与最佳实践

### 行业/生态 (Industry & Ecosystem)
- `consumer` — 消费者应用
- `china-ecosystem` — 中国生态
- `ai-workforce` — AI 劳动力平台

### 设计/产品 (Design & Product)
- `product-design` — 产品设计

### 技术概念补充 (Additional Technical Concepts)
- `memory` — 记忆系统

Rule: every tag on a page must appear in this taxonomy. If a new tag is needed,
add it here first, then use it.

## Directory Structure
```
wiki/
├── SCHEMA.md
├── index.md
├── log.md
├── raw/                     # Layer 1: Immutable source material
│   ├── articles/            # Web articles, clippings
│   ├── papers/              # PDFs, arxiv papers
│   ├── transcripts/         # Meeting notes, interviews
│   └── assets/              # Images, diagrams
├── entities/                # Layer 2: People, companies, products, models
├── concepts/                # Layer 2: Technical concepts and topics
│   ├── ai-models/           #   大模型相关概念
│   ├── agent/               #   Agent 相关概念
│   ├── tools/               #   工具/产品相关概念
│   ├── ai-coding/           #   AI Coding 相关概念
│   └── principles/          #   技术原理
├── comparisons/             # Layer 2: Side-by-side analyses
├── queries/                 # Layer 2: Filed query results
└── _archive/                # Archived/superseded pages
```

## Page Thresholds
- **Create a page** when an entity/concept appears in 2+ sources OR is central to one source
- **Add to existing page** when a source mentions something already covered
- **DON'T create a page** for passing mentions, minor details, or things outside the domain
- **Split a page** when it exceeds ~200 lines — break into sub-topics with cross-links
- **Archive a page** when its content is fully superseded — move to `_archive/`, remove from index

## Update Policy
When new information conflicts with existing content:
1. Check the dates — newer sources generally supersede older ones
2. If genuinely contradictory, note both positions with dates and sources
3. Mark the contradiction in frontmatter: `contradictions: [page-name]`
4. Flag for user review in the lint report
