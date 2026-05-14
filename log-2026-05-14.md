# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete
> When this file exceeds 500 entries, rotate: rename to log-YYYY.md, start fresh.

## [2026-04-25] create | Wiki initialized
- Domain: AI/ML 前沿资讯与技术知识库
- Structure created with SCHEMA.md, index.md, log.md
- Tag taxonomy: 模型 / 产品工具 / 组织人物 / 技术概念 / 元信息
- Concept sub-directories: ai-models, agent, tools, ai-coding, principles

## [2026-04-25] ingest | Batch ingest 04-25 articles (9 files)
- Raw articles linked from /Users/felix/work/github/media-conent/raw/
- entities/claude-code.md, openclaw.md, deepseek-v4.md, gpt-5.5.md, hermes-agent.md created
- concepts: prompt-context-harness-engineering, coordination-engineering, agent-skill-specification, grpo, ai-office-automation created

## [2026-04-25] ingest | Batch ingest 04-22 ~ 04-24 articles (24 files)
- Raw articles linked: 10 files (04-22) + 6 files (04-23) + 8 files (04-24)
- New entity pages: kimi-k2.6, generic-agent, gpt-image-2, floatim, claude-agent-sdk
- New concept pages: self-improving-agent, multi-agent-code-review, context-rot, agent-social-network, agent-cluster
- Updated: claude-code (ultrareview, ultraplan, 1M 上下文), hermes-agent (Self-Improving, RDSHermes), prompt-context-harness-engineering (七层模型, 企业案例), agent-skill-specification (蒸馏 vs RAG, 8 维度评估)
- Total wiki pages: 20

## [2026-04-25] ingest | AI编程工作流选型指南（SDD 工具生态）
- Source: raw/articles/2026-04-25_AI编程工作流选型指南：Spec-Kit、OpenSpec、Superpowers、Spec-First全面对比（附实操落地路径）.md
- New entity pages: spec-kit, openspec, superpowers（SDD 三大工具）
- New concept page: spec-driven-development（规范驱动开发理念、分层架构、选型决策树）
- Updated: agent-skill-specification（新增 Skill 与 SDD 交叉引用段落）
- Total wiki pages: 24
## [2026-04-25] update | 整合《再看Hermes Skills：Agent 如何自我进化？》新文章
- Source: raw/articles/2026-04-25-hermes-skills-agent-self-evolution.md
- Updated: hermes-agent（落盘原子替换、User Message 注入保护 Prompt Cache、最终一致性、过程资产三层模型）
- Updated: self-improving-agent（经验外部化路线对比、自动沉淀风险与五道门禁、hermes-agent-self-evolution 仓库）
- Updated: agent-skill-specification（稳定前缀不乱动原则、来源分级信任原则）
- Updated: index.md（hermes-agent 条目描述更新）

## [2026-04-25] create | 同步 Deep Research 技术栈文章（Onyx + CrewAI + Voxtral）
- Source: raw/articles/2026-04-25-deep-researcher-onyx-crewai-voxtral.md
- New entity page: onyx（开源 AI 平台，三阶段架构，六阶段检索流水线，40+ 数据源，DeepResearch Bench 第 1 名）
- CrewAI / Voxtral 不单独建实体（仅本文提及，不满足多来源创建标准）
- Total wiki pages: 26

## [2026-04-25] create | 同步《子智能体vs智能体团队：颠覆全局的架构抉择》文章
- Source: raw/articles/2026-04-25-sub-agent-vs-agent-team.md
- New concept page: sub-agent-vs-agent-team（Sub-Agent vs Agent Team 架构对比、5 种 multi-agent 模式、设计原则、使用场景判断）
- Updated: coordination-engineering（新增 Sub-Agent vs Agent Team 架构选择段落、交叉引用、source）
- Updated: index.md（新增条目、Total pages → 27）
- Total wiki pages: 27

## [2026-04-25] update | 同步《DeepSeek V4 发布｜全面测评+深度解读合集》文章
- Source: raw/articles/2026-04-25-deepseek-v4-review-collection.md
- Updated: entities/deepseek-v4.md（新增国产化适配、成本优势章节，补充来源）

## [2026-04-25] ingest | Anthropic PM Cat Wu 访谈 — AI 产品经理角色变革
- Source: https://mp.weixin.qq.com/s/2IxjZxhaZoC7kTSnKt4WHg
- Raw: raw/articles/2026-04-25_ai_pm_catwu.md
- New entity pages: cat-wu（Anthropic Claude Code/Cowork 产品负责人）、boris-cherny（Claude Code 技术负责人和创造者）
- New concept page: ai-native-pm（AI 原生产品经理方法论：Research Preview、Evergreen Launch Room、Team Principles、角色融合）
- Updated: claude-code（新增产品策略与团队段落、源码泄露事件、wikilinks）
- Updated: openclaw（新增创始人与爆火历史、Anthropic 封堵事件、来源）
- Updated: index.md（新增 3 个条目，Total pages → 33）
- Total wiki pages: 33

## [2026-04-26] ingest | 批量同步 04-22 ~ 04-24 Wiki 页面（待跟进1 + 待跟进2）

### 04-22 批次（10 篇）
- New entity: builderpulse（开源独立开发者信息日报工具）
- Updated: claude-code（新增 System Prompt 三层架构、6 个 AgentTool 详情、P/C/H 实现细节章节）
- Updated: hermes-agent（新增 Hermes+KimiK2.6 教程 source）
- Updated: spec-kit, openspec, superpowers（新增三剑客对比 source）
- Updated: gpt-image-2（新增提示词玩法 source）
- Updated: prompt-context-harness-engineering（新增 Claude Code 源码解析 P/C/H 实现细节子章节）
- Skipped: 2 篇重复 Kimi 文章、3 篇已被现有页面覆盖

