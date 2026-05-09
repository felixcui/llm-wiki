---
title: Skill Evaluator
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [tool, agent-framework, open-source]
sources:
  - raw/articles/2026-04-23_你写的Skill，及格了吗？.md
---

# Skill Evaluator

**类型**: 开源 Skill 评估工具
**作者**: 大博（sunxingboo）
**GitHub**: [github.com/sunxingboo/skill-evaluator](https://github.com/sunxingboo/skill-evaluator)
**适用框架**: [[claude-code]]、[[openclaw]]（小龙虾）、百度 dodo 等支持 Skill 的 AI 工具

## 概述

Skill Evaluator 是一套 8 维度的 Skill 量化评估工具，将 [[agent-skill-specification|Skill]] 质量从主观感受转化为可量化分数（S/A/B/C/D 五级），帮助开发者识别改进短板，辅助用户横向对比选择优质 Skill。^[raw/articles/2026-04-23_你写的Skill，及格了吗？.md]

## 8 维度评估框架

评估维度分布在 Skill 生命周期的三个阶段：

### 第一阶段：能不能被找到
- **D1. 元数据质量**（权重最高）：description 是否精准？是否包含触发关键词？是否写明不该在什么场景触发？这是唯一决定 Skill "生死"的维度

### 第二阶段：用起来顺不顺
- **D2. 执行引导清晰度**：Agent 加载后知道该走哪条路吗？
- **D4. 工作流完整性**：流程是否端到端？异常有没有处理方案？
- **D5. 输入输出清晰度**：用户给什么、得到什么？
- **D6. 资源利用**：是否遵循渐进式披露（SKILL.md 精简，详细内容按需加载）？

### 第三阶段：值不值得存在
- **D3. 领域知识密度**：Skill 里内嵌的知识是否通用 Agent 不靠它也做不到？
- **D7. 写作质量**：结构清晰？Agent 能快速扫读？
- **D8. 范围与聚焦**：做好一件事，不过宽不过窄

## 多模型交叉验证

不同模型对同一 Skill 的评分存在差异（如 GLM-5.1 评 7.8/A，Claude Opus 4.6 评 6.5/B），但指出的核心问题趋同。设计了三级共识机制：^[raw/articles/2026-04-23_你写的Skill，及格了吗？.md]

1. **独立评估**：多模型各自按 8 维度标准独立打分
2. **交叉互审**：找出分差 ≥2 分的维度，引用具体内容质疑对方，同时做自我修正
3. **仲裁综合**：主模型汇总所有评估和互审结果，做最终裁决

每个维度标注共识度（一致/多数/仲裁），仲裁表示质量处于模糊地带，值得重点关注。

## 四种执行策略

为适配不同 AI 工具环境设计了自动路由策略：
- **策略 A**：工具原生多模型调用（如 Claude Code 的 subagent）
- **策略 B**：通过千帆 API 调用第三方模型（Python 脚本，无额外依赖）
- **策略 A+B**：混合使用
- **策略 C**：单模型多视角评估（严格派/务实派/中立派三个角色）

## 局限性

该框架度量的是 Skill 的**文档工程质量**，而非运行时性能的完整度量。不能替代实际执行测试。
