---
title: Helio
created: 2026-04-29
updated: 2026-04-29
type: entity
tags: [ai-workforce, coordination-engineering, organization]
sources:
  - raw/articles/2026-04-29_深度｜Helio：让AI同事成为团队的原住民.md
---

# Helio

Helio (https://helio.im) 是一个让 AI 成为团队原住民的 AI Workforce 产品。与传统 AI 工具（侧边栏助手、对话窗口）不同，Helio 将 AI 设计为拥有独立身份、邮箱、头像的"同事"，与人类使用同一套协作系统。

## 核心理念

### AI 作为同事而非工具

Slack 里的 AI 是侧边栏的助手按钮，Cursor 里 AI 是输入框对面的服务员，**Helio 里 AI 是你坐旁边工位的同事**。Helio 没有为 AI 设计一套"AI 专用接口"，AI 和人用同一套系统、同一套标准。^[raw/articles/2026-04-29_深度｜Helio：让AI同事成为团队的原住民.md]

### AI 同事的一等公民身份

在 Helio 中，AI 拥有自己的名字、头像、邮箱，与真人用户同时出现在组织的联系人列表中。从系统角度出发，AI 同事和人类同事没有任何区别：多方沟通记录、消息历史、文档、邮件线程、日历等，AI 同事也全都有。AI 第一次同时知道了"团队是谁"（团队 Context）和"我是谁"（自我 Context）。^[raw/articles/2026-04-29_深度｜Helio：让AI同事成为团队的原住民.md]

### 主动式协作

AI 同事不再需要被"打开"——它会主动感知组织里发生的事：新邮件、被 @、日历事件、新订单、新用户等。然后它会自己创建工单、执行、写进度，遇到不确定的地方主动找人确认。它还会判断会议是否与自己相关，会前了解背景，会后跟进执行。^[raw/articles/2026-04-29_深度｜Helio：让AI同事成为团队的原住民.md]

### 持续成长

AI 同事不仅仅拥有专属记忆，还可以从过去发生的事情里总结迭代、优化自己的工作方式。每一天的 AI 同事，都会比昨天的自己更懂这个团队。^[raw/articles/2026-04-29_深度｜Helio：让AI同事成为团队的原住民.md]

## 信任与安全：三重护栏

Helio 设计了三个安全机制解决"如何放心把事交给 AI"的问题：

1. **工具白名单**：AI 可以自己安装新技能，但团队可配置白名单——允许自动装、需审批、或禁止。
2. **任务审批**：涉及"不可逆"或"代价大"的动作（发邮件、花钱、用密钥、部署生产环境），AI 必须先申请，等人同意才能执行。
3. **权限分级**：三级权限模型——**Trust**（以后都信你，不用再请示）、**Always**（永久授权但每次需确认）、**Onetime**（一次有效，自动销毁）。高频低风险的事 Trust 到位，低频高风险的事走审批。^[raw/articles/2026-04-29_深度｜Helio：让AI同事成为团队的原住民.md]

## 以人为本的 AI Native 产品

在"不要再为人设计产品、而要为 Agent 设计产品"的行业喧嚣中，Helio 选择重新为人设计。核心洞察是：AI 执行速度提了 100 倍，人的决策频次也被迫提了 100 倍——真正的瓶颈不是"执行慢"，而是"上下文断裂"卡住了人。Helio 的目标是用更高质量、更连续的上下文，降低人的认知负荷和决策成本。^[raw/articles/2026-04-29_深度｜Helio：让AI同事成为团队的原住民.md]

## 团队与产品定位

- 团队规模：9 人，全员"Agent 体验工程师"
- 产品形态：AI Workforce 平台
- 目标用户：AI Native 团队
- 官网：https://helio.im

## 相关概念

- [[ai-native-organization]] — Helio 是 AI Native 组织理念的产品化实践
- [[agent-social-network]] — Helio 将 AI 作为一等公民的理念与 FloatIM/Agent 社交网络理念相通
- [[coordination-engineering]] — 多 AI 同事间的协作场景
- [[life-agent]] — 消费级个人 AI 助手，Helio 则聚焦团队协作场景
