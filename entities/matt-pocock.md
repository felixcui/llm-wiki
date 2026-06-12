---
title: Matt Pocock
created: 2026-05-15
updated: 2026-05-15
type: entity
tags: [person, open-source]
sources:
  - raw/articles/2026-05-14_75Kstar的SKILL爆款仓库，没有一行新东西，且个个数十行，凭什么疯传？.md
---

# Matt Pocock

**身份**: TypeScript 教学专家 / 开源开发者
**知名项目**: mattpocock/skills（75K stars）、Total TypeScript 课程、AI Hero
**职业经历**: Vercel、XState 开发者

## 概述

Matt Pocock 是以 TypeScript 教学出名的开发者，创办了 Total TypeScript 课程和 AI Hero。他在 Vercel 和 XState 都做过开发。2026年5月，他的 `mattpocock/skills` 仓库在 GitHub 获得 75K stars，成为 AI Coding 领域的现象级项目。^[raw/articles/2026-05-14_75Kstar的SKILL爆款仓库，没有一行新东西，且个个数十行，凭什么疯传？.md]

## Skills 仓库的核心理念

Matt 的 skills 仓库包含十几个 markdown 文件，每个几十行规则，覆盖从理解、设计、实现到维护的完整工程流程。表面规则朴素，但背后是 5 个反复出现的信念：^[raw/articles/2026-05-14_75Kstar的SKILL爆款仓库，没有一行新东西，且个个数十行，凭什么疯传？.md]

1. **反馈优先** — 调试 bug 前先搭反馈循环，写代码前先让测试 RED 再 GREEN
2. **行为优于实现** — 测公开接口行为，不测内部实现；PRD 描述用户能力，不绑定文件路径
3. **垂直切片** — 每个工作项从用户端到数据库端到端打穿，反对水平拆分
4. **显式优于隐式** — 隐性约定全写下来，变成 ADR、CONTEXT.md 或 Skill
5. **删除优于保留** — 原型用完就删，用"删除测试"判断模块去留

## 经典血统

Matt 的 skills 不是凭空发明，而是将 4 本经典软件工程著作的方法论「压」成了 AI 能执行的招式：^[raw/articles/2026-05-14_75Kstar的SKILL爆款仓库，没有一行新东西，且个个数十行，凭什么疯传？.md]

| 信念 | 来源著作 | 年份 |
|------|---------|------|
| 反馈优先 + 行为优于实现 | Kent Beck《Test-Driven Development》 | 2002 |
| 垂直切片/曳光弹 | Hunt & Thomas《The Pragmatic Programmer》 | 1999 |
| 显式优于隐式 | Eric Evans《Domain-Driven Design》 | 2003 |
| 深模块/简单接口 | John Ousterhout《A Philosophy of Software Design》 | 2018 |

## 关键洞察：心法→招式

Matt 做的事不是翻译经典，而是**举重若轻**——把经典方法论 + 多年踩坑经验，融会贯通后压成几十行朴素 markdown。他的能力反 AI：AI 能记住信息，但不能沉淀判断。真正稀缺的不是发现原则，而是有能力把原则压成"朴素到没人愿意承认它复杂"的样子。^[raw/articles/2026-05-14_75Kstar的SKILL爆款仓库，没有一行新东西，且个个数十行，凭什么疯传？.md]

## 与 Skill 生态的关系

Matt 的 skills 仓库是 [[agent-skill-specification|Skill 规范]]生态中最具影响力的实践案例之一。其设计符合 [[skill-design-patterns|Skill 设计模式]]的核心原则——总上下文消耗控制在 ~10K tokens 以内，分层知识架构，明确的收敛条件。

## 参考

- GitHub: https://github.com/mattpocock/skills
