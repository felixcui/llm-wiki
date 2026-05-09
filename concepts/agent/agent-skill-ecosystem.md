---
title: Agent Skill Ecosystem
created: 2026-04-28
updated: 2026-04-28
type: concept
tags: [agent-framework, open-source]
sources:
  - raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md
  - raw/articles/2026-04-25_开源一个PPTSkill｜压进了我10年的设计经验.md
  - raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md
  - raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md
  - raw/articles/2026-04-28-google-agent-skill-toolkit.md
  - raw/articles/2026-04-28-taobao-marketing-workflow.md
  - raw/articles/2026-04-22_我做了个Skill：让AI帮你生成Logo和图标.md
  - raw/articles/2026-04-24_一句话变成62个专业分镜：山音超级导演大师Skill来了.md
---

# Agent Skill Ecosystem

## 跨框架 Skill 生态

不同 Agent 框架对 Skill 的支持和兼容程度不同：

| 框架 | Skill 支持 | 特点 |
|------|-----------|------|
| [[claude-code]] | CLAUDE.md / Agent Skills | 原生支持，社区生态丰富 |
| [[openclaw]] | 静态 Skill 文件 | 基础支持，作为配置文件 |
| [[hermes-agent]] | 动态 Skill + 自进化 | 自动生成与持续优化 |
| JiuwenClaw | Agent Skills + Team Skills | 单 Agent + 多 Agent 全覆盖 |

Team Skills Hub 平台已沉淀了开发编程、办公生产力、内容创作、多模态媒体、数据科研、合规法律、生活健康、金融理财八大类别的团队技能。^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]

## Google Agent Skills 标准化

Google 于 2026 年 4 月正式开源 Agent Skills（github.com/google/skills），标志着 Skills 从社区实践升级为厂商级标准。Addy Osmani 的 agent-skills 仓库（24K stars）提供了 19 个工程 Skills + 7 个 Commands，覆盖 Define→Plan→Build→Verify→Review→Ship 全生命周期。Skills 被定位为介于 Prompt 和 Tool 之间的"轻量级、可复用、主动式"能力格式，核心解决 MCP Context Inflation（上下文膨胀）问题。安装方式：`npx skills install github.com/google/skills`。^[raw/articles/2026-04-28-google-agent-skill-toolkit.md]

## SkVM：Skill 的编译优化层

[[skvm]]（上海交大 IPADS 团队）为 Skill 规范引入系统级的编译优化能力，将 Skill 从"静态文本"提升为"可编译、可优化、跨平台执行"的程序化资产。^[raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md]

### 编译优化

SkVM 借鉴 JVM 架构，在 Skill 安装时执行 AOT（Ahead-of-Time）编译：

- **基于能力的编译**：提炼 26 种原子能力（工具调用、指令遵循、格式对齐等），为模型 + Harness 生成能力画像，自动降低 Skill 对超出模型能力的部分的需求
- **环境绑定**：自动提取 Skill 依赖的包和工具，生成安装脚本，运行前一键配好环境
- **并发提取**：发掘 Skill 中的并行机会（数据并行、指令并行、线程并行），生成可并行 DAG 工作流

### 运行时优化

在运行阶段，SkVM 采用 JIT（Just-in-Time）编译：

- **代码固化**：对重复执行的代码模板进行指纹匹配，匹配成功后直接固化可执行代码，跳过 LLM 重复生成，将执行时间从上万毫秒压缩至几百毫秒（19-50 倍加速）
- **自适应重编译**：收集运行时错误日志，自动重新优化 Skill

SkVM 的核心价值在于：它让 Skill 规范从"写得好不好"的问题，延伸到"跑得快不快"的问题——同一份 Skill 在不同模型和 Harness 上也能通过编译获得最优执行效率。^[raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md]

## 应用案例

- **guizang-ppt-skill**：十年设计经验 → PPT 生成规范，10 种布局、5 套主题 ^[raw/articles/2026-04-25_开源一个PPTSkill｜压进了我10年的设计经验.md]
- **Logo Generator Skill**：AI 生成 SVG 基础 + AI 生成高级展示图，信息收集 → 6+ SVG 变体 → 高级展示图 ^[raw/articles/2026-04-22_我做了个Skill：让AI帮你生成Logo和图标.md]
- **导演分镜 Skill**：[[shanyin-director-skill]] 将一句话转化为 62 个专业分镜：导演定调 → 节奏规划 → 剧本微调 → 分镜拆解 → 输出分镜表 ^[raw/articles/2026-04-24_一句话变成62个专业分镜：山音超级导演大师Skill来了.md]
- **teamskill-creator**：自动生成多 Agent 团队技能，如 23 位 AI 医学专家联合会诊 ^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]
- **Hermes Skill 库**：运行轨迹自动积累的动态技能库 ^[raw/articles/2026-04-25_深度解析HermesAgent如何实现"自进化"及其PromptContextHarness的设计实践.md]
- **淘天 ProForm/ProTable Skill**：领域 Skill 作为按需手册，将表单（ProForm + useForm）和表格（ProTable + useTable）的开发范式分别封装，编码时由 Agent 按需加载。迁移场景中实现 62.73% 迁移时间降低 ^[raw/articles/2026-04-28-taobao-marketing-workflow.md]

## 参见

- [[agent-skill-specification]] — Skill 规范与设计原则
- [[skvm]] — Skill Virtual Machine
- [[skill-design-patterns]] — Skill 设计模式
