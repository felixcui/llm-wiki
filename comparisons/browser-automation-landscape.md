---
title: 浏览器自动化方案全景对比
created: 2026-05-19
updated: 2026-05-19
type: comparison
tags: [tool, agent-framework, comparison, practice]
confidence: medium
sources: []
---

# 浏览器自动化方案全景对比

浏览器自动化从最早的测试驱动工具，演进到如今专为 AI Agent 设计的智能操作层，已经形成一个完整的技术生态。本文按技术路线和适用场景分类梳理。

## 一、技术路线分类

浏览器自动化的核心技术路线有四条：

| 路线 | 原理 | 代表工具 |
|------|------|----------|
| **CDP（Chrome DevTools Protocol）** | 直接通过 WebSocket 控制浏览器内核 | Puppeteer, Playwright, BrowserUse |
| **WebDriver（W3C 标准）** | 通过 HTTP 协议与浏览器 Driver 通信 | Selenium, Appium |
| **视觉/截屏方案** | 截图后用 AI 视觉模型理解页面，模拟点击坐标 | Anthropic Computer Use, OmniParser |
| **混合方案** | CDP + 视觉 + DOM 解析结合 | Stagehand, Browser Use (Python) |

## 二、传统测试驱动工具

### Selenium（2004—）

- **类型**: 开源，WebDriver 协议鼻祖
- **语言**: Java/Python/JS/C#/Ruby 等
- **原理**: W3C WebDriver 协议，每个浏览器需要独立 Driver（ChromeDriver、GeckoDriver 等）
- **优势**: 生态最成熟，支持所有主流浏览器，企业级稳定
- **劣势**: API 较底层，速度慢于 CDP 方案，无原生 AI Agent 支持
- **适用**: 传统自动化测试、爬虫、CI/CD 流水线
- **GitHub Stars**: 31K+

### Puppeteer（2017—）

- **开发者**: Google Chrome 团队
- **类型**: 开源，Node.js
- **原理**: Chrome DevTools Protocol (CDP)
- **优势**: 直接控制 Chrome，速度快，API 现代，支持无头模式
- **劣势**: 仅 Chrome/Chromium，生态不如 Selenium 广
- **适用**: Chrome 专用自动化、PDF 生成、爬虫
- **GitHub Stars**: 90K+

### Playwright（2020—）

- **开发者**: Microsoft（原 Puppeteer 团队成员创建）
- **类型**: 开源，Node.js/Python/Java/.NET
- **原理**: CDP + 自研协议层（支持 Chromium/Firefox/WebKit）
- **优势**: 跨浏览器（三大引擎）、自动等待、网络拦截、Trace Viewer、Codegen（录制生成代码）
- **劣势**: 比 Puppeteer 稍重，资源占用较高
- **适用**: E2E 测试、跨浏览器自动化、AI Agent 的底层执行层
- **GitHub Stars**: 70K+
- **关键**: Playwright 已成为多数 AI 浏览器工具的底层引擎

## 三、专为 AI Agent 设计的浏览器工具

### Browser Use（Python，browser-use）

- **GitHub**: https://github.com/browser-use/browser-use
- **GitHub Stars**: 60K+
- **类型**: 开源（Apache 2.0）
- **技术**: Playwright + LLM 视觉理解 + DOM 解析
- **核心理念**: 让 LLM Agent 能像人一样浏览和操作网页
- **工作方式**: Agent 看到页面截图 + DOM 树精简版 → LLM 决策下一步操作 → Playwright 执行
- **特点**:
  - 支持 GPT-4o、Claude、Gemini 等多种 LLM
  - 自动处理 cookie/popup/登录
  - 支持多 Tab 并行
  - 可自定义 Action 空间
- **适用**: AI Agent 网页任务自动化、数据采集、表单填写

### Stagehand（Fabrication）

- **GitHub**: https://github.com/fabrication/stagehand
- **类型**: 开源
- **技术**: Playwright + AI 自然语言指令
- **核心理念**: "浏览器自动化，但用自然语言"
- **API 示例**: `stagehand.page.act("click the login button")` / `stagehand.page.extract("get all product prices")`
- **特点**:
  - 三种原语：`act`（操作）、`extract`（提取）、`observe`（观察）
  - 底层用 Playwright 执行，LLM 负责将自然语言映射为具体操作
  - 支持 Chrome MCP Server 模式
