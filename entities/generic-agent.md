---
title: Generic Agent
created: 2026-04-24
updated: 2026-04-24
type: entity
tags: [tool, agent-framework, open-source]
sources:
  - raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md
---

# Generic Agent

**作者**: lsdefine
**GitHub**: [github.com/lsdefine/GenericAgent](https://github.com/lsdefine/GenericAgent)
**中文教程**: [Datawhale Hello-GA](https://datawhalechina.github.io/hello-generic-agent/)
**类型**: 极简自进化 Agent 框架（开源）

## 概述

Generic Agent（GA）是一个极简、可自我进化的自主 Agent 框架，核心代码约 **3300 行**，仅用 **9 个原子工具**和 **92 行 Agent Loop**，即可赋予任意大模型对本地计算机的系统级控制能力——覆盖浏览器、终端、文件系统、键鼠输入、屏幕视觉，甚至移动设备。^[raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md]

2026年4月登上 GitHub Trending 第一，半个月涨了 **5k star**。Datawhale 同月发布了完整的中文教程「Hello Generic Agent」。

## 核心设计哲学：上下文信息密度最大化

GA 与 [[hermes-agent]]、[[claude-code]] 等框架最大的区别在于设计理念：不追求上下文有多长，只追求**每一个 token 都在为当前决策服务**。

对比效果显著：同样装了 20 个 Skill，对一个最简单的 "Hello" 请求，其他框架起步价约 17,000 token，GA 只需约 2,000 token——**节省近 10 倍**。这不是单纯优化出来的，而是整个设计哲学层面的差异。^[raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md]

## 9 个原子工具

Claude Code 有 53 个工具，80% 的调用集中在 3 个上，剩下 50 个每轮白白吃上下文预算。GA 只保留 9 个原子工具，覆盖文件操作、代码执行、网页交互、记忆管理、人机协作，任务成功率 100%。

9 个工具涵盖：
- **文件操作**：读取、写入、编辑
- **代码执行**：终端命令运行
- **网页交互**：浏览器控制
- **记忆管理**：长期记忆存储与检索
- **人机协作**：需要人类确认时的交互机制^[raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md]

## 92 行 Agent Loop

Agent Loop 核心代码仅 92 行，没有子 Agent 管理器、事件总线、调度守护进程，但子 Agent 分发、看门狗监控、定时任务调度全都能做。这就是"做减法"的极致体现——3300 行代码涌现出一切。^[raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md]

## Token 效率

同一个任务执行 9 轮、全程无人干预的测试中，GA 的 Token 消耗节省了 **89.6%**。三阶进化路径展示了 GA 如何逐步压缩 token 使用，同时保持甚至提升任务完成质量。^[raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md]

## Harness Engineering 实践

GA 的设计本身就是 [[prompt-context-harness-engineering]] 的实践案例。教程第二部分专门解析了 GA 如何通过以下方式实现极简而高效的 Harness：

- **工具极简化**：9 个原子工具覆盖所有场景
- **上下文密度最大化**：每个 token 都服务于当前决策
- **Agent Loop 极简**：92 行代码实现完整循环
- **自进化能力**：从经验中学习并优化自身行为^[raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md]

## Datawhale 中文教程

2026年4月，Datawhale 正式开源「Hello Generic Agent」教程，分为三部分：

1. **Part 1 · 应用指南**（6章）：零门槛上手，覆盖安装到精通的完整路径，无需编程基础
2. **Part 2 · 原理篇**（7章）：深度解析 GA 为什么能用 1/10 资源做到同样甚至更好的效果
3. **Part 3 · 案例篇**（即将上线）：办公、娱乐、挖宝实战案例^[raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md]

## 与其他框架对比

- **vs [[hermes-agent]]**：GA 以极简设计实现 10 倍 Token 节省，但 Hermes 在自进化机制（RL 训练闭环、Skill 动态生成）上更成熟
- **vs [[claude-code]]**：Claude Code 有 53 个工具但使用效率低，GA 仅 9 个工具但信息密度高
- **vs [[openclaw]]**：OpenClaw 偏"受控运行时"，GA 偏"极简高效"

## 适用场景

- 本地计算机自动化控制
- 浏览器操作与网页数据采集
- 微信、飞书等 IM 平台接入
- 移动设备控制
- 低 token 消耗的持续 Agent 任务

## 技术特点

GA 的极简设计并非功能缺失，而是通过"上下文信息密度最大化"实现了以更少资源完成同等甚至更高质量的工作。对于 token 成本敏感或需要长期持续运行的 Agent 场景，GA 是 [[hermes-agent]] 和 [[claude-code]] 之外的有力替代方案。^[raw/articles/2026-04-24_刚刚！GenericAgent中文教程发布！比Hermes省10倍Token.md]