### 04-23 批次（7 篇）
- New entities: tencent-lego（腾讯 CDN LEGO 五层架构案例）、skill-evaluator（Skill 五维评估框架）、aaron-levie（Box CEO，无界面化倡导者）
- New concept: skills-rag-integration（Skills + RAG 整合方法论）
- Updated: prompt-context-harness-engineering（新增 LEGO 五层架构案例）、multi-agent-code-review（新增对抗式 CR 实践）、agent-skill-specification（新增 Skill 作为岗位核心技能）
- Skipped: 3 篇 Image-2 相关已被覆盖、2 篇提及不满足阈值、1 篇提示词技巧不满足阈值

### 04-24 批次（8 篇）
- New entities: floatboat（FloatBoat 公司/产品矩阵）、lovart（Lovart AI 图像设计平台）、shanyin-director-skill（山音超级导演大师 Skill）、rdshermes（Hermes 托管版）
- New concept: ai-architecture-design（AI 主导架构设计四步实践）
- Updated: openclaw（新增 Harness 工程取向章节）、prompt-context-harness-engineering（新增 Generic Agent 极简实践）、self-improving-agent（新增 OpenClaw vs Hermes 对比、自我进化方向）、agent-skill-specification（新增导演分镜案例）、spec-driven-development（新增 AI 架构设计角色延伸）
- Skipped: 6 篇已被现有页面完整覆盖、5 项概念不满足创建阈值

### 汇总
- 新建页面: 11（8 实体 + 3 概念）
- 更新页面: 13
- Total wiki pages: 44
- Updated: index.md, log.md

## [2026-04-26] update | 例行维护：知识库健康巡检修复
- 修复断链：spec-driven-development.md 中 [[spec-first]] 改为纯文本引用（不满足独立建页标准）
- 修复 onyx.md 非标准 tags（rag, deep-research, self-hosted → data, inference）
- 清理 raw/articles/ 中 1 个断链 symlink（"从玩具到生产力" 旧链接）
- 拆分 claude-code.md（219→160行），新建 claude-code-system-prompt.md（83行）
- 拆分 prompt-context-harness-engineering.md（218→83行），新建 harness-engineering-deep-dive.md（98行）+ harness-engineering-cases-and-evolution.md（77行）
- Total wiki pages: 50
- Updated: index.md, log.md
- Source: https://mp.weixin.qq.com/s/cjNq3gvTGzKZ9PCYUcBhgA
- Raw: raw/articles/2026-04-26-embrace-ai-one-year-tools-practice.md
- New concept: agent-driven-learning（Agent 驱动学习三层架构 + 知识反哺循环）
- Updated: claude-code（新增实践者视角小节）、prompt-context-harness-engineering（新增四阶段演化框架 + OpenAI Harness 实践）、openclaw（新增 source）、spec-kit（新增 source）、spec-driven-development（新增 source）
- Total wiki pages: 47
- Updated: index.md, log.md
- Source: https://mp.weixin.qq.com/s/9IpWaz8yXHViW_MBNkOZfQ
- Raw: raw/articles/2026-04-26-designing-products-for-agents.md
- New entity: teddy-riker（Ramp 产品团队，提出 Agent First Design 三原则）
- New concept: agent-first-design（软件交互三阶段演进、三大设计原则、Ramp/Salesforce 案例）
- Updated: aaron-levie（新增 Salesforce Headless 360 + Ramp MCP 数据佐证、agent-first-design 交叉链接）
- Total wiki pages: 46
- Updated: index.md, log.md

## [2026-04-26] lint | 知识库健康巡检修复（第二轮）
- 修复 builderpulse 0 出链 → 新增 [[ai-native-pm]]、[[agent-driven-learning]] 两个 wikilink
- 修复 shanyin-director-skill 仅 1 出链 → 新增 [[claude-code]]、[[ai-office-automation]] 两个 wikilink
- 修复 5 个页面非标准 tags：
  - sub-agent-vs-agent-team: `multi-agent` → `coordination-engineering`
  - ai-native-organization: 移除 `management`，保留 `organization`
  - ragen/vagen/world-model-agent 的 `reinforcement-learning`、`computer-vision` 已添加到 SCHEMA.md taxonomy
- SCHEMA.md taxonomy 新增 3 个标签：`reinforcement-learning`、`computer-vision`、`organization`
- 修复 index.md Total pages 计数错误（50 → 49）
- Updated: builderpulse, shanyin-director-skill, sub-agent-vs-agent-team, ai-native-organization, SCHEMA.md, index.md

## [2026-04-27] ingest | Anthropic 产品负责人 Kat Wu 第二次访谈 — 极速发版方法论
- Source: https://mp.weixin.qq.com/s/t09DBqWAlujcUOfa3iWtCQ
- Raw: raw/articles/2026-04-27-anthropic-launch-secrets.md
- New entity: anthropic（AI 安全公司，产品矩阵、极速发版文化、角色融合）
- New concept: research-preview（Research Preview 早期发布策略，降低承诺、快速迭代）
- New concept: launch-room（Launch Room 跨职能快速发布机制，常驻频道一天发版）
- New concept: designing-for-future-models（为未来模型做产品，harness 会被模型吃掉，功能持续清理）
- Updated: cat-wu（角色融合、Just do things、别停在 95%、Cowork 实践、工作方式）
- Updated: boris-cherny（分工模式：Boris 负责愿景、Kat 负责执行路径）
- Updated: claude-code（功能演进案例、System Prompt 清理、产品三阶段路线图、源码泄露补充）
- Updated: openclaw（封堵原因补充：需求增长太快、credits 缓冲）
- Updated: ai-native-pm（Research Preview/Launch Room 细节、为未来模型做产品）
- Total wiki pages: 55
- Updated: index.md, log.md

