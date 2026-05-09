---
title: 白家杰 (Bai Jiajie)
created: 2026-04-29
updated: 2026-04-29
type: entity
tags: [coordination-engineering, person, tool]
sources:
  - raw/articles/2026-04-29_HarnessEngineering实践心得：如何高效驾驭AI？.md
---

# 白家杰 (Bai Jiajie)

白家杰是一名游戏客户端开发工程师，日常工作在 Unity 引擎开发。他是 **Harness Engineering** 的个人实践者，通过持续的 AI 辅助开发实践，独立交付了 WPF 桌面启动器、内部 Web 站点和企业微信机器人等多个项目。

他的核心价值在于：从一个纯客户端出身的开发者出发，完整经历了从 Prompt 时代 → Context 时代 → Harness 时代的 AI 编程范式跃迁，并将个人经验系统化为 JK Launcher 项目上的可复现工程实践。

## 相关项目

### JK Launcher

JK Launcher 是白家杰的 Harness Engineering 试验田——一个 Unity 项目管理工具（WPF .NET Framework 4.8），从 V3.0 迭代到 V3.11，见证了从 Prompt 到 Context 再到 Harness 的完整演化。详见 [[harness-engineering-cases-and-evolution]] 中的 JK Launcher 案例。

## 核心贡献

1. **个人视角的 Harness 演化实证** — 用真实项目数据记录了从"问一句答一句"到"搭建自动化环境"的完整跃迁
2. **Rules / Skills / Scripts 三层分离架构** — 将 14 条 always-applied 规则从 1010 行压缩到 275 行，核心思路是"能用机器查的就别靠 AI 记"
3. **多 Agent 协作流水线** — 7 个专业化 Agent（PM Orchestrator / Requirement Analyst / Solution Architect / Gate Reviewer / Developer Agent / Code Reviewer / QA Tester），通过文档化交接实现流水线式研发
4. **问题五分类法** — 将 AI 编程中的问题分为"编码规范违反、操作流程遗漏、角色交互混乱、外部能力缺失、系统性流程缺失"五类，每类对应不同的 Harness 改进手段
5. **事后验证体系** — verify_all.ps1 实现 14 项自动化检查，配合 verification_baseline.json 基线管理，确保规则被执行而非仅被"告知"

## 关键认知

- **"AI 编程的瓶颈从来不在 AI 有多聪明，而在我有没有给它搭好发挥的舞台"**
- **"Harness 是一种高度压缩的知识传递"** — 十年工程师经验拆成 Rule/Skill/Script，AI 能在秒级吸收
- 人的角色从"写代码"转向"设计 Agent 的职责边界和输出规范"——本质是一种元工程

## 关联

- [[harness-engineering-cases-and-evolution]] — JK Launcher 作为个人实践案例
- [[prompt-context-harness-engineering]] — 三维框架在个人项目中的完整应用
- [[harness-engineering-deep-dive]] — Harness 七层模型的 JK Launcher 实现
