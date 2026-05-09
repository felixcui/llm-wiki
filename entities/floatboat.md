---
title: FloatBoat
created: 2026-04-24
updated: 2026-04-24
type: entity
tags: [company, tool, open-source]
sources:
  - raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md
---

# FloatBoat

**创始人**: 少卿
**类型**: Agent 基础设施创业公司
**产品**: Floatboat（Agent 工作站客户端）、[[floatim]]（Agent 即时通信网络）
**官网**: [im.floatboat.ai](https://im.floatboat.ai)

## 概述

FloatBoat 是一家专注于 Agent 基础设施的创业公司，由创始人少卿创立。其核心理念是"把人类的基础设施改造成 Agent 能理解的环境"。团队是一个核心成员都经历了完整 AI 技术链条的团队。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## 产品矩阵

### Floatboat（Agent 工作站客户端）

Floatboat 是一款 Agent 工作站客户端，将整台电脑打造为 Agent 的运行环境。Agent 可以看到用户能看到的一切、使用用户能使用的一切。作为创造者使用，用户可以在自己的电脑中创建 Agent 的行动空间，运行多个 Agent。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

### [[floatim]]（Agent 即时通信网络）

Floatboat 的网络版，是 Agent 的即时通信工具。与 Floatboat 形成"工作站客户端+网络版"关系，类似剪映与抖音、微信电脑版与微信网页版。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## 开源项目

FloatBoat 开源了两个 Agent 基础设施协议（均采用 MIT 协议）：

- **Selfware 文件协议**：Agent 专属文件协议，实现 Agent 之间的上下文数据交换
- **IACT 协议**：Agent 与人的沟通协议，可交互的文本格式^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## 产品理念

### 低技术高体验

团队强调"低技术高体验"的理念——实现起来可能不难，但设计是难的。在强大 Agent 运行环境下，想法和产生想法的人跟环境往往是最重要的。团队为此做了大量基建来驾驭模型概率分布与产品设计之间的偏差。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

### Chat-as-OS

少卿远在各类"Terminal + 聊天软件"形态涌现之前，就已完成了"Chat-as-OS"及"预见 UI (No-UI)"等核心产品理念的系统性理论构建，团队早在 2025 年 7 月就完成了原型构建。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## 与其他 Agent 框架的关系

FloatBoat 的定位不同于 [[claude-code]]（编码 Agent）、[[hermes-agent]]（自进化 Agent 框架）和 [[generic-agent]]（极简 Agent 框架），它更偏重 Agent 的社交网络和通信基础设施。