## [2026-04-27] ingest | DeepSeek V4 成本优势与价格竞争
- Source: https://mp.weixin.qq.com/s/_k6tFQEKrvUiGh1RFDOUrQ
- Raw: raw/articles/2026-04-27-deepseek-v4-cost-reduction.md（symlink）
- New concept: llm-price-competition（三种定价逻辑、开发者迁移趋势、长期预测）
- Updated: deepseek-v4（新增定价对比表、用户迁移案例 3 则、竞争格局影响）
- Updated: gpt-5.5（新增 Priority 套餐/Pro 版/推理强度分层定价细节）
- Total wiki pages: 56
- Updated: index.md, log.md

## [2026-04-27] ingest | Agent 技术债 — 七大生产基础设施模块
- Source: https://thenewstack.io/hidden-agentic-technical-debt/ (InfoQ 中文翻译)
- Raw: raw/articles/2026-04-27_智能体工程的隐形技术债：10分钟造出一个Agent，公司却要为它养一个平台团队.md
- New concept: agent-technical-debt（七大基础设施模块、Google 2015 论文平行、债务触发时间线、微服务类比）
- Updated: harness-engineering-deep-dive（新增生产级七大基础设施模块章节 + Harness 七层与生产设施的映射表）
- Updated: coordination-engineering（新增生产化挑战：编排与治理章节，含路由错误/非确定性/所有权模糊/治理放大）
- Updated: ai-native-organization（新增组织级基础设施章节：治理与注册表作为组织管理工具、闭环学习基础设施支撑）
- Total wiki pages: 57
- Updated: index.md, log.md

## [2026-04-27] ingest | 记忆，是 Agent 基建｜对话 Calvin@Vida
- Source: https://mp.weixin.qq.com/s/VZUhUo-ppqpICzbbGgCpbA
- New entities: openchronicle（开源 AI 记忆基础设施，AX Tree + MCP，OpenAI Chronicle 替代品）、calvin（清华 00 后，Vida/OpenChronicle 创建者，Proactive Agent 研究方向）
- Updated: self-improving-agent（新增"设备侧记忆基础设施：OpenChronicle"章节，添加 AX Tree 方案对比，补充来源，更新日期）
- Total wiki pages: 59
- Updated: index.md, log.md

## [2026-04-27] ingest | SkVM — Skill 语言虚拟机（SJTU IPADS）
- Source: arxiv 2604.03088
- Raw: raw/articles/2026-04-27_Skill也有语言虚拟机了！上交大开源SkVM，实现一次编写，处处高效.md
- New entity: skvm（Skill 语言虚拟机，AOT/JIT 编译，26 原子能力基元，30B 匹配 Opus 4.6，40% token 减少）
- Updated: hermes-agent（新增 SkVM 兼容性段落）、agent-skill-specification（新增 SkVM 编译优化层章节）、harness-engineering-deep-dive（新增编译式 Harness 优化章节）
- Total wiki pages: 60
- Updated: index.md, log.md

## [2026-04-27] ingest | 多 Agent 跨境电商实战 + Claude Code 社区 Skills 生态
- Source: raw/articles/2026-04-27_我用多Agent搭了一家跨境电商公司，起飞！.md
- Source: raw/articles/2026-04-27_@的推文_3.md
- Updated: ai-native-organization（新增 CodeBanana 跨境电商超级组织案例）、coordination-engineering（新增多 Agent 协作实战案例）、sub-agent-vs-agent-team（新增 PM Agent 路由模式）
- Updated: claude-code（新增社区 Skills 生态段落：Claude Mem、LightRAG、n8n-MCP 等 8 个社区 Skills）
- Updated: index.md, log.md

## [2026-04-27] ingest | 批量同步 26 篇 Raw 文章 symlink
- 新增 symlink 26 个（01-28 ~ 04-27 日期范围）
- 其中 2 个源文件未找到（鹅厂 NotebookLM、Multica 对谈）
- 同步评估：3 篇高优先级已创建/更新 Wiki 页面（SkVM、Agent 技术债、OpenChronicle）
- 2 篇中优先级已更新现有页面（多 Agent 电商、Claude Code Skills）
- 21 篇低优先级/无实质内容已跳过（不满足建页阈值或信息已被覆盖）

## [2026-04-27] lint | 知识库健康巡检修复（第三轮）
- 修复 builderpulse 出链不足（2→5 个 wikilink）：新增 [[research-preview]]、[[ai-office-automation]]、[[launch-room]]
- 修复 openspec 出链不足（4→6 个 wikilink）：新增 [[ai-architecture-design]]、[[designing-for-future-models]]
- 修复 spec-kit 出链不足（4→6 个 wikilink）：新增 [[claude-code-system-prompt]]、[[prompt-context-harness-engineering]]
- claude-code.md（202 行）评估后保留不拆分（仅超 2 行，内容内聚且已有子页面 claude-code-system-prompt.md）
- Updated: builderpulse, openspec, spec-kit

