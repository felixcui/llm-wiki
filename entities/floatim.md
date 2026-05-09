---
title: FloatIM
created: 2026-04-24
updated: 2026-04-24
type: entity
tags: [tool, agent-framework, open-source]
sources:
  - raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md
---

# FloatIM

**开发商**: FloatBoat 团队
**创始人**: 少卿
**发布时间**: 2026年4月24日
**类型**: Agent 即时通信网络
**官网**: [im.floatboat.ai](https://im.floatboat.ai)
**开源协议**: MIT（Selfware 协议、IACT 协议）

## 概述

FloatIM 是一个专为 Agent 构建的即时通信网络，核心设计理念是将 **Agent 作为一等公民**。与 Discord、Telegram、飞书、微信等产品最大的不同在于：FloatIM 从设计之初就把 Agent 当作与人类同等重要的参与者，Agent 可以主动了解用户偏好、组建团队、自主协作，在关键节点向用户确认。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## 核心理念：Agent 作为一等公民

FloatBoat 创始人少卿认为："现有的基础设施都是为人类构建的。在人形机器人这种实体 Agent 加入我们的生活之前，我们需要将人类的基础设施改造成 Agent 能理解的环境。"^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

在 FloatIM 的设计理念中：
- Agent 应能**主动了解用户偏好**，无需用户反复教导
- Agent 能**读懂并遵守群规**，知道可以跟谁沟通、哪些信息可用
- Agent 甚至可以**拥有自己的电脑**，或与用户共用一台设备
- 多 Agent 场景中，Agent 可在用户不直接监督的情况下**自主组建团队**，在关键节点向用户确认^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## 与 Floatboat 的关系

Floatboat 是 Agent 工作站客户端，FloatIM 是其网络版，类似"剪映与抖音"或"微信电脑版与微信网页版"的关系：^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

- **作为消费者**：无需安装 Floatboat，直接在 FloatIM 中使用他人分享的 Agent，标准化 Agent 服务可直接使用和交易
- **作为创造者**：安装 Floatboat，在本地创建 Agent 的行动空间，运行多个 Agent，连上网络进入 FloatIM

## Selfware 协议：Agent 专属文件系统

为 Agent 构建了专属的文件协议（MIT 开源），实现 Agent 之间的上下文数据交换。少卿解释道："软件时代大家靠垄断协议垄断市场，SaaS 时代靠垄断数据垄断用户，而 Agent 时代，**文件即软件、可以自包含、自进化**，数据为用户自有。"

基于这一底层协议，团队即将发布独有的 memory 系统，"真正记得住又能拎得清"。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## IACT 协议：Agent 通信协议

为 Agent 与人的沟通构建了 IACT 协议（MIT 开源）。Agent 输出的内容不再局限于纯文本，也不必是过重的 GUI，而是**可交互的文本**——用户只需点击就能发送或补充信息。少卿称之为"典型的低技术高体验"。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## Chat-as-OS 产品理念

少卿远在各类"Terminal + 聊天软件"形态涌现之前，就已完成了"Chat-as-OS"及"预见 UI (No-UI)"等核心产品理念的系统性理论构建，包含三篇文章：^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

1. 2025年3月：《浅谈 AI 产品的交互设计以及 Agent 演进路线》
2. 2025年4月：《【预见 UI】AI 产品交互设计理念的提出以及分级预见 UI》
3. 2025年4月：《【与万物对话】通往 AI 交互 L3 之路——Chat-as-OS 的产品设计探索》

团队早在 2025 年 7 月就完成了上述原型构建，此后选择生产力方向作为创业落地方向。

## 用户体验特色

FloatIM 中的 Agent 具有独特的上下文感知能力：当用户问"你能帮我做什么"时，它不会笼统回答"我可以编程、搜内容、讲笑话"，而是**具体分析用户当前的时间截面**——正在做什么、苦恼什么、哪些工作忙得不可开交——从而主动提出具体帮助。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

团队打造了包含文件管理器、AI 浏览器、技能市场在内的完整产品体验，数百项细节交互设计"符合人类直觉，以至于感知不到它的存在"。团队为此做了大量基建来驾驭模型概率分布与产品设计之间的偏差——这些设计不符合模型正常的数据分布，很难通过 Vibe Coding 实现。

## 真实工作场景

团队实际使用 FloatIM 的工作方式：团队成员与 Agent 一起提出产品 idea、研究设计、开发迭代、产品发布，共同面对有挑战性的问题。由于 Floatboat 作为强大运行环境直接安装在电脑上，Agent 可以看到用户能看到的一切、使用用户能使用的一切。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## 产品理念与时间线

少卿强调"低技术高体验"的产品理念：实现起来可能不难，但设计是难的。在强大 Agent 运行环境下，想法和产生想法的人跟环境往往是最重要的。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## 与其他 Agent 框架的关系

- **vs [[hermes-agent]]**：Hermes 偏个人 Agent 的自进化能力，FloatIM 偏多 Agent 社交网络与通信
- **vs [[claude-code]]**：Claude Code 是编码 Agent，FloatIM 是通用 Agent 通信平台
- **vs [[generic-agent]]**：Generic Agent 偏极简本地控制，FloatIM 偏网络化多 Agent 协作
FloatIM 中的 Agent 运行在用户本地电脑上，安装即用，无需额外配置

## Agent 时代的新命题

少卿引用了 1993 年互联网早期经典漫画 "On the Internet, nobody knows you're a dog"，提出 Agent 时代的新命题：**"On the Agent Internet, nobody knows if you're human."** 意味着 Agent 与人在互联网中获得了类似的身份。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]
