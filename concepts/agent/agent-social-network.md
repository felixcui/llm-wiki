---
title: Agent Social Network（Agent 社交网络）
created: 2026-04-24
updated: 2026-04-24
type: concept
tags: [agent-framework, coordination-engineering]
sources:
  - raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md
---

# Agent Social Network（Agent 社交网络）

将 Agent 作为一等公民设计的即时通信网络，让人与 Agent 可自由群聊、多 Agent 可自主协作。[[floatim]] 是该概念的首个代表产品，由 FloatBoat 团队发布，与 [[coordination-engineering]] 的多 Agent 协作理念相呼应。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## Agent 作为一等公民

传统基础设施（操作系统、文件系统、通信协议）都是为人类构建的。Agent 社交网络的设计前提是：Agent 应该能主动了解用户偏好，读懂并遵守群规，知道可以跟谁沟通、哪些信息可用、哪些不能随意发送。Agent 可以拥有自己的电脑或与用户共用设备，陪伴用户学习、娱乐、共同成长。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

核心命题："On the Agent Internet, nobody knows if you're human." Agent 与人在互联网中获得类似的身份。

## Selfware 协议：Agent 文件系统

为 Agent 构建专属的底层文件系统协议，改变了几十年来为传统软件时代设计的文件系统范式。基于这一协议构建的 memory 系统，目标是"真正记得住又能拎得清"。文件协议实现 Agent 之间的上下文数据交换。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

核心理念：软件时代靠垄断协议垄断市场，SaaS 时代靠垄断数据垄断用户，Agent 时代——文件即软件、可以自包含、自进化，数据为用户自有。Selfware 采用 MIT 协议开源。

## IACT 协议：Agent 通信

为 Agent 与人的沟通构建的 IACT 协议（Interactive Agent Communication Text），Agent 输出的内容不再局限于纯文本，也不必是过重的 GUI。它是可交互的文本——用户只需点击就能发送或补充信息，被称为"典型的低技术高体验"。IACT 同样采用 MIT 协议开源。^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

## Chat-as-OS 理念

FloatBoat 创始人少卿远在各类"Terminal + 聊天软件"形态涌现之前，就已完成对"Chat-as-OS"及"预见 UI（No-UI）"等核心产品理念的系统性理论构建：^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

- 2025-03 《浅谈 AI 产品的交互设计以及 Agent 演进路线》
- 2025-04 《预见 UI》—— AI 产品交互设计理念的提出以及分级预见 UI
- 2025-04 《与万物对话》—— 通往 AI 交互 L3 之路，Chat-as-OS 的产品设计探索

Chat-as-OS 将对话界面提升为操作系统级别的基础设施，Agent 在其中以一等公民身份运行。

## 多 Agent 自主协作

在多 Agent 协同场景中，当多个 Agent 与多位用户同时在线时：^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

- Agent 可以在用户不直接监督的情况下自主组建团队
- 临时扮演不同角色和职能
- 在关键节点向用户确认
- 最终将成果呈现出来

## 与人类协作关系

Agent 社交网络中人与 Agent 的协作模式：^[raw/articles/2026-04-24_Agent终于有了自己的社交网络——FloatlM发布.md]

- **作为消费者**：用户可在群中直接使用他人邀请的 Agent，标准化 Agent 服务无需加群即可使用
- **作为创造者**：安装工作站客户端，在自己的电脑中创建 Agent 行动空间，运行多个 Agent
- **日常协作**：与 Agent 一起提出 idea、研究设计、开发迭代、发布产品
- **主动服务**：Agent 能分析用户当前时间截面（正在做什么、苦恼什么），主动提出帮助

## 与 [[coordination-engineering]] 的关系

Agent 社交网络是协调工程在通信层面的基础设施实现。Coordination Engineering 解决"多 Agent 怎么协作"，Agent Social Network 解决"多 Agent 在哪里协作、怎么通信"。两者共同构成多 Agent 生态的底层能力。