## [2026-04-28] ingest | 批量同步 14 篇 Raw 文章 symlink + Wiki 页面创建/更新
- 新增 symlink 14 个（02-22: 1 篇, 04-27: 1 篇, 04-28: 12 篇）
- raw/articles/ 总计 89 篇

### 新建页面（3 个）
- entities/multica.md — 多 Agent 协作平台，12K stars/周，Issue 粒度任务分发
- concepts/agent/skill-design-patterns.md — 5 种核心 Skill 设计模式 + 防偷懒技巧 + 3 层知识架构
- concepts/tools/design-md-specification.md — DESIGN.md Markdown 原生设计规范格式

### 更新页面（6 个）
- entities/hermes-agent.md — 新增五层架构、PTC、delegate_task、session chaining、dual-signal skill evolution
- entities/claude-code.md — 新增 Gary Tan YC 高级用法（GStack/Office Hours/对抗性评审）、Subagent 实现（Explore/Plan/Forking）
- concepts/agent/agent-skill-specification.md — 新增 Google Skills 官方标准化、Context Inflation、npm 安装
- concepts/agent/coordination-engineering.md — 新增 Multica 人 A 协作范式、Agent Idle Rate
- concepts/agent/sub-agent-vs-agent-team.md — 新增 Moxt 角色化团队、Claude Code Subagent 实现细节

### 跳过文章（8 篇，不满足建页阈值或信息已被覆盖）
- AI Engineer 学习路径（通用参考）
- 腾讯文档空间 NotebookLM（非核心 Agent 领域）
- Topview 跨境视频（产品评测）
- Moxt Agent 团队（概念已整合到 sub-agent-vs-agent-team）
- OPC 实战经验（商业概念）
- 柚漫剧 AI 流程（内部工具 Zulu）
- 淘天营销工作流（部分信息已整合到 agent-skill-specification）
- 谷歌 Agent Skill 工具箱（已整合到 agent-skill-specification）

## [2026-04-29] ingest | 批量同步 13 篇 Raw 文章 symlink + Wiki 页面创建/更新
- 新增 symlink 13 个（全部为 04-29 文章）
- raw/articles/ 总计 107 篇

### 新建页面（20 个）
- entities/choco.md — AI 食品分销平台，OrderAgent/VoiceAgent
- entities/chi-jianqiang.md — 池建强，MacTalk 作者
- entities/bai-jiajie.md — Harness Engineering 个人实践者
- entities/feng-chengcheng.md — 阿里云专有云 Agent 技术负责人
- entities/yao-liuyi.md — 阿里通义科学家
- entities/qwenpaw.md — 阿里云开源 Agent 产品
- entities/helio.md — AI Workforce 平台
- entities/vitaflow.md — 维塔流动，Life Agent 创业公司
- entities/zhang-xinhao.md — 张心皓，前字节产品高管
- entities/jovida.md — 消费级 Life Agent 产品
- entities/codejunkie99.md — 便携式记忆层 brain 作者
- entities/brain.md — Rust 便携式记忆层项目
- concepts/agent/agent-orchestrator.md — Agent 编排者角色
- concepts/agent/mcp-model-context-protocol.md — MCP 协议概念
- concepts/agent/life-agent.md — 消费级 Life Agent 概念
- concepts/agent/portable-memory-layer.md — 便携式记忆层概念

### 更新页面（12 个）
- entities/anthropic.md — 新增 MCP 战略、批量封禁事件
- entities/openclaw.md — 新增访谈信息，三堵墙框架
- entities/hermes-agent.md — 新增架构对比
- entities/claude-code.md — 新增 AI Coding 安全事件
- entities/deepseek-v4.md — 大幅扩展技术细节（240 行）
- entities/topview.md — 更新 sources
- concepts/agent/agent-technical-debt.md — 新增 PocketOS 案例 + 便携记忆层
- concepts/agent/harness-engineering-deep-dive.md — 新增 JK Launcher 案例 + PocketOS + brain 记忆层
- concepts/agent/harness-engineering-cases-and-evolution.md — 新增 JK Launcher 案例
- concepts/agent/agent-skill-specification.md — 新 sources
- concepts/agent/ai-native-organization.md — 新增 Helio 案例 + Choco 案例 + AI 与内卷分析
- concepts/agent/prompt-context-harness-engineering.md — 新 sources

### 跳过文章
- MCP_2.md — 重复文章
- DeepSeekv4_2.md — 重复文章
- 你不知道的Agent（726 行综合技术文章，信息已部分覆盖；建议后续单独处理）

## [2026-04-29] lint | 知识库健康巡检修复
- 修复 3 个断链：[[cross-border-e-commerce]]（topview.md）、[[jk-launcher]]（bai-jiajie.md）→ 纯文本；[[model-context-protocol]]（ai-engineering.md）→ [[mcp-model-context-protocol]]
- 修复 index.md Total pages 计数（91→95）
- 修复 generic-agent 被 concurrent edit 从 index 中移除的问题
- SCHEMA.md taxonomy 新增 6 个标签：consumer、china-ecosystem、ai-workforce、product-design、memory
- 修复 9 个页面非标准 tags：jovida、yao-liuyi、qwenpaw、bai-jiajie、feng-chengcheng、helio、vitaflow、zhang-xinhao、life-agent
- Total wiki pages: 95（49 entities + 46 concepts）
- Updated: index.md, log.md, SCHEMA.md, 12+ page files

## [2026-04-28] lint | Split hermes-agent.md (223 lines → ≤80) into sub-pages hermes-agent-architecture.md and hermes-agent-self-improving.md

