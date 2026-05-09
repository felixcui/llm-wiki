---
title: OpenSpec
created: 2026-04-25
updated: 2026-04-27
type: entity
tags: [tool, ai-coding, open-source]
sources:
  - raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md
  - raw/articles/2026-04-22_AI编程三剑客：Spec-Kit、OpenSpec、Superpowers深度对比与实战指南.md
---

# OpenSpec

OpenSpec 来自 Fission-AI 团队，定位是为已有项目提供低侵入性的规范层。核心设计是 **Delta Specs（增量规范）**——把"当前项目的事实"和"这次的变更提案"明确分开存放，确保规范始终跟着代码走。^[raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md]

## 核心设计：Delta Specs

```
openspec/
├── specs/          # 当前项目的真相（已建立的能力）
└── changes/        # 变更提案（准备改的东西）
    └── add-dark-mode/
        ├── proposal.md   # 为什么改、改什么
        ├── specs/        # 这次涉及的规范变更
        ├── design.md     # 技术方案
        └── tasks.md      # 任务清单
```

每次改需求是一个独立的"变更单"。改完后执行 `archive`，变更合并回主规范库，历史可查。

## 核心价值

- **规范不失真**：通过强制的"归档"步骤，让规范始终与代码同步，解决"文档写完就扔"的问题
- **低侵入性**：不需要重写现有项目，从下一个需求开始渐进式建立规范
- **工具兼容广**：支持 25+ 种 AI 工具，零 API 密钥，完全用本地工具

## 适用场景

- 存量项目维护和迭代
- 需求频繁变更的中小团队
- "已有一堆代码但没有任何规范文档"的个人开发者

## 工作流命令

```bash
npm install -g openspec-cn  # 推荐中文版
cd your-project
openspec init
# 在 Cursor 或 Claude Code 中：
/opsx:new add-dark-mode       # 创建新变更
/opsx:ff                      # 快进，自动生成所有文档
/opsx:apply                   # 执行实现
/opsx:archive                 # 归档，更新主规范库
```

## 与其他工具的关系

OpenSpec 属于 [[spec-driven-development]] 的规范管理层，与 [[spec-kit]] 互补——Spec-Kit 适合全新项目，OpenSpec 适合存量项目。社区推荐与 [[superpowers]] 组合为"黄金搭档"：OpenSpec 负责规划对齐，Superpowers 负责执行纪律。支持 [[claude-code]] 和 Cursor。
 
OpenSpec 的增量规范设计也体现了 [[ai-architecture-design]] 中"渐进式建立规范"的原则，其工具兼容性（25+ AI 工具）与 [[designing-for-future-models]] 的理念一致——不绑定特定模型，让规范层独立于执行层。
