---
title: 规范驱动开发（SDD）
created: 2026-04-25
updated: 2026-05-08
type: concept
tags: [ai-coding, comparison]
sources:
  - raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md
  - raw/articles/2026-04-24_很多人只是把AI当代码助手，但它其实已经能主导架构设计了.md
  - raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md
  - raw/articles/2026-04-27-ai-programming-tipping-point.md
  - raw/articles/2026-05-07-aicoding-guide.md
  - raw/articles/2026-05-08-veteran-developer-agent-journey.md
  - raw/articles/2026-05-08-agent-eval-ai-coding-310k-lines.md
---

# 规范驱动开发（Spec-Driven Development, SDD）

规范驱动开发是一种 AI 编程方法论：在写第一行代码之前，先把"做什么、为什么做、怎么做、不做什么"说清楚，再让 AI 基于清晰规范生成代码。其核心主张是：AI 编程的瓶颈不是 prompt 写得不好，而是缺少结构化工作流。^[raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md]

## Vibe Coding 瓶颈

AI 编程工具的典型问题演进：

1. **第一阶段**：用 Copilot 补全代码
2. **第二阶段**：用 Cursor / Claude Code 做复杂功能
3. **第三阶段**：超过三个文件的项目中，AI 越跑越偏——上下文失忆、前后逻辑矛盾、花在修正上的时间比直接写还多

本质原因：让一个"能力极强但极度字面意思"的 AI 在没有图纸的情况下盖房子。

## SDD 分层架构

SDD 生态可分为三个层次：

```
spec-first（理念层，见下文）
       ↓
Spec-Kit / OpenSpec（规范管理层）
       ↓
Superpowers（执行纪律层）
```

- **理念层**：spec-first — "先画图纸，再动砖头"的底层哲学
- **规范管理层**：[[spec-kit]]（全新项目的宪法级规范）/ [[openspec]]（存量项目的增量规范）
- **执行纪律层**：[[superpowers]] — 强制 AI 按工程最佳实践干活

## spec-first 核心四要素

不用任何工具也能实践 spec-first 理念，只需在项目根目录维护 `AGENTS.md` 文件，写清：

1. **What（做什么）** — 功能描述
2. **Why（为什么做）** — 动机与背景
3. **Constraints（约束）** — 技术限制与质量要求
4. **Non-Goals（非目标）** — 明确告诉 AI 不要做什么（最有价值的部分）

"非目标"一栏尤为重要——AI 跑偏往往是因为太"积极"，总想实现没说到的边角案例。

## 选型决策树

| 情况 | 推荐 |
|------|------|
| 全新项目 + 团队 ≥3 人 + 流程正规 | [[spec-kit]] |
| 已有代码库 + 加新功能/局部重构 | [[openspec]] |
| 主要用 Claude Code + 提升代码质量 | [[superpowers]] |
| 先感受 SDD，不想装工具 | spec-first（AGENTS.md） |
| 最强组合 | OpenSpec + Superpowers（黄金搭档） |

## 黄金搭档：OpenSpec + Superpowers

社区最推荐的组合工作流：

1. **OpenSpec 锁定需求**：`openspec init` → `/opsx:new` → `/opsx:ff` 自动生成文档 → 手动补充"非目标"
2. **Superpowers 执行开发**：`/superpowers:brainstorm` → `/superpowers:write-plan` → `/superpowers:execute-plan`
3. **归档闭环**：`/opsx:verify` 检查规范 → `/opsx:archive` 合并回主规范库

## 实践要点

- **规划会话与执行会话分开**：长对话中前面的约束会被后面的 prompt 覆盖，通过文件传递上下文
- **工具匹配项目**：工具越重门槛越高，复杂项目回报高，简单项目成本超过收益
- **核心能力是抽象能力**：需求抽象、约束定义、任务拆解——工具只是将这些能力固化成流程

## AI 在架构设计中的角色延伸

SDD 理念不只适用于编码阶段，也可前移到架构设计阶段。[[ai-architecture-design]] 实践表明，AI 擅长架构设计前期的信息收集、模式归纳、方案比较和约束澄清——这些正是 SDD 中"先画图纸再动砖头"理念在更上层的表现。具体做法是让 AI 先发散 2-3 套候选方案、补充开源实现和行业最佳实践，再围绕成本、复杂度、可维护性等维度收敛。^[raw/articles/2026-04-24_很多人只是把AI当代码助手，但它其实已经能主导架构设计了.md]

## SDD 的契约本质

