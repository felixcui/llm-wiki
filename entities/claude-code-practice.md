---
title: Claude Code 实践案例
created: 2026-04-30
updated: 2026-05-07
type: entity
tags: [tool, agent-framework, ai-coding, practice]
sources:
  - raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md
  - raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md
  - raw/articles/2026-04-28-yc-claude-engineering-team.md
  - raw/articles/2026-04-27_@的推文_3.md
  - raw/articles/2026-04-28-subagents-clean-context.md
  - raw/articles/2026-04-30-claude-vscode-plugin.md
  - raw/articles/2026-05-06-blog-practice.md
  - raw/articles/2026-05-07-agents-md-guide.md
---

# Claude Code 实践案例

> 本页拆分自 [[claude-code]]，汇总社区与企业的实践案例。

## 应用场景

Claude Code 应用于软件开发、代码重构、多文件批量修改、终端自动化、技术文档生成等场景，以及与 Team Skills 等多 Agent 协作框架的集成。^[raw/articles/2026-04-25_CoordinationEngineering关键一环，JiuwenClaw再发布TeamSkills技能新范式.md]

## 实践者视角

Claude Code 被多位实践者认为是当前投入产出比最高的 Agent 工具。实际应用场景包括：基于 Skills 体系和 multi sub-agent 构建知识管理工作流（参见 [[agent-driven-learning]]）、全自动托管 Kaggle 比赛（约 4000 支队伍中最高排名第六）、驱动海量 Skill 库完成复杂任务编排等。^[raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md]

## Subagent 机制

Claude Code 内置了 Subagent 机制用于上下文隔离：^[raw/articles/2026-04-28-subagents-clean-context.md]

- **Explore**：在独立窗口搜索代码库（grep/find），只返回相关发现
- **Plan**：调研架构，返回 step-by-step 计划文档，主上下文看不到中间读取过程

**自定义 Subagents**：在 `.claude/agents/`（项目级）或 `~/.claude/agents/`（全局级）放置 Markdown 文件（YAML frontmatter 定义 name/description/tools/model），Claude Code 根据 `description` 自动匹配调用。详见 [[sub-agent-vs-agent-team]]。

**上下文 Fork**：设置 `CLAUDE_CODE_FORK_SUBAGENT=1` 后，subagent 继承父级完整对话上下文，共享 prompt cache prefix（后续子进程输入 token 成本约降 10 倍），但工具调用仍隔离、只有最终摘要返回主对话。

## Gary Tan（YC CEO）高级用法

Gary Tan 将 Claude Code 定位为"需要角色、流程和评审的工程团队"而非单个助手：^[raw/articles/2026-04-28-yc-claude-engineering-team.md]

- **GStack 框架**：薄脚手架（thin harness）+ 胖 Skills（fat skills），复杂度放在可复用专业技能里
- **Office Hours Skill**：先问"谁真的想要这个"，把需求验证前置到编码之前
- **Plan Mode + 对抗性评审**：多步自动评审发现缺口（失败处理、隐私、2FA），评分从 6 提高到 8
- **并行 PR 工厂**：同时运行 10-15 个 Claude Code 会话，每个想法/bug report 变成独立 worktree，各自跑完整评审流程后合并
- **浏览器 QA 自动化**：自建 Playwright CLI，agent 截图、点击、填表、下载、跑回归测试，绕过 Chrome MCP 的慢速和不稳定

## 社区 Skills 生态

Claude Code 的 Skills 机制催生了活跃的社区生态，开发者围绕不同场景构建了大量扩展。以下为社区推荐的高质量 Skills：^[raw/articles/2026-04-27_@的推文_3.md]

| Skill | 功能定位 | 说明 |
|-------|---------|------|
| **Claude Mem** | 记忆管理 | 为 Claude 添加持久记忆，避免重复描述需求 |
| **Obsidian Skills** | 上下文理解 | 让 Claude 真正理解用户的 Obsidian 笔记上下文 |
| **GSD** | 任务执行 | 强化 Claude 的实际执行能力，而非停留在讨论层面 |
| **LightRAG** | 知识图谱 | 集成 RAG 知识图谱，快速获得专业知识 |
| **Superpowers** | 增强能力 | 为 Claude 装配额外的"超能力"扩展 |
| **Everything Claude Code** | 功能集合 | 汇集多种实用功能的综合 Skill 包 |
| **n8n-MCP** | 自动化集成 | 通过 MCP 协议接入 n8n 自动化工作流，一个提示词驱动完整流程 |
| **UI UX Pro Max** | 设计辅助 | 提升设计审美能力，改善 UI/UX 输出质量 |