- **适用**: 用自然语言驱动浏览器操作，适合 Agent 集成

### Playwright MCP Server

- **开发者**: Microsoft / 社区
- **类型**: 开源
- **技术**: Playwright + MCP（Model Context Protocol）
- **核心理念**: 将浏览器能力暴露为 MCP Tool，任何支持 MCP 的 Agent 可直接调用
- **工作方式**: Agent 通过 MCP 协议调用 `browser_navigate`、`browser_click`、`browser_snapshot` 等工具
- **特点**:
  - 标准化接口，不绑定特定 LLM
  - 支持 accessibility tree 快照（比截图更高效）
  - Claude Desktop / Hermes 等已原生支持
  - 轻量，无需额外模型推理
- **适用**: MCP 生态内的 Agent 浏览器交互（当前最主流的 Agent 浏览器方案）

### BrowserUse（hixuanxuan/browser-automation，Skill 形态）

- **类型**: 开源 Skill
- **Wiki 页面**: [[browseruse]]
- **技术**: CDP 脚本集 + Claude Code Skill 封装
- **核心理念**: 为 Agent 构建 Runtime Harness，验证 Web 页面的正确性
- **特点**:
  - 六维验证框架（路径/内容/视觉/交互/控制台/网络）
  - Contract 模式断言
  - 带标注截图 + VET 覆盖层
  - 渐进式笔记机制
- **适用**: AI Agent 驱动的 Web 前端验证和 QA
- **详见**: [[browseruse]]

## 四、云端浏览器基础设施

### Browserbase

- **网站**: https://browserbase.com
- **类型**: 商业 SaaS
- **核心理念**: "Headless Browser as a Service"
- **特点**:
  - 云端运行 Chrome 实例，提供 HTTP API
  - 内置代理、反检测、Session 管理
  - 支持 CDP 远程连接
  - 按 Session/分钟计费
  - 专为 AI Agent 场景优化
  - 提供 `connect()` SDK，Playwright/Puppeteer 原生兼容
- **适用**: 生产级 Agent 浏览器任务，避免本地环境依赖

### Steel（原 MultiOn）

- **网站**: https://steel.dev
- **类型**: 商业 SaaS + 开源 SDK
- **核心理念**: 浏览器云基础设施，为 Agent 提供远程浏览器环境
- **特点**:
  - 云端浏览器 + Session 持久化
  - 内置反检测指纹
  - API 控制浏览器生命周期
  - 与 Browser Use / Playwright 集成
- **适用**: 大规模 Agent 浏览器任务的云端执行

### Browserless

- **网站**: https://browserless.io
- **类型**: 商业 SaaS + 开源
- **核心理念**: 无头浏览器即服务
- **特点**:
  - 云端 Chrome/Chromium 实例
  - 支持 Puppeteer/Playwright 远程连接
  - PDF 生成、截图、爬虫 API
  - 按并发/请求计费
- **适用**: PDF 生成、截图、基础爬虫自动化

## 五、AI 视觉方案（Computer Use）

### Anthropic Computer Use

- **类型**: Claude 模型内置能力
- **技术**: 视觉理解 + 坐标点击
- **工作方式**: 截屏 → Claude 视觉理解屏幕内容 → 输出鼠标/键盘操作坐标 → 系统执行
- **特点**:
  - 不依赖 DOM/HTML 解析，纯视觉理解
  - 可以操作任何桌面应用（不只是浏览器）
  - 当前精度有限，复杂布局可能误操作
  - Claude 3.5 Sonnet 首次引入，后续模型持续优化
- **适用**: 桌面级通用自动化，非标准界面操作

### OpenAI Operator

- **类型**: ChatGPT 内置功能
- **技术**: GPT-4o 视觉 + CUA（Computer Using Agent）模型
- **工作方式**: 用户描述任务 → Operator 打开浏览器执行 → 视觉理解 + 操作
- **特点**:
  - 消费者友好的 UI，嵌入 ChatGPT
  - 可完成购物、订餐、填表等日常网页任务
  - 目前仅限美国部分用户