2025 年下半年 SDD 成为 AI 编程方法论的显学：GitHub [[spec-kit]] 迅速获得 72,000+ stars，AWS 围绕该理念推出 IDE Kiro，学术界（arXiv 2602.00180）正式形式化了 spec-first / spec-anchored / spec-as-source 三级规范体系。^[raw/articles/2026-04-27-ai-programming-tipping-point.md]

SDD 的核心转变不是「先写 spec 再写代码」，而是让 spec 成为 AI 和人之间唯一共享的、可验证的契约：

- **共享的**：AI 和人必须看到同一份权威文档，避免 AI 的「幻觉 spec」
- **可验证的**：出事后有 artifact 可对照，而不是翻聊天记录
- **契约**：违反 spec 的 PR 应该被自动拒绝，而不是靠人眼发现

对独立开发者来说，SDD 的真正价值是它**强制你在每次 AI 介入之前把问题想清楚**——这恰恰是对抗 [[comprehension-debt]]（理解债）最有效的手段。

需警惕的两种误用：一是「AI 写 spec，AI 写代码」，spec 变成另一份没人读懂的产物，理解债照样累积；二是过度 spec 化，扼杀快速试错的价值。^[raw/articles/2026-04-27-ai-programming-tipping-point.md]

SDD 的正确使用区间：从 MVP 走向生产、从一个人走向小团队、从稳定栈引入新栈——这些「复杂度即将跨越临界点」的节点。

## Spec Coding = 给 AI 固定骨架

从实践角度看，Spec Coding 的本质是**给 AI 提供一个固定骨架**，让它在骨架内填充细节，而不是从零开始自由发挥。^[raw/articles/2026-05-07-aicoding-guide.md]

### 四步实操工作流

1. **定义骨架**：在编码前用 [[agents-md]] 或 spec 文件定义项目结构、接口契约、数据模型和非目标约束
2. **骨架验证**：让 AI 先基于骨架生成方案评审，确认理解无误后再开始编码
3. **骨架内执行**：所有代码生成严格限制在已定义的骨架范围内，超出骨架的需求先更新骨架再实现
4. **骨架回归检查**：每次功能完成后验证是否偏离骨架，偏离则回滚或更新骨架

### 与 AGENTS.md 的关系

[[agents-md]] 是 Spec Coding 理念的轻量级落地形式。通过在项目根目录维护一份结构化的 Agent 配置文件，开发者不需要引入额外工具就能实践 Spec Coding——AGENTS.md 本身就是一份活的 spec，随项目演进持续更新。^[raw/articles/2026-05-07-aicoding-guide.md]

### 核心原则

Spec Coding 不是消灭创造力，而是**把创造力限定在正确的范围内**。正如 [[vibecoding]] 是"先射箭再画靶"，Spec Coding 是"先画靶再射箭"——两种模式在不同场景下各有优势，但生产环境需要后者来保证可靠性。

## SDD 在 Agent 调度系统中的实践

[[zhiyuanfu]] 在"24h 打工人"Agent 调度系统中将 SDD 推向更深层应用——不只是约束代码生成，而是约束 Agent 的每一步执行。每个需求处理完会留下一组文档：^[raw/articles/2026-05-08-veteran-developer-agent-journey.md]

- `spec.md`：把模糊需求变成明确目标和验收标准
- `plan.md`：把问题变成技术方案
- `tasks.md`：把方案拆成可执行步骤，每步有输入、输出和完成标准

这套流程的更深层意义是**让系统可进化**：如果没有留痕记录（Prompt、状态、输出、错误），就无法回答"它当时看到了什么输入？为什么做出这个判断？Prompt 在哪个环节失效了？"等问题，系统就只能不断"重来一次"。有了记录，系统才可能"学会下一次做得更好"。

配合 `constitution.md`（架构约束文件，定义目录结构、模块边界、命名规则），SDD 流程让 Agent 实现了自举——系统曾自行修复自身的 bug。这验证了一个核心原则：**留痕不是为了 debug，而是为了进化。**

### 决策层级

zhiyuanfu 还提出了 SDD 的决策层级——**能在下层解决的，绝不上推**：

| 层级 | 适用场景 | 不确定性 |
|------|---------|---------|
| 目标层 | 想清楚到底要解决什么 | 最低 |
| 代码层 | 确定性逻辑（if/else、正则、模板） | 低 |
| CLI 层 | 组合现有工具（grep + jq + curl） | 中低 |
| Prompt 层 | 需要语义理解和判断 | 中高 |
| Agent 层 | 多步推理、动态决策、循环执行 | 最高 |

每往上一层，不确定性和成本增加一个量级。
