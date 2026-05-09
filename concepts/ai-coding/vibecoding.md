---
title: Vibe Coding
created: 2026-05-05
updated: 2026-05-09
type: concept
tags:
  - ai-coding
  - practice
sources:
  - raw/articles/2026-05-05-karpathy-agentic-engineering.md
  - raw/articles/2026-05-05-vibecoding-to-agentic-engineering.md
  - raw/articles/2026-05-07-aicoding-guide.md
  - raw/articles/2026-05-08-veteran-developer-agent-journey.md
  - raw/articles/2026-05-09-agent-era-productivity-paradox.md
  - raw/articles/2026-05-09-claude-code-html-effectiveness.md
---

# Vibe Coding

**Vibe Coding** 是 [[karpathy]] 于 2025 年创造的术语，指"凭感觉让 AI 写代码"的编程方式——开发者不需要完全理解生成的代码，而是通过自然语言描述意图，结合迭代反馈来完成开发。

## 定义

核心特征：
- 用自然语言表达开发意图，而非手动编写每一行代码
- 依赖 AI 生成的代码，信任模型输出
- 通过快速迭代和视觉/运行反馈来修正结果
- 不要求开发者理解代码的每个细节

## 2026 年重新定义

Karpathy 在 2026 年 Sequoia 访谈中重新定位了 Vibe Coding 的角色：

> Vibe Coding 是**抬高地板**——降低开发门槛，让非程序员也能构建应用。
> [[agentic-engineering]] 是**抬高天花板**——保证质量、安全与可靠性。

这一区分明确了 Vibe Coding 的适用边界：它擅长快速原型和非关键应用，但无法处理需要严格规格和验证的场景。

## 与 Agentic Engineering 的关系

| 维度 | Vibe Coding | Agentic Engineering |
|------|-------------|---------------------|
| 目标 | 快速出活，降低门槛 | 保证质量与安全 |
| 用户 | 非程序员、快速原型 | 专业工程师、生产系统 |
| 验证 | 人工目视、运行测试 | 自动化规格验证 |
| 典型工具 | [[claude-code]]、[[codex]] 的即兴模式 | Harness + Spec 驱动的工作流 |

## 局限性

- 生成的代码可能包含隐蔽错误，开发者难以审查
- 缺乏系统性测试和安全保障
- 大型项目容易出现 [[context-rot]] 和维护困难
- 不适合对安全性、可靠性有严格要求的生产系统

## Vibe Coding 翻车实录

[[zhiyuanfu]] 在"24h 打工人"项目初期尝试 Vibe Coding，记录了典型的时间线：^[raw/articles/2026-05-08-veteran-developer-agent-journey.md]

- **Day 1-3**：✨ 几句话出完整页面，产出速度惊人，成就感爆棚
- **Day 7**：⚠️ AI 对功能的实现越来越差，陷入"打地鼠"——修了这个 bug 冒出那个
- **Day 14**：🔥 大量过度设计、冗余逻辑，三层抽象解决一个本该一个函数搞定的问题
- **Day 15**：🔧 整整一天"设计与实现对齐"，逐个重构，比前两周加起来都累但也比前两周加起来都有价值

本质问题：**Vibe Coding 是"先易后难"，[[spec-driven-development|SDD]] 是"先难后易"。** 前期省掉的设计时间，后期会以 10 倍的 debug 时间还回来。Vibe Coding 翻车的真正价值在于迫使建立 SDD 流程、设计文档和架构约束——没有 Day 15 的阵痛，就没有后续的系统化运行。

## Vibe Coding vs Spec Coding 详解

AI 编程实践中存在两种截然不同的方法论，可以形象地比喻为：^[raw/articles/2026-05-07-aicoding-guide.md]

- **Vibe Coding = 先射箭再画靶**：先让 AI 跑起来，看效果再调整。适合探索性、创意性任务，快速验证想法。
- **Spec Coding = 先画靶再射箭**：先定义清楚规格和约束，再让 AI 基于规格生成。适合工程性、生产级任务，确保可靠性和可维护性。

### 何时使用 Vibe Coding

- 快速原型验证（MVP 阶段）
- 创意探索和头脑风暴
- 非关键功能的快速实现
- 个人项目和学习实验
- 对可维护性要求不高的场景

### 何时使用 Spec Coding

- 生产级代码开发
- 团队协作项目（需要统一规范）
- 安全敏感场景
- 长期维护的项目
- 复杂业务逻辑实现

### 实践建议

在实际工作中，两种模式往往交替使用：早期用 Vibe Coding 快速验证方向，确认方案后切换到 Spec Coding 固化成果。关键判断标准是**「这段代码会活多久」**——活不过一周的用 Vibe，要长期维护的用 Spec。详见 [[spec-driven-development]] 和 [[agentic-engineering]]。

## 相关概念

- [[agentic-engineering]] — Vibe Coding 的"天花板"互补
- [[spec-driven-development]] — 为 Vibe Coding 引入规格约束
- [[claude-code]] — 典型的 Vibe Coding 工具
- [[codex]] — OpenAI 的编程 Agent
- [[harness-engineering-deep-dive]] — 构建安全执行环境

## Vibe Coding 与生产力悖论

向邦宇在 Agent 时代生产力分析中指出，Vibe Coding 存在结构性局限——仅引入 AI 工具而不改变组织形态会导致 [[productivity-paradox|生产力悖论]]。Vibe Coding 让个人开发者产出大幅提升，但当这些产出需要通过传统协作流程（代码审查、PR 合并、人工测试）时，组织瓶颈反而拉低了整体效率。^[raw/articles/2026-05-09-agent-era-productivity-paradox.md]

这意味着 Vibe Coding 的价值上限取决于组织形态的匹配程度——在 [[opc-one-person-company|OPC]] 场景下效果最佳，在大型组织中需要配合 [[agentic-engineering]] 和 [[ai-native-organization]] 的系统性变革。

## HTML 替代 Markdown 作为输出格式

Vibe Coding 的输出格式正在从 Markdown 向 HTML 演进。Claude Code 的实践表明，[[html-agent-output-format|HTML 作为 Agent 输出格式]] 在视觉保真度、交互性和前端代码直接预览方面显著优于 Markdown，尤其在 Vibe Coding 的典型场景——快速生成 UI 组件和完整页面时表现突出。^[raw/articles/2026-05-09-claude-code-html-effectiveness.md]

这一趋势将进一步降低 Vibe Coding 的反馈延迟——开发者不再需要"想象 Markdown 渲染后的效果"，而是直接看到最终的视觉呈现。
