---
title: 柚漫剧
created: 2026-04-28
updated: 2026-04-28
type: entity
tags: [company, ai-coding, organization]
sources:
  - raw/articles/2026-04-28-youmanju-ai-workflow.md
---

# 柚漫剧

柚漫剧是一个以 APP 产品为锚点的 AI 全流程提效实践团队。其核心实践覆盖「需求—设计—开发—测试」全链路，构建了一套从单点提效到工程融合的 AI 赋能体系，推动产研模式从「人力驱动」向「AI 驱动」跃迁。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 核心实践

### 产品环节

柚漫剧团队在产品侧构建了 [[prompt-friendly-prd|Prompt 友好型 PRD]] 方法，将 AI 定位为「从 0 到 60 分」的信息处理助手，由人完成深度判断。通过标准化的需求模板和 AI 辅助撰写流程，提升需求文档的完整性和可读性。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

### 交互设计环节

产品与设计共同定义「需求范式」，使 AI 能辅助快速生成原型草稿。设计师主动作为「需求架构师」向上游赋能 PM，提供 Prompt 组件模板和需求撰写新模板。向下游交付实现「设计即代码」——设计师在研发设定的代码约束框架内进行调优，产出天然符合技术架构的设计方案。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

### 研发环节

研发侧全面接入 Zulu（AI 辅助编码）、F2C（设计稿转代码）、AI CR（智能代码审查）、AI 单测等工具。核心发现是仅靠工具本身不够——AI 需要理解工程的上下文。团队为此投入了 Rules（25 个规则文件覆盖自研跨端框架）、MCP 工具（客户端团队提供端能力查询）、Skills（可复用任务流程封装，单测 Skill 为首个落地场景）三大基建能力。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

### 测试环节

测试侧实现了从 Copilot 到 [[aiqa-system|AI Agent]] 的范式转移，建立 [[aiqa-system|AIQA 智能测试体系]]——主 Agent + 数字员工（「度小智」）+ 六大专业 Agent + 工具生态 + 交付工程记忆，从「人用工具」升级到「人机协同」。客户端需求用例生成占比 74%，服务端接口用例生成占比 81%。同时实现 AI 助力测试左移（AICR 智能代码审查 + AI Checker 走查）。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 关键经验

- **基建是单点到系统的桥梁**：Rules 让 AI 理解框架，MCP 让 AI 查到能力接口，Skills 让 AI 按流程做事
- **先找「高闭环任务」建立信任**：优先选择标准化程度高、反馈周期短的任务（样式拼装、单测补齐）形成 AI 闭环样板
- **跨角色协同需要前置对齐架构**：不同角色思维方式不同，需在生成前对齐整体结构
- **承认边界，分场景落地**：重交互、无包袱的新项目适合整体生成流程；存量项目适合模块化渐进式引入^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 展望

团队计划持续完善 Rules/Skills 体系，探索 Figma → Mermaid → 代码链路，以及在 1~2 个有历史包袱的复杂模块上试点多角色协同。^[raw/articles/2026-04-28-youmanju-ai-workflow.md]

## 交叉引用

- [[ai-native-organization]] — AI 原生组织方法论，柚漫剧的「AI 驱动」范式跃迁是典型案例
- [[prompt-friendly-prd]] — Prompt 友好型 PRD 方法论，柚漫剧产品环节的核心创新
- [[aiqa-system]] — AIQA 智能测试体系，柚漫剧测试环节的核心实践
- [[ai-native-pm]] — AI 原生产品经理方法论，与柚漫剧产品环节实践互为补充
