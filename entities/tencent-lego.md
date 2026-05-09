---
title: 腾讯 CDN LEGO
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [company, open-source, ai-coding]
sources:
  - raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md
---

# 腾讯 CDN LEGO

**类型**: 超大规模分布式后端系统
**所属**: 腾讯 CDN 核心接入层
**技术栈**: C++（主系统）、Rust（Nonstop 代理框架）
**代码规模**: 核心代码 100 万行 + 深度改造第三方库 300 万行

## 概述

LEGO 是腾讯 CDN 和 EdgeOne 业务的核心接入层系统，承载亿级用户流量，承担流量调度、协议解析、安全防护、缓存加速等关键职责。它是 [[prompt-context-harness-engineering]] 在企业级超大规模系统中最完整的落地案例，通过五层 Harness 架构和多模型对抗式代码审查，为 AI 编码在高风险后端场景建立了从生成到上线的完整质量屏障。^[raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md]

## 系统复杂性

LEGO 面对的不是确定性输入输出，而是不可控的客户端（浏览器、App、爬虫、攻击工具，数十亿设备）、不可控的源站（数百万域名）、多协议并存（HTTP/1.1、HTTP/2、HTTP/3/QUIC、WebSocket、TLS），维度组合路径高达 13,824 × N 种。一行代码失误即可引发全网事故。

## AI Coding 能力验证：Nonstop 项目

团队先用 20 天由 1 人 + AI 完成 Rust 版 Nonstop 代理框架，验证 AI 编码能力边界：
- 功能：L4/L7 代理、HTTP/3 QUIC、内置 WAF、V8 JS Workers 边缘计算
- 性能：42,052 QPS / 5000 并发 0 错误 / P50 延迟 1.1ms
- 成果的同时暴露了 13 类典型问题和 5 大根因^[raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md]

## 五层 Harness 架构

围绕"上下文、约束、反馈"三大核心要素构建：^[raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md]

### 上下文建设
- 四层递进上下文体系：Agent.md（项目宪法）→ 安全纪律（反例免疫）→ 领域知识（可复用模式库）→ 专业 Skill
- 将 38,068 行 RFC 原文固化在本地，AI 通过直接读取而非"回忆"引用协议标准
- 建立竞品调研 Agent 团队和协议安全测试 Agent 团队，实现自动化、结构化、并行化调研

### 约束体系
- 三层约束架构：权限安全基座 → 代码规则即编译器 → 流程约束（测试不可跳过）
- 五条核心约束均来自真实踩坑（单项目调研、严禁网络操作、本地不存在则跳过、不修改核心代码、严格搜索范围）
- 明确约束比模糊期望更有效：`"禁止裸 new，必须 unique_ptr"` AI 100% 遵循 vs `"写高质量代码"` AI 理解模糊

### 反馈闭环
- 三条并行反馈通道：自动采集（Hook）、踩坑日志（Pitfall Journal）、CLAUDE.md 内联反馈
- 踩坑 → 规则 → Skill 的进化闭环：PIT-001(mmap nullptr→SIGSEGV) → 写入 R2 规则 → AI 自动使用 MAP_FAILED
- 单命令驱动的 9 阶段全自动流水线（需求→设计→实现→单元测试→集成测试→代码审查→部署→监控→复盘）

## 多模型对抗式代码审查

参见 [[multi-agent-code-review]]。LEGO 使用 cr_claude、cr_codex、cr_gemini 三个 Reviewer 并行独立审查，cr_manager 汇总出 cr_report.md，通过交叉验证解决单模型的知识盲区、注意力盲区和确认偏差三大问题。^[raw/articles/2026-04-23_HarnessEngineering：AI能在真正出事会炸的后端系统里写代码吗？.md]

## 实践成果

| 维度 | 提升幅度 |
|------|---------|
| 竞品调研 | 3 人天 → 1 天（~3x） |
| 方案设计 | 2-3 人天 → 1 天（~2x） |
| 协议安全测试 | 3-5 人天 → 1 天（~4x） |
| 代码审查 | 等待 1-3 天 → 30 分钟 |
| cpplint 通过率 | >95% |
| CVE 防护覆盖 | 100% |

知识资产：86,422 行代码、31 个 [[agent-skill-specification|Skill]]、34 条踩坑规则、4 竞品并行调研、3 组 A/B 实验。综合效率提升约 20%（含 Review 成本和学习曲线）。

## 已知挑战

- 误报率 36%（9 个代码问题中真实 P0 仅 1 个）
- 文档爆炸（8 个需求生成 99 个文件）
- AI 的"自信"会传染（格式工整的文档反而降低审查意愿）
- 团队能力退化风险（AI 用多了，工程师专业能力可能下滑）