## [2026-04-28] lint | Split agent-skill-specification.md (213 lines → ≤80) into sub-pages agent-skill-evaluation.md, agent-skill-distillation-vs-rag.md, agent-skill-ecosystem.md

- Updated: concepts/agent/agent-skill-specification.md, concepts/agent/agent-skill-evaluation.md, concepts/agent/agent-skill-distillation-vs-rag.md, concepts/agent/agent-skill-ecosystem.md, log.md

## [2026-04-28] lint | Updated index.md with 5 new sub-pages; Total pages 63→68

- Updated: index.md
- Total wiki pages: 68

## [2026-04-28] create | Topview 跨境视频成本文章 — Topview + Seedance 2.0 页面创建
- Source: raw/articles/2026-04-28-topview-video-cost.md (原被跳过，现根据任务重新评估)
- New entity: entities/topview.md — 跨境电商 AI 视频生产工具，整合 Image 2 + Seedance 2.0，Storyboard→Video 工作流，$0.1/秒
- New entity: entities/seedance-2.md — AI 视频生成模型，被 Topview 集成，720p 下 $0.10/秒
- Updated: index.md (新增 2 个条目，Total pages: 71→73)
- Total wiki pages: 73

## [2026-04-28] create | 内容驱动型 OPC 实战经验文章同步（阿星 2050 大会分享）
- Source: raw/articles/2026-04-28-opc-content-driven-experience.md
- New entity page: a-xing.md（阿星，内容驱动型 OPC 实践者，AI 提效培训与自媒体创作者）
- New concept pages: opc-one-person-company.md（OPC 一人公司概念，摊位-飞轮-杠杆三阶段模型）、content-driven-opc.md（内容驱动型 OPC 方法论，双螺旋模型）
- Updated: index.md（新增 3 个条目，Total pages → 71）
- Total wiki pages: 71

## [2026-04-28] create | 柚漫剧 AI 全流程提效拆解文章同步

- Source: raw/articles/2026-04-28-youmanju-ai-workflow.md
- New entity page: entities/youmanju.md（柚漫剧团队，AI 全流程提效实践者，覆盖需求-设计-开发-测试全链路 AI 赋能）
- New concept page: concepts/tools/prompt-friendly-prd.md（Prompt友好型PRD：结构化需求文档方法论，AI 可直接解析和消费）
- New concept page: concepts/agent/aiqa-system.md（AIQA 智能测试体系：主 Agent + 数字员工「度小智」+ 六大专业 Agent，从 Copilot 到 AI Agent 的测试范式转移）
- Updated: index.md（新增 3 个条目，Total pages: 71→74）
- Skipped: 此文章之前在 04-28 批次中被跳过（被标记为「内部工具 Zulu」），经重新评估后确认内容涵盖全流程方法论，满足建页标准
- Total wiki pages: 74

## [2026-04-29] create | MCP 生产级 Agent 集成架构文章同步（池建强/MacTalk）

- Source: raw/articles/2026-04-29_MCP-production-agents.md
- New entity page: entities/chi-jianqiang.md（池建强，MacTalk 作者，墨问创始人，提出 MCP 是 AI 的 UI 非 SDK）
- New concept page: concepts/agent/mcp-model-context-protocol.md（MCP Model Context Protocol，Agent 与外部系统间的标准化连接层，三种集成方式对比、任务单元设计原则、与 Skills 互补关系、行业采用）
- Updated: entities/anthropic.md（新增 MCP 战略和 M×N Integration Problem 章节，添加 mcp 交叉链接）
- Updated: index.md（新增 chi-jianqiang、mcp-model-context-protocol 条目，恢复 calvin 条目，Total pages: 75→76）
- Total wiki pages: 76

## [2026-04-29] ingest | Choco × OpenAI 案例：880 万单零售全部 AI 执行

- Source: raw/articles/2026-04-29_Choco×OpenAI：一年880万单零售，都是AI在执行.md
- New entity page: entities/choco.md（AI 食品分销平台，首家独角兽，OrderAgent + VoiceAgent，8.8M+ 订单/年，200B+ token，1-5% 错误率）
- New concept page: concepts/agent/agent-orchestrator.md（Agent 编排者：不写代码但设计和管理 Agent 的新角色，由 Choco 明确提出）
- Updated: concepts/agent/ai-native-organization.md（新增 Choco 实战案例章节、Agent Orchestrator 角色、Token Maxing 佐证）
- Updated: index.md（新增 choco、agent-orchestrator 条目，Total pages: 77→79）
- Total wiki pages: 79

## [2026-04-29] ingest | 前字节产品高管拿到数千万元 Pre-Seed 轮融资，锦秋、百度风投押注 Life Agent

- Source: raw/articles/2026-04-29_前字节产品高管拿到数千万元Pre-Seed轮融资，锦秋、百度风投押注LifeAgent.md
- New entity page: entities/vitaflow.md（维塔流动，AI 创业公司，前字节高管创立，消费级 Life Agent Jovida，获锦秋/百度风投 Pre-Seed 融资）
- New entity page: entities/zhang-xinhao.md（张心皓，前字节产品高管 / 阶跃星辰 C 端合伙人，维塔流动创始人）
- New entity page: entities/jovida.md（维塔流动旗下消费级 Life Agent 产品，主动式个人生活 AI 助手）
- New concept page: concepts/agent/life-agent.md（消费级个人生活 AI Agent，消除从愿望到行动的摩擦，主动提醒 + 上下文感知）
- Updated: index.md（新增 4 个条目，Total pages: 79→83）
- Total wiki pages: 83

