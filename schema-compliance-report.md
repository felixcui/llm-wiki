# Schema 合规检查报告

**检查时间**: 2026-04-28 22:47
**检查范围**: /Users/felix/wiki/ 中所有 wiki 页面（共 65 页，含 33 entities + 32 concepts）
**检查依据**: /Users/felix/wiki/SCHEMA.md

---

## ✅ 通过项

### YAML Frontmatter
所有 65 个 wiki 页面均包含 YAML frontmatter（`---` 分隔），且具备所有必需字段：`title`, `created`, `updated`, `type`, `tags`, `sources`。

### Tag Taxonomy 合规
所有页面使用的标签均存在于 SCHEMA.md 的 Tag Taxonomy 中，未发现非法标签。

### 日期一致性（updated >= created）
所有页面的 `updated` 日期均大于或等于 `created` 日期，无反向日期问题。

### 出链数量（>= 2 [[wikilink]]）
所有页面均包含至少 2 个 `[[wikilink]]` 出链。

---

## ❌ 不合规项

### 1. 未在 index.md 中注册（违反 SCHEMA.md Convention："Every new page must be added to index.md under the correct section"）

| 页面路径 | 问题 |
|----------|------|
| `concepts/agent/hermes-agent-architecture.md` | 未出现在 index.md 任何位置 |
| `concepts/agent/hermes-agent-self-improving.md` | 未出现在 index.md 任何位置 |

这两页是 Hermes Agent 架构和自进化机制的拆分子页面，已存在于文件系统中但未被加入 index.md。

### 2. raw/ 文件缺少标准 Frontmatter

| 文件路径 | 问题 |
|----------|------|
| `raw/analysis/2026-04-23_analysis.md` | 不符合 raw/ Frontmatter 规范（缺少 `source_url`, `ingested`, `sha256` 字段），也不符合标准 wiki frontmatter。其目录 `analysis/` 未在 SCHEMA.md 目录结构定义中列出。 |

### 3. index.md 页数统计不准确

| 声明 | 实际 | 差异 |
|------|------|------|
| index.md 声称 "Total pages: 63" | 文件系统中实际有 65 个 wiki 页面 | 缺少 2 页（即上述未注册的 2 页） |

---

## 统计数据

| 检查项 | 通过 | 不通过 |
|--------|------|--------|
| YAML Frontmatter | 65/65 | 0 |
| Tag 合规 | 65/65 | 0 |
| 日期一致性 | 65/65 | 0 |
| 出链数量 >= 2 | 65/65 | 0 |
| 在 index.md 中注册 | 63/65 | 2 |
| raw/ 文件合规 | 0/1 | 1 |
