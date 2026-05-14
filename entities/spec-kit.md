---
title: Spec-Kit
created: 2026-04-25
updated: 2026-05-14
type: entity
tags: [tool, ai-coding, open-source]
sources:
  - raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md
  - raw/articles/2026-04-22_AI编程三剑客：Spec-Kit、OpenSpec、Superpowers深度对比与实战指南.md
  - raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md
---

# Spec-Kit

Spec-Kit 是 GitHub 官方在 2025 年推出的开源工具包，专为 AI 辅助编程设计，核心理念是"规范先行"（Spec-First）——先精确定义做什么和为什么，再由 AI 基于清晰的规范生成代码。^[raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md]

## 五阶段工作流

Spec-Kit 采用严格顺序的五阶段工作流：

1. **Constitution（宪法）** — 定义项目最高原则：技术栈限制、代码风格、测试覆盖率要求、安全策略等，后续所有 AI 行为以此为约束
2. **Specify（功能规范）** — 精确定义功能需求
3. **Plan（技术方案）** — 制定技术实现方案
4. **Tasks（任务拆解）** — 将方案拆解为可执行任务
5. **Implement（代码实现）** — AI 基于规范生成代码

其中 Constitution 是最独特的设计，相当于项目的"宪法"，一旦写入，AI 不可逾越。

## 适用场景

- 复杂的新建项目（3-5 个模块以上）
- 对合规性有要求的企业场景
- 团队协作中需要统一标准的情况

## 局限性

- 依赖 Python 环境和 uv 包管理器
- 前期文档工作量大（"一片 Markdown 的海洋"）
- 不适合快速迭代的小需求或已有代码库的局部改动
- 严格阶段门控在多线并行、随时变更的企业场景下可能成为阻碍

## 与其他工具的关系

Spec-Kit 属于 [[spec-driven-development]] 的规范管理层。与 [[openspec]] 相比更重、更正式；与 [[superpowers]] 互补——Spec-Kit 管"做什么"，Superpowers 管"怎么做好"。适合与 [[claude-code]] 配合使用。

Spec-Kit 的 Constitution 机制与 [[claude-code-system-prompt]] 中"静态配置层"的设计理念相通——都通过顶层约束文件来塑造 AI 的行为边界。其严格阶段门控也反映了 [[prompt-context-harness-engineering]] 中对结构化流程的重视。

类似 Spec-Kit 的 AI 编程工作流工具还有 [[cursor-team-kit]]，两者都致力于优化 AI 辅助编码的协作流程。

## 上手命令

```bash
pip install uv
git clone https://github.com/github/spec-kit.git
# 在 Claude Code 中依次执行：
/speckit.constitution
/speckit.specify "用户登录功能，邮箱密码验证，返回JWT"
/speckit.plan
/speckit.tasks
/speckit.implement
```
