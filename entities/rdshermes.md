---
title: RDSHermes
created: 2026-04-24
updated: 2026-04-24
type: entity
tags: [tool, agent-framework, company]
sources:
  - raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md
---

# RDSHermes

**类型**: Hermes Agent 托管版本（阿里云）
**平台**: 阿里云 RDS AI 应用市场
**基于**: [[hermes-agent]]

## 概述

RDSHermes 是 [[hermes-agent]] 的阿里云托管版本，将 Hermes 的自进化能力包装为开箱即用的服务，面向不写代码的团队成员。核心理念：开源 Hermes 是给开发者的引擎，RDSHermes 是给整个团队的成品车。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## 核心能力

### 数据库安全纳管

支持 MySQL、PostgreSQL、SQL Server、MariaDB 多引擎一键接入，密码提交瞬间加密。可设只读模式——Agent 能查但不能改，生产环境安全有底线。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

### 身份认证托管

AK/SK 加密托管，Agent 调用云 API 时由网关代理鉴权，密钥不暴露给 Agent 也不暴露给用户。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

### Skill Hub 预装技能

预装智能巡检、慢 SQL 诊断、索引优化等数据库专业技能。解决开源版 Hermes 的冷启动问题——Agent 上线第一天就具备领域能力。DBA 说一句"帮我巡检一下 prod-mysql"，Agent 连着库做真实分析。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

### 全链路监控审计

写操作需二次确认才执行，会话可追溯，Token 消耗可监控，安全事件有告警。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## 与开源 Hermes 对比

| 维度 | 开源 Hermes | RDSHermes |
|------|------------|-----------|
| 开始使用 | 命令行安装，手写 config.yaml | 控制台一键开通，零配置 |
| 对话界面 | 终端 CLI | 内置 WebUI |
| 接入 IM | config.yaml 配凭证 | 控制台填 App ID |
| 数据库连接 | 手动配连接串，密码明文 | 一键接入，密码自动加密 |
| 云凭证管理 | AK/SK 写进环境变量 | 加密托管，网关代理鉴权 |
| 技能管理 | Agent 自动创建，磁盘文件 | Skill Hub 预装专业技能 |

^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## 组织级进化

开源版 Hermes 的 Skill 存储在本地 `~/.hermes/` 目录下，经验积累是单点的。RDSHermes 将 Skill 存储搬到云端——一个 DBA 踩过的坑，团队所有人的 Agent 都能绕过。Skill Hub 解决冷启动，自进化解决越用越强。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## 迁移支持

支持从 [[openclaw]] 一条命令（`hermes claw migrate`）导入全部配置和记忆数据，平滑切换。^[raw/articles/2026-04-24_深入源码：HermesAgent如何实现Self-Improving.md]

## 与 [[self-improving-agent]] 的关系

RDSHermes 保留了 Hermes 的完整 Self-Improving 闭环（Memory + Skill + Nudge Engine），同时通过 Skill Hub 和组织级 Skill 存储实现了团队级别的经验共享。
