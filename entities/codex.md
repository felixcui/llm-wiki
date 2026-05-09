---
title: OpenAI Codex
created: 2026-05-05
updated: 2026-05-08
type: entity
tags:
  - tool
  - ai-coding
  - agent-framework
sources:
  - raw/articles/2026-05-05-openai-codex-mac-takeover.md
  - raw/articles/2026-05-05-codex-pet-goal.md
  - raw/articles/2026-05-05-codex-game-development.md
  - raw/articles/2026-05-08-gpt-image2-open-source.md
---

# OpenAI Codex

OpenAI Codex 从代码生成 CLI 工具进化为通用 AI Agent，是 [[openai]] 2026 年最重要的产品。

## 演进历程

### 阶段一：代码生成 CLI
初始定位为终端内的代码生成工具，类似 [[claude-code]]。

### 阶段二：通用 Mac Agent（2026）
重大升级，能力突破代码范畴：
- **操控整台 Mac**：操作 Adobe Creative Suite、Slack、Google Workspace 等任意桌面应用
- **Codex App**：独立桌面应用，不限于终端
- **/pet 虚拟宠物**：持久化 Agent 实例，具有"个性"和记忆
- **/goal 长期目标追踪**：设定目标后 Agent 自主规划、执行、监控进度

### 阶段三：游戏开发 Agent
展示 Codex 在游戏开发场景中的自主编程能力。

## 核心能力

- 底层模型：[[gpt-5.5]]
- 代码生成与编辑
- 桌面应用操控（GUI Agent）
- 长期任务规划与执行
- 持久化上下文与记忆

## Browser Use 与 Computer Use

Codex App 在浏览器自动化和电脑控制方面表现突出：^[raw/articles/2026-05-08-gpt-image2-open-source.md]
- **Browser Use**：可自动打开网页、搜索信息、浏览内容、提取数据，例如自动从 X（Twitter）搜集 GPT-Image-2 的提示词帖
- **Computer Use**：可控制本地应用程序，在 Mac Mini 等设备上实现完全访问模式
- **插件生态**：支持 GitHub、Vercel、Browser Use、Computer Use 等插件，或直接使用本地 CLI 工具
- **远程控制**：可通过手机/iPad 远程连接 Mac Mini 上的 Codex 实例，实现 24 小时不间断工作
- **自动化工作流**：支持设置规则（如"10 分钟内不确定则自行决策"），结合 GitHub + Vercel 实现自动部署

## 实践案例

博主 [[canghe]] 使用 Codex App 在医院陪护期间，仅通过手机远程指挥完成了以下工作：^[raw/articles/2026-05-08-gpt-image2-open-source.md]
1. 搭建 GPT-Image-2 提示词开源项目（awesome-gpt-image-2）
2. 开发配套可视化网站（gpt-image2.canghe.ai）
3. 建立 Prompt 自动搜集与分类发布工作流
4. 重构个人网站 Wesight
5. 创建 CodexGuide 开源教程项目

## 竞争对比

| 特性 | Codex | Claude Code |
|------|-------|-------------|
| 终端集成 | ✅ | ✅ |
| 桌面操控 | ✅（原生） | 有限 |
| 持久化 Agent | /pet | 无等价功能 |
| 长期目标 | /goal | 手动管理 |
| 底层模型 | GPT-5.5 | Claude Opus 4.7 |

## 相关链接

- [[openai]] — 开发公司
- [[gpt-5.5]] — 底层模型
- [[claude-code]] — 直接竞品
- [[anthropic]] — 竞品公司
