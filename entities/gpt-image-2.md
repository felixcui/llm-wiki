---
title: GPT-Image-2
created: 2026-04-23
updated: 2026-05-08
type: entity
tags: [model, tool, media-generation, company, open-source]
sources:
  - raw/articles/2026-04-23_从技巧到API，Image2最完整解读.md
  - raw/articles/2026-04-23_刚刚，实测完GPT-Image-2：设计师没完蛋，但我被AI骗麻了.md
  - raw/articles/2026-04-24_Image2能批量生图了，23个真实场景和提示语一口气学会！.md
  - raw/articles/2026-04-22_最新Image-2模型正式上线，附提示词玩法合集！.md
  - raw/articles/2026-05-08-gpt-image2-open-source.md
---

# GPT-Image-2

**开发商**: OpenAI
**发布时间**: 2026年4月22日
**类型**: 图像生成模型
**API 模型名**: `gpt-image-2`
**可用平台**: ChatGPT、Codex、API

## 概述

GPT-Image-2（ChatGPT Images 2.0）是 OpenAI 推出的新一代图像生成模型，最大的范式变化是**首次引入思考能力**。在 ChatGPT、Codex 和 API 三端同时全量上线，支持中文字渲染、多尺寸输出和批量生图。属于 [[ai-office-automation]] 中媒体生成领域的重要产品。^[raw/articles/2026-04-23_从技巧到API，Image2最完整解读.md]

## 两种模式

### Instant 模式
- 对所有 ChatGPT、Codex 和 API 用户开放
- 主打速度，追求快速出图
- 不具备联网搜索和自我核查能力

### Thinking 模式
- 仅 ChatGPT Plus、Pro、Business 用户可用
- **首个带思考能力的图像模型**，生成前会自行推演
- 三大能力：联网搜索实时信息、一次产出最多 8 张连贯图、自我检查输出质量
- 定位为"Visual thought partner"（视觉思维伙伴）^[raw/articles/2026-04-23_从技巧到API，Image2最完整解读.md] ^[raw/articles/2026-04-23_刚刚，实测完GPT-Image-2：设计师没完蛋，但我被AI骗麻了.md]

## 核心能力提升

### 多语言文本渲染
中文不再是图像模型的"二等公民"。在中文、日文、韩文、印地文、孟加拉文等非拉丁文字的渲染上有显著改善，文字可融入设计本身，不再是后期强加的贴图感。^[raw/articles/2026-04-23_从技巧到API，Image2最完整解读.md]

### 风格保真度
在多种视觉风格上还原度显著提升：
- 摄影风格：胶片颗粒感、镜头眩光、光线不完美均可主动触发
- 关键词 `photorealistic` 可规避 AI 塑料感
- 电影感、像素艺术、漫画等风格精准捕捉^[raw/articles/2026-04-23_刚刚，实测完GPT-Image-2：设计师没完蛋，但我被AI骗麻了.md]

### 灵活宽高比
支持从 3:1（超宽）到 1:3（超高）的宽高比范围，适配横幅、海报、手机屏、社交媒体等不同场景。最大边长 ≤ 3840px，长短边比 ≤ 3:1。^[raw/articles/2026-04-23_从技巧到API，Image2最完整解读.md]

### 世界知识
知识截止日期为 2025 年 12 月，比同期大多数图像模型都新。在生成信息图、教育图表、视觉摘要时更具时效性。^[raw/articles/2026-04-23_刚刚，实测完GPT-Image-2：设计师没完蛋，但我被AI骗麻了.md]

## TouchEdit 局部修改

结合 Lovart 工具链，支持 TouchEdit 精准局部修改——用户可选中图中特定元素进行替换或修改，"指哪打哪"，无需重新生成整张图。^[raw/articles/2026-04-24_Image2能批量生图了，23个真实场景和提示语一口气学会！.md]

## 批量生图

全量上线后支持通过 Agent 批量复刻同款版式。确定一个满意的排版样式后，可以让 Agent 自动生成多张同系列图片，一致性强，省时省力。^[raw/articles/2026-04-24_Image2能批量生图了，23个真实场景和提示语一口气学会！.md]

## API 定价

API 价格较上代 gpt-image-1.5 明显上涨：^[raw/articles/2026-04-23_从技巧到API，Image2最完整解读.md]

| 质量 | 1024×1024 | 1024×1536 / 1536×1024 |
|------|-----------|----------------------|
| Low | $0.006 | $0.005 |
| Medium | $0.053（+56%） | $0.041 |
| High | **$0.211（+59%）** | $0.165 |

High 档方图从 $0.133 涨到 $0.211（+59%），Medium 档方图从 $0.034 涨到 $0.053（+56%），Low 档基本持平。

编辑模式默认按 high fidelity 处理参考图，`input_fidelity` 参数已移除。带参考图的编辑请求 token 消耗比上代略高。

## 应用场景

实测覆盖的 23+ 真实场景包括：^[raw/articles/2026-04-24_Image2能批量生图了，23个真实场景和提示语一口气学会！.md]

- **信息图鉴**：MBTI 人格图鉴、食物热量分析、历史事件分析图
- **产品分析**：充电宝构造分析图、产品原理拆解
- **IP 设计**：三视图、表情包、配色方案、个人简介
- **品牌周边**：抱枕、手机壳等 Mockup 样机
- **UI 设计**：App 界面、记账应用、天气应用
- **游戏设计**：抽卡结算画面、角色互动、地图界面、战斗画面
- **平面设计**：开业海报、放映排期表、展览海报
- **社交媒体物料**：抹茶店多平台广告（一次出多尺寸）

## 生态集成

- **Codex**：内置图像生成，无需单独 API key，ChatGPT 订阅直接覆盖，属于 OpenAI 编程生态的一部分（对比 [[claude-code]] 在 Anthropic 生态中的角色）
- **Canva**：美妆品牌广告生成
- **Figma**：从文字密集视觉到逼真场景全流程支持
- **Adobe Firefly**：从单图生成升级到结构化视觉内容
- **OpenArt**：电影级视频生产 Smart Shot 的创意分镜^[raw/articles/2026-04-23_从技巧到API，Image2最完整解读.md]

## 已知局限

- 需要完整物理世界模型的任务（折纸指南、魔方拼图）仍然吃力
- 极密、极重复的视觉细节会逼到模型上限
- 带精确箭头和零件标签的标注图准确度仍需人工复核
- 2K 以上分辨率当前是 Beta 阶段，结果可能不稳定
- 复杂提示词延迟最高可达 2 分钟^[raw/articles/2026-04-23_从技巧到API，Image2最完整解读.md]

## 竞品对比

与同期产品相比，GPT-Image-2 在文字信息容纳量和排版设计上优势明显。在多文字信息图场景中，文字排版更美观、层级区分更清晰。与 [[ai-office-automation]] 领域的其他 AI 图像工具形成竞争关系。^[raw/articles/2026-04-24_Image2能批量生图了，23个真实场景和提示语一口气学会！.md]

## 开源生态

博主 [[canghe]] 创建了开源项目 [awesome-gpt-image-2](https://github.com/freestylefly/awesome-gpt-image-2)，系统性收集整理 GPT-Image-2 的提示词（Prompt），按分类、风格、场景组织，12 天内获得 4.2K Star。配套可视化网站 gpt-image2.canghe.ai 支持按分类快速检索提示词和查看生成案例。^[raw/articles/2026-05-08-gpt-image2-open-source.md]