- **适用**: 消费级网页任务自动化

### Google Mariner

- **类型**: Google 实验性项目
- **技术**: Gemini 视觉 + Chrome 扩展
- **工作方式**: 在 Chrome 浏览器中直接操作，理解页面语义
- **适用**: 实验性探索

## 六、方案对比矩阵

| 工具 | 类型 | 技术路线 | AI 集成 | 浏览器支持 | 开源 | 适用场景 |
|------|------|----------|---------|------------|------|----------|
| **Selenium** | 测试框架 | WebDriver | ❌ | 全部 | ✅ | 传统自动化测试 |
| **Puppeteer** | 测试框架 | CDP | ❌ | Chrome | ✅ | Chrome 自动化 |
| **Playwright** | 测试框架 | CDP+协议 | ❌ | 三大引擎 | ✅ | E2E 测试/底层引擎 |
| **Browser Use** | Agent 工具 | Playwright+视觉 | ✅ | Chrome | ✅ | Agent 网页操作 |
| **Stagehand** | Agent SDK | Playwright+NL | ✅ | Chrome | ✅ | 自然语言浏览器操作 |
| **Playwright MCP** | MCP Server | CDP | ✅（MCP） | 三大引擎 | ✅ | Agent 浏览器交互 |
| **Browserbase** | 云服务 | CDP | ✅ | Chrome | ❌ | 生产级云端浏览器 |
| **Steel** | 云服务 | CDP | ✅ | Chrome | 部分 | 大规模 Agent 浏览器 |
| **Browserless** | 云服务 | CDP | ❌ | Chrome | ✅ | PDF/截图/爬虫 |
| **Computer Use** | 模型能力 | 视觉 | ✅ | 全桌面 | ❌ | 桌面级通用自动化 |
| **Operator** | 消费产品 | 视觉+CUA | ✅ | Chrome | ❌ | 消费级网页任务 |
| **BrowserUse Skill** | Skill | CDP | ✅ | Chrome | ✅ | Web 前端验证 QA |

## 七、选型建议

### 如果你是 Agent 开发者

- **轻量交互**: Playwright MCP Server — 标准化、低成本、生态兼容
- **复杂网页任务**: Browser Use（Python）— 视觉+DOM 双通道，处理复杂页面
- **自然语言驱动**: Stagehand — API 最直觉
- **Web QA 验证**: BrowserUse Skill — 六维验证框架
- **生产部署**: Browserbase/Steel — 云端运行，免本地环境

### 如果你是测试工程师

- **通用 E2E**: Playwright — 跨浏览器，生态最好
- **已有 Selenium 投资**: 继续 Selenium，逐步迁移
- **Chrome 专用**: Puppeteer 更轻量

### 如果你是终端用户

- **日常网页任务**: OpenAI Operator / Claude Computer Use
- **不想写代码**: Operator 或类似消费产品

## 八、趋势与展望

1. **MCP 成为标准接口** — 浏览器能力通过 MCP Tool 暴露给 Agent，正在成为事实标准
2. **视觉方案精度提升** — Computer Use 类方案从实验走向生产，但 CDP 方案在精确操作上仍有优势
3. **云端浏览器基础设施化** — Browserbase/Steel 等将浏览器变成可调用的 API，降低 Agent 部署门槛
4. **混合方案成为主流** — 单纯视觉或单纯 DOM 都不够，两者结合（Browser Use/Stagehand 模式）是当前最优解
5. **从测试到 Agent** — 浏览器自动化从 QA 工具进化为 Agent 的「眼睛和手」，是 2025-2026 年最显著的趋势转变

## 相关页面

- [[browseruse]] — BrowserUse 实体页面（Skill 形态的浏览器验证工具）
- [[harness-engineering-deep-dive]] — Harness Engineering 深度解析
- [[agent-first-design]] — Agent First Design：软件交互从 UI 转向 API/MCP
- [[mcp-model-context-protocol]] — MCP 协议详解
- [[html-agent-output-format]] — HTML 作为 Agent 输出格式