## [2026-04-29] audit | DeepSeek V4 第二篇文章去重检查
- Source 1: raw/articles/2026-04-29-deepseek-v4-guide.md（腾讯程序员，来源: mp.weixin.qq.com/s/MamimcCQj_Hd12T8iFVmKg）
- Source 2: raw/articles/2026-04-29-deepseek-v4-guide-2.md（同一篇文章，与上文完全重复）
- 结论：**完全重复**，无新内容。deepseek-v4.md 已引用该文章，无需更新。

## [2026-04-29] update | AI 破卷杠杆文章 → ai-native-organization
- Source: raw/articles/2026-04-29-ai-lever-or-accelerator.md（曹犟，AI 与内卷三种类型分析）
- Updated: concepts/agent/ai-native-organization.md（新增「AI 与内卷：三种类型与杠杆效应」完整章节，含过剩型/惯性型/组织低效型三种内卷分析、Context Engineering 作为组织能力、AI 作为组织方向放大器；新增 source 引用）
- 文章具足够实质内容补充到现有概念页，但过哲学化，不独立创建实体页
- Updated: index.md（ai-native-organization 条目描述更新）
|- Total wiki pages: 83（页面数量未变，内容已充实）

## [2026-04-29] ingest | AI 删库安全事件 — PocketOS × Cursor 生产事故 + Anthropic 批量封禁

- Source: raw/articles/2026-04-29-ai-delete-database.md
- 事件一：PocketOS 使用 Cursor AI 编程工具时，Agent 在 9 秒内自主删除全部生产数据库及备份。暴露了 AI 安全护栏（安全提示词/Plan Mode）、云平台 API 设计（零确认 GraphQL）、Token 权限管理（超越职责范围）三层面的系统性失败
- 事件二：110 人农业科技公司全员 Claude 账号被批量封禁，管理员无法登录，API 仍在计费，36 小时无回复
- Updated: entities/claude-code.md（新增"AI Coding Safety Incidents"章节，记录 Cursor/Plan Mode 安全漏洞和行业性规律）
- Updated: concepts/agent/agent-technical-debt.md（新增"Real-World Incident: PocketOS Database Deletion"章节，治理/人机回环/集成/上下文四维技术债分析）
- Updated: concepts/agent/harness-engineering-deep-dive.md（新增"Safety Failure Case Study: PocketOS"章节，缺失的 4 个 Harness 层次映射 + 三大系统问题 + 行业启示）
- Updated: entities/anthropic.md（新增"Claude 账号批量封禁事件"章节，暴露组织级熔断机制缺失）
- Updated: index.md（更新 4 个条目描述，Total pages: 91）
- Skipped: PocketOS/Cursor/Railway 均不满足 2 来源实体创建阈值
- Total wiki pages: 91

## [2026-04-30] ingest | 批量同步 4 篇 Raw 文章 symlink + Wiki 页面更新
- 新增 symlink 4 个（全部为 04-30 文章）
- raw/articles/ 总计 111 篇

### 新建页面（1 个）
- entities/sensenova-u1.md — 商汤开源 8B 多模态模型，NEO-Unify 架构直接处理像素，信息图生成接近闭源水平

### 更新页面（5 个）
- entities/claude-code.md — 新增人机协作实践案例小节（零代码构建 VSCode 插件）
- concepts/agent/self-improving-agent.md — 新增上下文保洁模式：洁癖.Skill 章节
- concepts/agent/context-rot.md — 新增上下文保洁实践小节
- concepts/agent/harness-engineering-deep-dive.md — 新增六种核心模式章节（持久化指令文件、作用域上下文组装、分层记忆、做梦整理、渐进式上下文压缩、工作流与编排）
- concepts/agent/prompt-context-harness-engineering.md — 补充 Hermes 五层记忆架构细节

### 跳过文章
- VSCode 插件实践（个人经验，已整合到 claude-code 实践案例）
- 洁癖.Skill（社区 Skill 产品，已整合到 self-improving-agent / context-rot）
- Harness 核心模式（内容已整合到 existing harness pages）
- Total wiki pages: 96 (↑1 新实体)
- Updated: index.md, log.md

## [2026-04-30] lint | 知识库健康巡检修复（第四轮）
- 修复 lovart 出链不足（1→5 wikilinks）：新增 [[seedance-2]]、[[media-generation]]、[[ai-office-automation]]

### 拆分 deepseek-v4.md (240行 → 55行)
- 新建 entities/deepseek-v4-technical.md (179行)：架构/训练/基础设施技术细节
- 新建 entities/deepseek-v4-market.md (68行)：定价竞争/用户迁移/市场影响
- 所有 provenance markers 随内容迁移

### 拆分 claude-code.md (233行 → 68行)
- 新建 entities/claude-code-capabilities.md (134行)：REPL/Tool/1M上下文/ultrareview/产品策略
- 新建 entities/claude-code-practice.md (76行)：Subagent/YC高级用法/Skills生态/VSCode插件案例
- 已有 claude-code-system-prompt.md 未改动

### 拆分 harness-engineering-deep-dive.md (228行 → 55行)
- 新建 concepts/agent/harness-engineering-patterns.md (53行)：六种核心模式
- 新建 concepts/agent/harness-engineering-implementations.md (163行)：工程取向/安全案例/SkVM/Brain
- 已有 harness-engineering-cases-and-evolution.md 未改动
- Updated: index.md, log.md
- Total wiki pages: 102 (↑7 new from splits + 1 new entity)

