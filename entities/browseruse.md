---
title: BrowserUse
created: 2026-05-15
updated: 2026-05-15
type: entity
tags: [tool, agent-framework, open-source, practice]
sources:
  - raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md
---

# BrowserUse

**类型**: 开源浏览器自动化工具（75K+ stars）
**技术栈**: Chrome DevTools Protocol (CDP)
**GitHub**: https://github.com/hixuanxuan/browser-automation

## 概述

BrowserUse 是一套面向 Agent 的浏览器 Runtime [[harness-engineering-deep-dive|Harness]]，核心理念是让 Agent 具备感知和操作浏览器的能力。Web 界面的正确性不是代码的静态属性，而是运行时的组合结果——组件代码、CSS cascade、运行时数据、容器尺寸、异步状态共同作用。Agent 只看代码无法判断最终渲染效果，必须让 Agent "看到"浏览器。^[raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md]

## 核心设计原则

**只有网页内容才是唯一信源，代码只是参考。** 所有断言的依据必须来自 CDP 获取的 computed style 或 `getBoundingClientRect()` 返回的实际尺寸，而不是源代码中的声明值。^[raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md]

## 六维验证框架

BrowserUse 将浏览器验证分成六个维度：^[raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md]

| 维度 | 验证内容 | 说明 |
|------|---------|------|
| **路径** | 导航通畅性 | 登录→导航→目标页面，中间无报错 |
| **内容** | 元素齐全性 | 列表渲染、标题文案、按钮可见性，用 Contract 断言 |
| **视觉** | 布局正确性 | 错位、换行、间距、截断等，必须截图才能发现 |
| **交互** | 操作响应 | 弹窗、表单提交、Tab 切换后内容更新 |
| **控制台** | 运行时异常 | JS error、React 渲染报错、资源加载失败 |
| **网络** | 接口正常 | 请求状态码、响应数据，定位前端/后端问题 |

## 技术架构

### CDP 环境

通过 Chrome DevTools Protocol (CDP) 作为核心通道，支持三种运行形态：

- **带界面 Chrome** — 开发机，调试直观
- **Headless Chrome** — CI/服务器，资源省
- **NoVNC 方案** — 远程容器 + Web 远程桌面，协同调试

### 核心工具集

```
navigate.mjs    — 页面导航
click.mjs       — 点击元素
fill.mjs        — 填写输入框
wait.mjs        — 等待元素出现
screenshot.mjs  — 截取页面/元素截图
eval.mjs        — 执行 JavaScript
get-text.mjs    — 读取元素文本
get-html.mjs    — 读取元素 HTML
console-check.mjs — 收集 console 输出和异常
```

### Tab 隔离约束

**一个对话独占一个 Tab。** 多个 Agent 对话同时运行时，通过 `--tab <id>` 精确指定 Tab，避免争抢和竞态。所有工作流开头固定 Tab ID，整个会话期间不变。^[raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md]

## 工作流

### Contract 模式

用 JSON 定义 Checkpoint，Agent 按用例逐条执行。用例分阶段构建：先探索页面→建立认知→记录笔记→写第一批 Checkpoint→验证→补充调整。^[raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md]

### 辅助验证手段

- **带标注截图** — `annotate-screenshot.mjs` 在截图上标红框、绿框和测量数据
- **VET 覆盖层** (Visual Element Tree) — 按 DOM 深度给内容元素覆盖语义色块，对比标准/开发页面
- **DOM 层级识别** — 批量查询可见元素信息，构建"页面结构快照"

## 两种实践方案

| 方案 | 特点 | 成本 | 适用场景 |
|------|------|------|---------|
| **重 QA** | 独立 Inspector Subagent 对抗式测试，产出 report.md | ~300 RMB/任务 (Sonnet 4.6) | 关键功能上线前 |
| **快测试** | 验证嵌入编码过程，边写边测，Checkpoint 驱动 | 远低于方案 1 | 日常开发迭代 |

### 重 QA 实例

"评论区快捷短语面板"任务中，Inspector 跑了三轮：第一轮发现面板被 `overflow: hidden` 裁剪、Modal 不关闭；第二轮确认裁剪修复（改用 `createPortal`）；第三轮全部通过。^[raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md]

## 渐进式笔记

Agent 持续维护 `visual-notes.md`，记录可靠的 selector、失败的 selector、页面状态转换时机。引入笔记机制后，后半程验证失败率明显降低，Token 消耗更可控。^[raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md]

## 与 Harness Engineering 的关系

BrowserUse 代表 [[harness-engineering-deep-dive|Harness Engineering]] 在 Web 前端领域的必然延伸：

- Agent 写代码只是任务的一半，确保代码在 Runtime 中正确运行才是完整交付
- CDP 脚本、截图能力、标注工具、DOM 断言框架是可跨项目复用的基础设施
- 成本是当前最大挑战——视觉验证天然高成本，需持续探索平衡点

^[raw/articles/2026-05-14_BrowserUse：为Agent构建RuntimeHarness.md]

## 参考

- GitHub: https://github.com/hixuanxuan/browser-automation
- 安装: `npx skills add hixuanxuan/browser-automation --skill visual-verify`
