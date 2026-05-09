---
title: Multi-Agent Code Review
created: 2026-04-24
updated: 2026-04-26
type: concept
tags: [agent-framework, ai-coding]
sources:
  - raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md
  - raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md
---

# Multi-Agent Code Review（多 Agent 代码审查）

通过多个 AI Agent 并行独立审查代码，再由独立 Critic 交叉验证结果的代码审查模式。[[claude-code]] 的 `/ultrareview` 是该模式的代表实现，采用 3 Explorer + 1 Critic 架构，在云端沙箱中完成审查。^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]

## 3+1 架构

ultrareview 的核心架构分工精妙：^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]

- **3 个 Explorer Agent**：各自拿到一份代码改动，独立分析。每个 Explorer 拥有独立的 context window，互不干扰，可能分别聚焦安全漏洞、逻辑错误、性能隐患
- **1 个 Critic Agent**：拿到三个 Explorer 的全部产出，逐条检查——能否复现？是否片面？有无遗漏场景？只有经过 Critic 验证的 Bug 才出现在最终报告

## 并行独立审查 + 交叉验证

多 Agent 并行工作的核心优势：

- **信噪比高**：报出来的基本都是真问题，不是代码风格建议
- **覆盖面广**：多个 Agent 并行探索，能发现单次审查容易遗漏的交叉问题
- **不占本地资源**：整个过程在云端跑，用户终端不受影响^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]

## 云端沙箱执行

ultrareview 必须在云端运行的原因：^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]

1. **真正并行**：4 个 Agent 在云端沙箱同时工作，本地串行处理光等待时间就够呛
2. **执行验证**：Critic Agent 在沙箱环境里实际执行代码来复现问题，不只是静态分析
3. **资源需求**：每个 Agent 都需要独立的模型调用和 context window，这也是 [[prompt-context-harness-engineering]] 中 Harness 层需要解决的核心问题

## found / verified / refuted 三级结果

审查结果以三级分类呈现：^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]

- **found**：Explorer 发现的可疑问题
- **verified**：Critic 确认为真实 Bug
- **refuted**：Critic 判定为误报，直接过滤

演示案例中，ultrareview 总共发现 4 个 verified 问题，8 个被 refuted 过滤。典型的 verified 问题包括竞态条件（token 刷新在 destroy() 后触发）、无限重试循环（429 响应无上限）等单次 review 容易漏掉但上线会出事的问题。^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]

## 与传统 /review 的对比

| 维度 | /review | /ultrareview |
|------|---------|-------------|
| 运行位置 | 本地 | 云端沙箱 |
| 审查深度 | 单次扫描 | 多 Agent + 交叉验证 |
| 耗时 | 几秒到几分钟 | 5-10 分钟 |
| 费用 | 算在日常用量 | $5-$20/次 |
| 适合场景 | 写代码时随手查 | 合并前的最后一道关 |

## 与对抗式 CR 的区别

多 Agent Code Review 与对抗式 Code Review（Adversarial CR）的核心区别在于：^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]

- **目标不同**：ultrareview 是协作式审查，Explorer 和 Critic 共同追求找出真实 Bug；对抗式 CR 中两个 Agent 分别扮演"辩护"和"攻击"角色
- **结果处理**：ultrareview 的 Critic 负责验证而非辩论，误报直接过滤而非需要"辩护方"反驳
- **用户体验**：ultrareview 输出精炼的 verified 问题列表，对抗式 CR 可能产生大量需要人工判断的辩论内容

## 对抗式 CR：LEGO 实践

[[tencent-lego]] 在 [[prompt-context-harness-engineering]] 框架内实践了对抗式 CR，使用 cr_claude、cr_codex、cr_gemini 三个 Reviewer 并行独立审查，cr_manager 汇总交叉验证结果。^[raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md]

### 单模型 CR 的三大盲区

| 盲区类型 | 表现 | 根因 |
|---------|------|------|
| 知识盲区 | 对特定框架/模式理解有差异 | 训练数据不同 |
| 注意力盲区 | 500+ 行 diff 后半部分审查质量下降 | 上下文窗口有限 |
| 确认偏差 | 发现一个问题后沿同一方向继续找 | 单模型固有倾向 |

### 对抗式 CR 流程

1. 三个模型（Claude + Codex + Gemini）并行独立审查
2. 汇总问题并交叉验证（被 2+ 模型同时发现 = 高置信度）
3. 对争议问题进行辩论式讨论（同意/反对/维持）
4. 全员无新发现时自动收敛

### 与业界对比

| 维度 | GitHub Copilot CR | OpenAI Codex Review | LEGO 对抗式 CR |
|------|-------------------|--------------------|--------------------|
| 模型数 | 1 | 1-2 | 3（Claude+Codex+Gemini） |
| 执行方式 | 串行单次 | 串行两次 | 并行+交叉迭代 |
| 交互方式 | 静态扫描 | 静态扫描 | 辩论式（同意/反对/维持） |
| 收敛机制 | 无（一次性） | 固定轮数 | 全员无新发现自动收敛 |
| 容错 | 失败则无结果 | 失败则无结果 | 部分模型失败仍可产出 |
| 审查标准 | 通用 | 通用 | 项目定制 P0-P3 + review-patterns |

## 使用方式

```bash
/ultrareview              # 审查当前分支与默认分支的 diff
/ultrareview 1234         # 审查指定 GitHub PR
```

PR 模式下云端沙箱直接从 GitHub 拉取内容，不需要上传本地代码。启动前弹出确认框，显示审查范围和预估费用。审查在后台运行，可通过 `/tasks` 随时查看进度或中途叫停。^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]

## 局限性

- 需要开启 extra usage 才能使用付费 ultrareview
- 需要 Claude.ai 账号登录（API key 不行）
- 不支持 Amazon Bedrock、Google Cloud Vertex AI、Microsoft Foundry 等渠道
- 开启零数据保留（Zero Data Retention）的组织无法使用
- 仓库太大时需改用 PR 模式^[raw/articles/2026-04-24_ClaudeCode推出ultrareview超级审查功能，20美金一次，10分钟干完.md]
