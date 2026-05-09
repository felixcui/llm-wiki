---
title: Skill Distillation vs RAG
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [agent-framework, data, optimization]
sources:
  - raw/articles/2026-04-23_【万字】抛开RAG谈蒸馏.skill，大概率是形式主义.md
---

# Skill Distillation vs RAG

## 纯蒸馏的局限

当前社区流行的"蒸馏同事.skill"——将一个人的能力封装为 Skill——存在根本性局限：^[raw/articles/2026-04-23_【万字】抛开RAG谈蒸馏.skill，大概率是形式主义.md]

- Skill 只能教 AI **怎么做**（流程），却不一定能让 AI 知道**为什么这么做**（知识）
- 蒸馏出来的往往不是一个人的完整能力，而只是最容易被整理、最容易被复用、也最容易被自动化的那部分——更接近 Workflow 而非"能力"
- 真正难蒸馏的是：为什么优先看这个文档、为什么知道这个问题要参考上个月的事故复盘、为什么知道这里不能照常规流程走——这些都和数据有关

## Skills + RAG 的正确组合

正确的做法是：**Skill 负责定义怎么做，RAG 负责提供做这件事时所需的知识，模型负责基于流程和知识把事情串起来**。

- **Skill** 定义骨架流程（任务编排）：识别问题类型 → 判断是否需要查知识库 → 构造检索问题 → 过滤重排 → 基于结果回答
- **RAG** 在关键节点动态取知识：最新政策、历史案例、相关规则，而非写死在 prompt 里
- **模型** 对检索结果做局部推理：判断哪些资料相关、是否冲突、最终形成稳妥回答

没有 RAG 的 Skill 更像在模仿一个人的动作；有了 RAG 之后，Skill 才开始有机会模仿一个人做判断时的信息组织方式。

## 参见

- [[agent-skill-specification]] — Agent Skill 规范与设计原则
- [[skill-design-patterns]] — Skill 设计模式
