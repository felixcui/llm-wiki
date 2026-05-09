---
title: Cursor Team Kit
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [tool, ai-coding, open-source]
sources:
  - raw/articles/2026-05-06-cursor-team-kit.md
---

# Cursor Team Kit

Cursor Team Kit 是 Cursor 团队将其**内部开发流程**打包而成的官方插件，在 [[ai-native-editor|Cursor]] 中通过 `/add-plugin cursor-team-kit` 即可即插即用，无需第三方服务集成。^[raw/articles/2026-05-06-cursor-team-kit.md]

该套件包含 **18 个技能、2 个规则和 1 个代理**，覆盖 CI 监控与修复、PR 评审与清理、代码去杂乱等全流程开发场景，旨在帮助开发者直接复用 Cursor 团队内部的高效工作流。

## 组件列表

### 技能（Skills）

| 技能 | 描述 |
| --- | --- |
| `loop-on-ci` | 监控 CI 运行并迭代修复失败，直到检查通过 |
| `review-and-ship` | 运行结构化评审、提交更改并打开 PR |
| `pr-review-canvas` | 生成交互式 HTML PR 步骤指导，带注释分类 diff |
| `verify-this` | 使用基线/处理证据证明或否让主张 |
| `control-cli` | 构建或适配本地驱动器来驱动交互式 CLI 或 TUI |
| `control-ui` | 构建或适配本地浏览器/CDP 驱动器处理 Web 或 Electron UI |
| `make-pr-easy-to-review` | 清理杂乱 PR 历史，改进描述并添加评审指导 |
| `run-smoke-tests` | 运行 Playwright 热身测试并分类失败 |
| `fix-ci` | 查找失败 CI 任务，检查日志并应用重点修复 |
| `new-branch-and-pr` | 创建新分支、完成工作并打开 PR |
| `get-pr-comments` | 获取并总结活跃 PR 的评审评论 |
| `check-compiler-errors` | 运行编译和类型检查命令并报告失败 |
| `what-did-i-get-done` | 总结指定时间段内的提交，生成简洁状态更新 |
| `weekly-review` | 生成周度回顾，高亮 bug 修复/技术债/新功能 |
| `fix-merge-conflicts` | 解决合并冲突，验证构建/测试并总结决策 |
| `deslop` | 移除 AI 生成的代码杂乱并清理代码风格 |
| `workflow-from-chats` | 从聊天中提取持久工作偏好到技能、规则或文档 |

### 规则（Rules）

| 规则 | 描述 |
| --- | --- |
| `typescript-exhaustive-switch` | 要求对联合类型/枚举进行穷尽切换处理 |
| `no-inline-imports` | 保持 import 在模块顶层，确保可读性和一致性 |

### 代理（Agents）

| 代理 | 描述 |
| --- | --- |
| `ci-watcher` | 监控 GitHub Actions 运行并返回简洁的通过/失败总结 |

## 安装方式

在 Cursor 中直接输入命令：

```
/add-plugin cursor-team-kit
```

插件地址：https://cursor.com/marketplace/cursor/cursor-team-kit

## 与同类工具对比

Cursor Team Kit 与 [[claude-code]] 的社区 Skills 生态（如 [[superpowers]]、[[spec-kit]]）定位类似，都是为 AI 编程工具提供预构建的工作流扩展。核心差异在于：

- **来源不同**：Team Kit 是 Cursor 团队内部流程的官方外化；[[superpowers]] 和 [[spec-kit]] 则是社区/第三方开源项目
- **平台绑定**：Team Kit 仅限 Cursor 使用；[[claude-code]] 的 Skills 生态仅限 Claude Code
- **侧重点**：Team Kit 侧重 CI/CD 流程自动化和代码质量治理；[[superpowers]] 侧重强制工程纪律（Process over Prompt）；[[spec-kit]] 侧重规范先行（Spec-First）

三者共同反映了 AI 编程工具领域的一个趋势：**通过可复用的工作流插件（Skills/Rules/Agents）来提升 AI 编程的工程化水平**。

## 相关页面

- [[ai-native-editor]] — Cursor 所属的编辑器范式
- [[claude-code]] — Claude 的 AI 编码 Agent，拥有类似的 Skills 生态
- [[superpowers]] — Claude Code 的社区 Skills 包
- [[spec-kit]] — GitHub 官方的 AI 编程工具包