## 用户实践案例：零代码构建 VSCode 插件

2026 年 4 月，一位开发者使用 Claude 在 2 小时内零手写代码完成了一个生产级 VSCode 插件开发——监控 Comate 模型用量，支持状态栏实时显示、三色告警、Webview 详情面板、自动从 Chrome 读取 Cookie 并静默登录。全程 8 个核心文件、1000+ 行 TypeScript、零第三方 npm 依赖，打包为 `.vsix` 可直接分发给同事使用。^[raw/articles/2026-04-30-claude-vscode-plugin.md]

该案例的核心方法论是「**Claude 负责实现和验证，我负责判断和决策**」。具体的人机协作原则包括：

- **设计对话而非让 AI 自由发挥**：不扔一句宽泛需求，而是提供接口样例、真实报错、cURL 响应等完整上下文，让 Claude 基于真实数据而非猜测工作
- **提供足够上下文**：差的提问只描述症状，好的提问贴完整报错堆栈和真实请求，Claude 能瞬间定位根因（如 Cookie 作用域问题）而非空泛猜测
- **人类判断 vs AI 实现**：做什么、先做什么、这条路值不值得继续——这些由人决策；陌生领域知识覆盖、代码实现、边界情况提醒——这些由 AI 提供
- **AI 诚实比 AI 迎合更重要**：Claude 在项目初期直接否定作者倾向的 Webview 登录方案（VSCode 隔离 + SSO X-Frame-Options 限制），避免了数小时试错
- **接受 AI 也会犯错**：Claude 猜的字段名第一版全错、文件路径反复放错、Cookie 作用域处理需三次迭代才收敛——需用真实响应验证 AI 输出

该案例的深层启示与 [[agent-driven-learning]] 中「人负责策略、AI 负责执行」的实践模式一致，也印证了 [[harness-engineering-deep-dive]] 的核心论点：AI 编程工具的可靠性瓶颈不在模型智能，而在于人如何设计交互上下文和反馈闭环。

## 博客功能开发实战

使用 [[claude-code]] 开发博客功能的案例展示了"plan→execute→verify"的逐功能迭代模式，而非一次性全量生成。^[raw/articles/2026-05-06-blog-practice.md]

### 技术栈与功能

- **NeonDB + Resend**：数据库使用 Neon（Serverless PostgreSQL），邮件通知使用 Resend 服务
- **Margin Annotations**：使用 margin annotations（边注）模式实现博客评论功能，区别于传统的嵌套评论列表
- **IntersectionObserver**：点赞功能基于 `IntersectionObserver` API 实现，当点赞按钮进入视口时触发交互

### 开发方法论

该案例强调按功能逐个推进——每个功能都经历"规划→实现→验证"的完整循环，而非一次性描述全部需求后让 AI 批量生成。这种模式与 [[spec-driven-development]] 的理念一致，同时体现了 [[agentic-engineering]] 的实践精神。

## AGENTS.md 演进与上下文配置

Claude Code 的项目配置经历了从 CLAUDE.md（Anthropic 专属格式）到碎片化配置文件（`.cursorrules`、`.windsurfrules` 等各自为政），最终统一为跨平台开放标准 [[agents-md]] 的演进过程。^[raw/articles/2026-05-07-agents-md-guide.md]

这一演进反映了 AI Coding Agent 生态的核心矛盾：开发者需要在多个工具间切换，但每个工具的上下文配置格式不同。AGENTS.md 通过统一为"地图，而非手册"的设计理念（Map, not Manual），让一份配置文件在 [[claude-code]]、Cursor、Windsurf 等主流工具间通用。实践中，AGENTS.md 也是 [[harness-engineering-deep-dive]] 在项目配置层面的具体落地形式。
