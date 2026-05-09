---
title: Agent Skill Evaluation
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [agent-framework, tool]
sources:
  - raw/articles/2026-04-23_你写的Skill，及格了吗？.md
---

# Agent Skill Evaluation

## 8 维度 Skill 评估框架

百度团队提出的量化评估框架（S/A/B/C/D 五级），从能否被找到、用起来顺不顺、值不值得存在三个阶段衡量 Skill 质量：^[raw/articles/2026-04-23_你写的Skill，及格了吗？.md]

| 阶段 | 维度 | 关注点 |
|------|------|--------|
| 能否被找到 | D1 元数据质量 | description 精准度 + 触发关键词（唯一决定"生死"的维度） |
| 用起来顺不顺 | D2 执行引导 | 端到端流程清晰度与异常处理 |
| 用起来顺不顺 | D4 工作流完整 | 流程覆盖度、分支处理、回退机制 |
| 用起来顺不顺 | D5 输入输出 | 输入输出格式规范与示例 |
| 用起来顺不顺 | D6 资源利用 | 上下文开销、引用加载效率 |
| 值不值得存在 | D3 领域知识密度 | 技能包含的专业知识浓度 |
| 值不值得存在 | D7 写作质量 | 语言清晰度、结构合理性 |
| 值不值得存在 | D8 范围聚焦 | 单一职责、边界清晰 |

D1 是唯一决定 Skill"生死"的维度——如果 description 不精准、触发关键词不合适，Skill 根本不会被加载。D3、D7、D8 则是 Skill 存在的根本理由：没有足够的领域知识密度，再好的流程也只是一份 checklist。

## 多模型交叉验证

不同模型对同一 Skill 的评分存在差异（如 GLM-5.1 评 7.8/A 等，Claude Opus 4.6 评 6.5/B），但指出的核心问题趋同。为此设计三级共识机制：^[raw/articles/2026-04-23_你写的Skill，及格了吗？.md]

1. **独立评估** — 各模型分别基于 8 维度打分
2. **交叉互审** — 分差 ≥2 分的维度需引用具体内容质疑
3. **仲裁综合** — 综合多方意见形成最终评级

## 参见

- [[agent-skill-specification]] — Skill 规范与设计原则
- [[skill-design-patterns]] — Skill 设计模式
