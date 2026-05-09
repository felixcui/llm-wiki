---
title: Agent Cluster（Agent 集群）
created: 2026-04-22
updated: 2026-04-22
type: concept
tags: [model, benchmark, coordination-engineering]
sources:
  - raw/articles/2026-04-22_KimiK2.6开源了！还附送了300个Agent员工？.md
---

# Agent Cluster（Agent 集群）

单个主 Agent 自动拆解任务，创建大量子 Agent 并行执行的模式。[[kimi-k2.6]] 的 Agent 集群功能是该模式的代表实现，支持最多 300 个子 Agent、4000 步执行，将单 Agent 的串行工作转化为团队级并行交付。^[raw/articles/2026-04-22_KimiK2.6开源了！还附送了300个Agent员工？.md]

## 能力规模

K2.6 相比 K2.5 在 Agent 集群上的跃升：^[raw/articles/2026-04-22_KimiK2.6开源了！还附送了300个Agent员工？.md]

| 维度 | K2.5 | K2.6 |
|------|------|------|
| 子 Agent 上限 | 100 | 300 |
| 步骤上限 | 1500 | 4000 |

一句话输入即可触发集群模式——主 Agent 自动拆解任务、创建子 Agent、分配角色、并行执行、汇总交付。

## 在 SWE-Bench Pro 上的应用

K2.6 在 SWE-Bench Pro 上取得 58.6 分（开源 SOTA），超过 GPT-5.4（xhigh）和 Claude Opus 4.6（max effort）。这是开源模型首次在主流编程基准上取得对闭源模型的压制优势。^[raw/articles/2026-04-22_KimiK2.6开源了！还附送了300个Agent员工？.md]

其他基准成绩：SWE-Bench Verified 80.2、Terminal-Bench 2.0 66.7、LiveCodeBench v6 89.6、BrowseComp 83.2。

## 长任务稳定性

K2.6 在真实场景中的工程能力提升显著：^[raw/articles/2026-04-22_KimiK2.6开源了！还附送了300个Agent员工？.md]

- **连续工作 12 小时不崩**：用 Zig 语言本地部署 Qwen3.5-0.8B，4000+ 次工具调用，14 轮迭代
- **金融引擎重构**：对 exchange-core 做全面重构，13 小时，1000+ 工具调用，修改 4000+ 行代码，吞吐量提升 185%
- **跨语言泛化**：Rust、Go、Python、前端、DevOps 工作流均能稳定输出

CodeBuddy 报告 18% 的长上下文稳定性提升。平均步骤数比 K2.5 减少约 35%，意味着更少的 Token 消耗和更少的出错机会。

## 工具调用成功率

工具调用成功率达 96.6%（CodeBuddy 报告），是长任务稳定性的关键支撑。高成功率意味着 Agent 在数百上千次工具调用中很少因调用失败而中断工作流。^[raw/articles/2026-04-22_KimiK2.6开源了！还附送了300个Agent员工？.md]

## 集群工作流程

实测案例（AI 编程工具市场分析）：^[raw/articles/2026-04-22_KimiK2.6开源了！还附送了300个Agent员工？.md]

1. **制定计划**：将任务拆成 12 个维度（市场格局、竞争格局、各工具深度分析等）
2. **创建子 Agent**：自动创建 12 个有名字、有角色定位的子 Agent
3. **并行研究**：12 个 Agent 同时在浏览器中搜索不同维度资料
4. **交叉验证**：多阶段验证与洞察提取
5. **产物制作**：并行派出子 Agent 分别制作 PDF、Excel、PPT
6. **交付**：约 1 小时完成三套完整文件

## 与单 Agent 模式的对比

| 维度 | 单 Agent | Agent 集群 |
|------|---------|-----------|
| 执行方式 | 串行 | 并行 |
| 任务粒度 | 全部由一个 Agent 完成 | 自动拆解、分工执行 |
| 时间效率 | 线性增长 | 可并行提速 |
| 适用场景 | 简单到中等复杂度 | 大规模复杂任务 |
| Token 成本 | 单次对话成本 | 多 Agent 并行成本较高 |
| 可观测性 | 单一对话流 | 多 Agent 进度面板 |

## 与 [[coordination-engineering]] 的关系

Agent 集群是协调工程的一种具体实现形式。Coordination Engineering 提供理论框架（任务拆解、角色分工、通信机制），Agent 集群在此基础上增加了自动角色创建（给子 Agent 命名和定位）、大规模并行（300 子 Agent）和进度可视化（阶段面板）等产品化能力。
