---
title: SkillFactory
created: 2026-05-15
updated: 2026-05-15
type: entity
tags: [tool, agent-framework, practice]
sources:
  - raw/articles/2026-05-15_SkillFactory：三天手搓面向Harness设计的技能工厂（附AIcoding实践）.md
---

# SkillFactory

面向 [[harness-engineering-deep-dive|Harness]] 设计的"技能工厂"，阿里云开发者月珩创建。核心思路是标准化测试驱动 [[agent-skill-specification|Skill]] 生成。

## 解决的问题

现有 Skill 创建方式的两种模式及其局限：

| 模式 | 问题 |
|------|------|
| 人工编写 | 效率低、质量波动大、测试覆盖不足 |
| 对话生成（[[openclaw|OpenClaw]]/[[claude-code|Claude Code]]） | 非确定性、缺乏自动化验证、一次只能生成一版 |

## 核心流程

1. **技能定义**：输入技能需求 + 测试问题 + API 接口
2. **基线诊断**：
   - 裸模型评估：直接让模型执行，作为基线对照
   - Skill 匹配分析：召回相似 Skill，评估现有技能是否满足需求
3. **多路并发生成**：并行调用 3 种不同策略（不同模型/Prompt 模板），相当于一次买三张彩票
4. **回归迭代**：格式规范/复用创新/功能可用性/运行稳定性/文档规范五维评价
5. **质量检查**：自动化闭环验证

## 创新点

- **失败优先**：基线诊断的失败点就是 Skill 需要解决的真实能力缺口
- **多路并行竞优**：同时生成 3 种策略版本，择优录用
- **生态适配**：支持知流平台的 [[mcp-model-context-protocol|MCP]]、HTTP、Dify Agent 工具直接生产技能

## 与 Trace2Skill 的关系

未来方向参考千问团队的 Trace2Skill：将 Agent 执行轨迹（隐性经验）转化为结构化技能文档（显性知识），仅通过小模型轨迹分析提炼通用的专家级能力。与 [[skill-design-patterns|Skill 设计模式]] 和 [[agent-skill-evaluation|Skill 评估]] 框架互补。
