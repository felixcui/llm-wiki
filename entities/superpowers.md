---
title: Superpowers
created: 2026-04-25
updated: 2026-04-25
type: entity
tags: [tool, ai-coding, open-source]
sources:
  - raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md
  - raw/articles/2026-04-22_AI编程三剑客：Spec-Kit、OpenSpec、Superpowers深度对比与实战指南.md
---

# Superpowers

Superpowers 是 Jesse Vincent（GitHub ID: obra）在 2025 年 10 月开源的项目，进入 Anthropic 官方插件市场后迅速爆发，Star 数突破 15 万。核心理念是 **Process over Prompt（流程大于提示词）**——与其给 AI 写更精准的 prompt，不如给 AI 套上工程纪律。^[raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md]

## 强制工程流程

安装后，AI 处理超过 50 行代码的任务时强制走完整流程：

1. **brainstorming（需求澄清）** — 先问清楚要做什么
2. **git worktrees（创建隔离分支）** — 每个任务独立分支
3. **writing-plans（任务拆解）** — 拆成 2-5 分钟粒度的任务
4. **TDD（测试驱动开发）** — 先写失败测试，再写实现，最后重构
5. **subagent execution（子代理并行执行）** — 多任务并行
6. **code review（双阶段审查）** — 两轮代码审查
7. **finish branch（验收）** — 处理分支合并

### TDD 强制

最突出的是 TDD 强制：AI 必须先写"会失败的测试"，再写实现让测试通过。任何试图跳过的 prompt 都会被拒绝执行并解释原因。

## Token 效率

完整流程比直接写代码多消耗 10-20% Token，但返工减少 60-70%，总账划算。

## 适用场景

- [[claude-code]] 重度用户
- 对代码质量有硬性要求的正式项目
- 想让 AI 产出"能上线"而非"能运行"代码的开发者

## 局限性

- Superpowers 本身不管理规范文档，只管执行时的行为约束
- 最好与 [[openspec]] 或 [[spec-kit]] 配合使用

## 上手命令

```bash
# 在 Claude Code 中执行：
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
# 重启 Claude Code 后：
/superpowers:brainstorm    # 从需求澄清开始
/superpowers:write-plan    # 生成执行计划
/superpowers:execute-plan  # 开始执行
```

中文增强版：`npm install -g superpowers-zh`
