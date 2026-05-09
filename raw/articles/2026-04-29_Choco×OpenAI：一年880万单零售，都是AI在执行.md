---
source_url: https://mp.weixin.qq.com/s/nSQ62QYEYsy17a6Zo-aNMg
ingested: 2026-04-29
sha256: placeholder
---

# Choco × OpenAI：一年 880 万单零售，都是 AI 在执行

**作者**: 金色传说大聪明

---

## 摘要

Choco利用OpenAI API处理每年880万以上订单，调用超过2000亿token，将订单错误率降至1%至5%，减少50%手动录入并提升两倍销售效率，通过OrderAgent和VoiceAgent实现24/7自动收单与隐式上下文消歧，其成功关键在于动态in-context学习与持续评估体系。

---

## 正文

...

## Choco 是谁

Choco 2018 年柏林创立，创始人是 Daniel Khachab 和 Julian Hammer. 这是食品分销技术领域第一家独角兽，累计融资超过 328M 美元

平台目前服务 21000 多家分销商，覆盖 100000 多家餐饮买家，业务跑在美国、英国、法国、德国、西班牙、UAE

## 两个产品

OrderAgent 接异步收单，VoiceAgent 接实时电话。

### OrderAgent，订单 agent

OrderAgent 收下所有非电话的输入。邮件、短信、图片、文档全收，出口是结构化的 ERP 订单。关键工程在动态 in-context learning.

### VoiceAgent，语音 agent

VoiceAgent 跑在 OpenAI 的 Realtime API 上，sub-second 延迟接电话，24/7 接得动。

## 成绩单

- 8.8M+ 订单/年，全部走 AI 一道
- 200B+ token 在生产环境跑过
- 50% 手动录入下降
- 2x 销售团队生产力，团队不加人
- 1-5% 错误率，自动化阈值可配置
- 24/7 收单，夜里和周末没有延迟

## 工程方法论

1. Evaluation 从第一天就跑
2. 搞 AI-native observability
3. 管理对概率系统的预期

## 下一步：agent orchestrator

Choco 给一类新角色起了名字：agent orchestrator. 他们不写代码，但设计和管理 agent.
