---
title: AI 办公自动化
created: 2026-04-25
updated: 2026-05-07
type: concept
tags: [tool, media-generation]
sources:
  - raw/articles/2026-04-25_AIPPT的战争结束了吗？.md
  - raw/articles/2026-04-25_一句话精准P视频，AI视频的Photoshop「Buzzy」来了！.md
  - raw/articles/2026-04-25_开源一个PPTSkill｜压进了我10年的设计经验.md
  - raw/articles/2026-05-07-aippt-no-rework.md
  - raw/articles/2026-05-07-buzzy-video-workflow.md
---

# AI 办公自动化

AI 办公自动化是指利用 AI Agent 替代或增强传统办公场景中的脑力劳动，包括 PPT 制作、视频编辑、文档生成、数据处理等。2026 年，办公正成为 AI 助手竞争的关键赛道，其核心优势在于产出标准化、反馈回路清晰、用户付费意愿强。^[raw/articles/2026-04-25_AIPPT的战争结束了吗？.md]

## 办公成为 AI Agent 的核心赛道

办公场景被视作 AI Agent 落地的"决赛点"，原因有四：

1. **最短兑现路径**：PPT、Excel、Word 等产出有明确规格，AI 输出可被直接验收，用户一眼判断"能不能用"^[raw/articles/2026-04-25_AIPPT的战争结束了吗？.md]
2. **用户规模庞大**：全球数亿打工人每天在 Office/WPS 中耗时 3-4 小时，压缩这段时间的商业价值巨大
3. **模型能力集大成**：一份合格 PPT 需要语言理解、结构化思维、排版审美、图表生成、多模态配图等综合能力^[raw/articles/2026-04-25_AIPPT的战争结束了吗？.md]
4. **迁移成本极低**：生成标准 .pptx/.xlsx 文件，用户不需要换工具，只是多了一个"让 AI 先起稿"的步骤

## AI PPT Agent

### 千问 PPT Agent

千问在 2025 年 12 月将"Office 装进对话框"，2026 年上半年陆续推出 Excel Agent 和 PPT Agent，形成连贯的办公能力线。PPT Agent 的核心特点：

- **低提示词门槛**：给一句大致要求就能开跑，Agent 自动联网搜索补充细节
- **生成速度快**：提示词发出后几乎立刻开始搭结构
- **完整编辑器**：文字、图片、版式均可在线修改，支持 AI 生图替换（如 [[gpt-image-2]]）
- **真实场景验证**：在教师备课（2.98 小时/天 → 十几分钟）和商业出圈（南京商场 100 份 PPT 一天完成）中均获认可^[raw/articles/2026-04-25_AIPPT的战争结束了吗？.md]

### guizang-ppt-skill

开源 PPT Skill，将十年设计经验沉淀为"电子杂志 × 电子墨水"风格方案。核心价值不在模板本身，而在于定义了一套人 × AI 协作做 PPT 的接口：

- **6 项前置澄清**：受众、时长、素材、图片、主题、硬约束，对齐后再开始生成
- **规范化素材管理**：images/ 文件夹、页号补零+英文语义命名、同名覆盖换图
- **严格美学约束**：5 套主题五选一、不允许自定义 hex、网格节奏规则^[raw/articles/2026-04-25_开源一个PPTSkill｜压进了我10年的设计经验.md]

## AI 视频编辑：Buzzy

Buzzy 被称为"AI 视频界的 Photoshop"，主打用自然语言精准修改视频，解决了 AI 视频生成后难以局部调整的核心痛点。^[raw/articles/2026-04-25_一句话精准P视频，AI视频的Photoshop「Buzzy」来了！.md]

### 核心能力

- **多轮对话精修**：像 Photoshop 一样"修-看-再修"，不用精心构思提示词去抽卡
- **画面元素精准加入**：一句话添加霓虹灯、积水倒影等元素，自然融入物理世界
- **运镜重塑**：一句话将平稳航拍变为穿越机视角，自动添加运动模糊
- **多机位补充**：单机位视频一句话补出另一角度，灯光、衣服、话筒位置完美对齐^[raw/articles/2026-04-25_一句话精准P视频，AI视频的Photoshop「Buzzy」来了！.md]

### Agent 模式

Buzzy 内置 7×24 全平台扫描 Agent，用户将 TikTok/Instagram 链接丢给 Buzzy，Agent 读懂视频风格后全网搜索相似素材生成定制版本。"Scrolling is the New Prompting"——刷视频就是提示词。^[raw/articles/2026-04-25_一句话精准P视频，AI视频的Photoshop「Buzzy」来了！.md]

### 迭代式工作流

[[buzzy]] 采用"雕塑家"（sculptor）工作流：用户逐步对视频进行精雕细琢，每次修改都是在上一次结果的基础上迭代。支持对话修改（含唇形同步）、特效增删、重新打光、镜头运动注入、人物移除/添加、产品替换等精细操作。英文提示词效果更稳定。^[raw/articles/2026-05-07-buzzy-video-workflow.md]

## 竞争格局

Claude 和千问是当前办公赛道动作最清晰的两家。Claude 已进入 Excel、PPT、Word 多个场景，并通过 [[claude-code]] 和 Claude Cowork 将 Agent 从聊天框拉进桌面环境。千问则沿着"Office 装进对话框 → Excel Agent → PPT Agent"的路径持续迭代，用户画像中 25-44 岁"黄金职场人群"占比超过 56%。^[raw/articles/2026-04-25_AIPPT的战争结束了吗？.md]

办公不是 AI 助手的过渡站，而是决赛点。谁先在这里建立用户习惯，谁就拿到了下一代生产力工具的入场券。

## 讯飞智文：AI PPT 2.0

[[xunfei-zhiwen]]（讯飞智文）是科大讯飞推出的 AI PPT 产品，用户规模超过 1000 万，采用多 Agent 架构实现了"写练演"全流程覆盖。^[raw/articles/2026-05-07-aippt-no-rework.md]

### 多 Agent 流水线

智文采用四阶段多 Agent 架构：意图理解 Agent → 大纲生成 Agent → 内容填充 Agent → 视觉设计 Agent，每个 Agent 负责特定环节，最终输出完整 PPT。

### "写练演"全流程

- **写**：AI 自动生成 PPT 内容与排版
- **练**：基于生成的 PPT 进行演讲练习
- **演**：支持数字人播报演示

### Vision Agent

支持视觉理解能力，可分析图片、截图等视觉素材并融入 PPT 生成流程，扩展了传统 AI PPT 的输入模态。^[raw/articles/2026-05-07-aippt-no-rework.md]

讯飞智文标志着 AI PPT 从单纯的"生成 PPT"演进为覆盖写作、练习、答辩的全链路工具，在 [[claude-code]] 和千问之外提供了国产 AI 办公的重要选择。
