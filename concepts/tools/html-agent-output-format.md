---
title: HTML作为Agent输出格式
created: 2026-05-09
updated: 2026-05-09
type: concept
tags: [tool, ai-coding, product-design]
sources: [raw/articles/2026-05-09-claude-code-html-effectiveness.md, raw/articles/2026-05-09-claude-code-html-effectiveness-cn.md]
---

# HTML作为Agent输出格式

[[thariq]] 提出的核心理念：在AI Agent时代应使用HTML替代Markdown作为输出格式。^[raw/articles/2026-05-09-claude-code-html-effectiveness.md]

## 设计哲学

- **Markdown** 假设"人会从头读到尾"
- **HTML** 假设"人只想扫重点和动手改"

## HTML的优势

1. **信息密度** — 支持表格、SVG、代码片段等丰富排版
2. **可视化** — 选项卡、移动端自适应等交互能力
3. **分享便捷** — 浏览器直接打开，无需渲染工具
4. **双向交互** — 滑块、按钮调参后一键复制回 [[claude-code]]

## Token成本考量

在 [[opus-4.7]] 的100万token窗口下，HTML多花的token可忽略不计，但其带来的交互体验提升是质的飞跃。^[raw/articles/2026-05-09-claude-code-html-effectiveness.md]

## 传播

原文 "The Unreasonable Effectiveness of HTML" 被 @dotey（宝玉）翻译为中文推广。^[raw/articles/2026-05-09-claude-code-html-effectiveness-cn.md]

## 相关

- [[claude-code]] — Claude Code工具
- [[claude-code-capabilities]] — Claude Code能力体系
- [[vibecoding]] — 感性编程
- [[opus-4.7]] — Anthropic旗舰模型
