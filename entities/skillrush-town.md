---
title: Skillrush Town（淘金小镇）
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [tool, open-source, agent-framework]
sources:
  - raw/articles/2026-05-06-skillrush-town.md
---

# Skillrush Town（淘金小镇）

**类型**: 开源 Skill — 排行榜数据自动挖掘工具
**作者**: 卡尔（AI沃茨）
**GitHub**: [github.com/LearnPrompt/skillrush-town](https://github.com/LearnPrompt/skillrush-town)
**适用框架**: [[claude-code]] 等支持 Skill 的 AI Agent 工具

## 概述

Skillrush Town 是一个开源 Skill，能够自动挖掘各类排行榜中隐藏的信息差。它每天定时抓取 ClawHub 下载榜 Top100 等数据源，生成对比报告并部署到 GitHub Pages。核心理念是将每天变化的公开信息转化为可供 Agent 复盘分析的情报网络，帮助用户从海量 Skill 中筛选出值得关注的高价值工具。^[raw/articles/2026-05-06-skillrush-town.md]

## 核心功能

### ClawHub 排行榜抓取

每日自动抓取 ClawHub 下载榜 Top100 数据，生成对比报告。通过连续运行积累时间线数据，追踪排名变化、新上榜 Skill、以及下载量和星标的增长趋势。^[raw/articles/2026-05-06-skillrush-town.md]

### 多数据源扩展

除 ClawHub 外，还能监控 Claude Code 更新日志、AA Index 模型排行榜等动态数据源，以最少环境依赖稳定运行多次数据分析。^[raw/articles/2026-05-06-skillrush-town.md]

### GitHub Pages 部署

自动将生成的对比报告部署到 GitHub Pages 页面，提供可视化查看。不过作者强调网页展示只是最不重要的一层，核心价值在于数据积累与 Agent 分析。^[raw/articles/2026-05-06-skillrush-town.md]

### 智能筛选规则

内置多条筛选规则自动发现值得关注的 Skill：
- 前 100 名中排名提升 8 名以上
- 下载量增长排进前 20
- 下载和星标均提升 10 名以上

筛选出的 Skill 会与用户已有 Skill 体系做差距对比，判断是否值得花时间适应。^[raw/articles/2026-05-06-skillrush-town.md]

## 技术方案

### 直接调用 Convex 数据库 API

作者最初使用浏览器自动化（Puppeteer 等）方案，但失败率极高（一周三天失败）。后来通过让 Codex 循环探索，发现 ClawHub 页面背后使用 Convex 云端数据库，前端通过 `skills:listPublicPageV4` 查询路径获取数据。^[raw/articles/2026-05-06-skillrush-town.md]

关键参数：
- `sort=downloads` — 按下载量排序
- `dir=desc` — 从高到低
- `nonSuspiciousOnly=true` — 过滤可疑 Skill
- `numItems=25` — 每页 25 条

通过 `nextCursor` 分页标记连续请求 4 次，即可获取 Top100 完整数据。这种方案复刻了 ClawHub 前端自身的数据获取方式，依赖极少且稳定。^[raw/articles/2026-05-06-skillrush-town.md]

## 应用场景

- **Skill 生态监控**：在类似早期 Chrome 插件市场的 [[agent-skill-ecosystem|Agent Skill 生态]] 中，自动追踪新上架和排名变动的 Skill
- **信息差挖掘**：发现信息流中容易错过的高质量 Skill（如预测市场 Polymarket Skill、上下文管理 GBrain 等）
- **决策辅助**：对比新上榜 Skill 与已安装 Skill 的功能差异，辅助升级决策
- **趋势分析**：通过同类能力排名变动，观察社区偏好变化

## 参见

- [[agent-skill-ecosystem]] — Agent Skill 生态全景
- [[skill-evaluator]] — Skill 质量量化评估工具