### 未拆分（仅报告）
- concepts/agent/ai-native-organization.md (201行)：仅超阈值1行，内容内聚，保留不拆分

## [2026-05-01] lint | 知识库健康巡检修复（第五轮）
- 修复 deepseek-v4-technical.md 5 个断链：[[kimi]]（3处）和 [[qwen]]（2处）→ 纯文本引用
- 修复 lovart.md 1 个断链：[[media-generation]] → 纯文本引用
- SCHEMA.md taxonomy 新增 1 个标签：`practice`（实践案例与最佳实践）
- 修复 claude-code-practice.md 非标准 tags：`practice` 已纳入 taxonomy
- 评估 ai-native-organization.md（201行）：仅超阈值1行，内容内聚，保留不拆分
- Updated: deepseek-v4-technical.md, lovart.md, claude-code-practice.md, SCHEMA.md, index.md

## [2026-05-03] lint | 知识库健康巡检修复（第六轮）
- 修复 prompt-context-harness-engineering.md 1 个断链锚点：[[harness-engineering-deep-dive#Brain]] → [[harness-engineering-implementations#Brain：记忆层（Harness 第 2 层）的完整实现]]（Brain 内容已迁移至 implementations 页面）
- Updated: prompt-context-harness-engineering.md

## [2026-05-05] ingest | Batch ingest 05-05 articles (29 files)
- Raw articles linked: 29 files (2026-05-05 batch)
- New entity pages: karpathy, openai, codex, opus-4.7
- New concept pages: vibecoding, agentic-engineering, software-3-0, managed-agents, jagged-intelligence, ai-native-editor
- Updated: index.md (Total pages → 114)
- Note: 29 articles analyzed, ~50 additional entities/concepts identified for future creation (pending more sources or manual review)

## [2026-05-05] lint | 知识库健康巡检修复（第七轮）
- 修正 index.md Total pages 计数：114 → 112（实际文件数）
- 巡检结果：索引一致性 ✅（修正后）、Wikilink 完整性 ✅（3 个锚点链接待验证）、孤儿页面 ✅（0 个）、Schema 合规 ✅（100%）、Raw 覆盖率 5.0%（建设阶段正常）、文件体积 ✅（1 页 201 行，已评估保留）
- Updated: index.md

## [2026-05-06] create | entities/sonar.md
- Source: raw/articles/2026-05-06-ai-code-trust-gap.md（Sonar 2026 开发者代码现状调查报告）
- Content: Sonar 公司概述、SonarQube 产品特性、2026 调查核心数据（72% 日用 AI / 42% AI 代码 / 96% 不信任）、AI 代码信任鸿沟、影子 AI、低效工作转移、LLM 代码质量排行榜
- Tags: company, tool
- Outbound links: agentic-engineering, comprehension-debt, agent-technical-debt, claude-code
- Updated: index.md（112 → 113）

## [2026-05-06] create | concepts/principles/token-roi.md
- Source: raw/articles/2026-05-06-token-roi.md（曹犟《别晒 token 了，先算算 token ROI》）
- Content: Token ROI 概念定义、token 消耗量的误导性分析、ROI 公式（收益/综合成本）、三个判断信号、云计算成本类比、企业三本账框架
- Tags: practice, optimization
- Outbound links: comprehension-debt, agent-technical-debt, prompt-engineering
- Updated: index.md（113 → 114）

## [2026-05-06] create | entities/skillrush-town.md
- Source: raw/articles/2026-05-06-skillrush-town.md
- 淘金小镇（Skillrush Town），开源 Skill，自动挖掘 ClawHub 排行榜数据
- 核心功能：每日抓取 Top100、GitHub Pages 部署、智能筛选规则
- 技术亮点：直接调用 ClawHub 背后的 Convex 数据库 API，绕过浏览器自动化
- Tags: tool, open-source, agent-framework
- Outbound links: agent-skill-ecosystem, skill-evaluator, claude-code
- Updated: index.md（114 → 115）

## [2026-05-06] lint | 知识库健康巡检修复（第八轮）
- 修正 index.md：添加 cursor-team-kit 条目，Total pages 115 → 116
- 10 个页面 tags 字段检查：均已存在有效 tags，无需修改（tags 在健康检查后已由上游流程补充）
- Updated: index.md

## [2026-05-07] ingest | 批量同步 10 篇 Raw 文章 symlink + Wiki 页面创建/更新
- 新增 symlink 10 个（05-06: 1 篇, 05-07: 9 篇）
- raw/articles/ 总计 155 篇

### 新建页面（8 个）
- entities/xunfei-zhiwen.md — 讯飞智文，多 Agent AI PPT 产品，Vision Agent 模式，1000 万+ 用户
- entities/cider.md — 明略科技 MLX 推理加速框架，W8A8/W4A8 量化
- entities/mano-p.md — 明略科技 GUI-VLA Agent，OSWorld #1（58.2%）
- entities/buzzy.md — AI 视频编辑工具（"Video Photoshop"），精确视频二创
- entities/genspark.md — AI 员工平台，$200M ARR，"Harness for humans"理念
- concepts/ai-coding/agents-md.md — AGENTS.md 开放标准，60K+ 项目采用
- concepts/agent/ai-employee.md — AI 员工：2026 企业 AI 新范式
- concepts/agent/private-ai.md — Private AI：全本地化范式

### 更新页面（10 个）
- claude-code-practice（博客功能开发案例 + AGENTS.md 演化）
- vibecoding（Vibe vs Spec Coding 详细对比）
- spec-driven-development（Spec Coding 视角 + AGENTS.md 交叉引用）
- context-rot（实用反腐烂技巧）
- ai-office-automation（讯飞智文 + Buzzy）
- gpt-5.5（Instant 变体详情）
- openai（GPT-5.5 Instant 发布）
- claude-code（创始人观点，低置信度）
- managed-agents（AI 员工赛道背景）
- harness-engineering-deep-dive（AGENTS.md 实践 + Harness for humans 区分）

### 跳过文章
- aicoding-guide-2.md — 与 aicoding-guide.md 完全重复
- claude-code-future.md — 源文件内容过少（31 行），仅更新现有页面

- Total wiki pages: 124（116 + 8 新页面）
- Updated: index.md, log.md


## [2026-05-08] ingest | Batch ingest 05-08 articles (9 files)
- Raw articles linked: 9 files from /Users/felix/work/github/media-conent/raw/
- 2026-05-08-gpt-image2-open-source.md, 2026-05-08-agi-panorama.md, 2026-05-08-ai-us-middle-class-china-manufacturing.md
- 2026-05-08-harness-engineering-90pct-ai-coding.md, 2026-05-08-ai-customer-service-full-stack.md, 2026-05-08-medical-ai-harness-era.md
- 2026-05-08-veteran-developer-agent-journey.md, 2026-05-08-agent-eval-ai-coding-310k-lines.md, 2026-05-08-builderpulse-free-open.md

### 新建实体页面（6 个）
- canghe — AI 博主苍何，开源 awesome-gpt-image-2（4.2K Star）
- zhizhen-tech — 智诊科技，WiseDiag 多模态模型（DoctorBench #1），WiseClaw 2.0
- wiseclaw — WiseClaw 2.0 医疗 Agent OS 平台
- aihot — AIHOT 热点聚合网站
- shuzi-shengming-kazik — 数字生命卡兹克，AI 自媒体博主
- zhiyuanfu — 腾讯程序员，"24h 打工人"系统作者

### 新建概念页面（4 个）
- ai-customer-service — AI 客服系统全栈落地
- ai-workforce-impact — AI 对劳动力市场的非对称冲击
- goal-driven-agent — Goal-Driven vs Task-Driven Agent 设计范式
- agi-paradox — AGI 悖论（哲学/商业/道德三重矛盾）

### 更新页面（10 个）
- gpt-image-2（开源生态 + awesome-gpt-image-2）
- codex（Browser Use + Codex App 实践案例）
- jagged-intelligence（AGI 哲学视角 + BCG 研究）
- harness-engineering-cases-and-evolution（90% AI 代码率案例 + WiseClaw 医疗案例）
- openclaw（WiseClaw 医疗行业应用）
- vibecoding（Vibe Coding 翻车实录 + 先易后难 vs 先难后易）
- spec-driven-development（SDD Agent 调度实践）
- agentic-engineering（脚手架>模型原则 + Observability 六维度）
- agent-technical-debt（31 万行代码重构 + AI 加速腐化）
- builderpulse（同赛道 AIHOT 对比）

- Total wiki pages: 134（124 + 10 新页面）
- Updated: index.md, log.md

## [2026-05-08] lint | 自动修复高优先级问题

### 修复 1：Wikilink 断链（2 个）
- agi-paradox：[[alignment]] → 纯文本"AI 对齐问题（alignment）"（alignment 是 taxonomy tag 无独立页面）
- ai-customer-service：[[ai-coding]] → 纯文本 + 新增 [[claude-code]]、[[codex]] 链接（修复断链同时补充出链）

### 修复 2：文件超过 200 行（1 个）
- ai-native-organization（201 行）→ 拆分为：
  - ai-native-organization（主页面，摘要 + 核心理论，≤100 行）
  - ai-native-organization-cases（子页面，实战案例 + 内卷分析）
  - 两页面互相交叉链接

- Total wiki pages: 135（134 + 1 拆分子页面）
- Updated: index.md, log.md

## [2026-05-09] create | 7 new wiki pages from 05-09 articles

### 新建实体页面（4 个）
- entities/krowork.md — 快手打工人Agent产品，"跑通一次，变成应用"，零token消耗
- entities/kuaishou.md — 快手公司，AI员工赛道"应用固化"路线
- entities/mcp-model-context-protocol.md — MCP协议（实体页，原为index空引用）
- entities/thariq.md — Claude Code团队工程师，HTML输出格式倡导者

### 新建概念页面（3 个）
- concepts/agent/productivity-paradox.md — Agent时代生产力悖论
- concepts/tools/html-agent-output-format.md — HTML作为Agent输出格式
- concepts/ai-coding/ai-engineer-roadmap.md — AI工程师路线图2026

### 索引与日志
- index.md：新增7个条目（krowork, kuaishou, mcp-model-context-protocol移至实体区, thariq, productivity-paradox, html-agent-output-format, ai-engineer-roadmap），移除1个重复mcp条目，Total pages: 135 → 140
- log.md：追加本条目

## [2026-05-09] lint | 自动修复高优先级问题
- 修复断链：[[claude-design]] → 纯文本（2处）
- 修复断链：[[agent-safety]] → 纯文本（1处）
- 修复锚点链接：移除无效 section anchor（3处）
- 修正 index.md Total pages：140 → 142
- 清理重复页面：删除 concepts/agent/mcp-model-context-protocol.md（保留 entities/ 版本）

