---
title: KroWork
created: 2026-05-09
updated: 2026-05-09
type: entity
tags: [tool, agent-framework, china-ecosystem]
sources: [raw/articles/2026-05-09-kuaishou-krowork-agent.md]
---

# KroWork

快手推出的打工人Agent产品。核心理念为"跑通一次，变成应用"。

## 核心机制

用户通过自然语言描述需求，生成完整桌面应用（含界面、后端、数据源）。应用固化后像普通软件一样通过桌面快捷方式反复使用，后续运行完全不消耗token。^[raw/articles/2026-05-09-kuaishou-krowork-agent.md]

## 解决的痛点

1. **提示词疲劳** — 不必每次重新编写复杂提示词
2. **成功率赌博** — 一次跑通后固化为确定性应用
3. **重复token消耗** — 固化后运行零token消耗
4. **数据出域安全** — 数据全部本地处理，在安全沙箱中执行

## 关键特性

- 支持 browser-use 能力，可操控浏览器
- 支持自然语言迭代改进已有应用
- 未来将支持应用一键分享给同事

PocketOS删库事件被引用为反面案例，凸显Agent权限控制与数据安全的重要性。^[raw/articles/2026-05-09-kuaishou-krowork-agent.md]

## 相关

- [[ai-employee]] — AI员工赛道
- Agent 安全 — Agent安全与沙箱机制
- [[token-roi]] — token投资回报率
- [[agent-first-design]] — Agent优先设计理念
