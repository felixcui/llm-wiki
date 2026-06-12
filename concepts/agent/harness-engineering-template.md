---
title: Harness Engineering Template
created: 2026-05-15
updated: 2026-05-15
type: concept
tags: [practice, ai-coding]
sources:
  - raw/articles/2026-05-14_别让AI瞎猜了：用HarnessEngineering终结无限返工.md
---

# Harness Engineering Template

基于 [[harness-engineering-deep-dive|Harness Engineering]] 理念的项目模板（GitHub: SisyphusSQ/harness-template），为 AI Agent 参与研发提供一套最小可用的工程安排。

## 核心原则

> Prompt 是台词，Agent 是演员，工具是道具，模型是大脑，Harness 是整个舞台的导演、灯光、调度系统。

从 Prompt Engineering 到 Harness Engineering 的转变：

| 问题 | Prompt Engineering | Harness Engineering |
|------|-------------------|-------------------|
| 背景怎么说清楚 | 写进 prompt | 写进项目入口和 docs |
| 约束怎么提醒模型 | 加提示词 | 写进 plan、规则、gate |
| 验证怎么做 | 提醒跑测试 | 提供可执行验证入口 |
| 经验怎么复用 | 复制旧 prompt | 写回仓库/任务系统 |

## 目录结构

```
项目/
├── AGENTS.md                    # 入口地图
├── docs/harness/
│   ├── control-plane.md         # 任务流转控制面
│   └── project-constraints.md   # 项目约束登记
├── .agent/
│   ├── PLANS.md                 # 计划协议
│   ├── plans/TEMPLATE.md        # 计划模板
│   ├── prompts/                 # 标准 prompt
│   └── guides/                  # 维护循环/review 口径
├── docs/test/                   # runbook/验证摘要
└── scripts/harness/             # 检查入口脚本
```

## 五类核心组件

| 类别 | 作用 |
|------|------|
| 任务约束与规则 | 目标、范围、非目标、验收口径 |
| 工具执行与运行入口 | Makefile、脚本、测试命令 |
| 上下文和计划工件 | AGENTS.md、docs、plan |
| 权限控制与失败恢复 | 停止条件、回滚策略 |
| 验证、评审与结果记录 | runbook、review gate、PR/MR |

## 前端三层模型

1. **执行依据层**：Pencil / 设计结构 —— 固定页面目录和关键状态
2. **状态暴露层**：Storybook —— 显示各种状态的可运行环境
3. **交付实现层**：真实页面接入路由、权限、接口

## 后端三层模型

1. **执行依据层**：docs / plan —— 运行模式、输入输出、异常口径
2. **状态暴露层**：验证脚本 / mock 环境 —— 什么条件算成功/失败
3. **交付实现层**：实现、测试、联调、收口

与 [[agents-md|AGENTS.md]] 标准和 [[superpowers|Superpowers]] 工作流互补。
