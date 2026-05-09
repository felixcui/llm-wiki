---
title: SenseNova U1
created: 2026-04-30
updated: 2026-04-30
type: entity
tags: [model, open-source, architecture, media-generation]
sources:
  - raw/articles/2026-04-30-sensenova-u1-infographic-model.md
---

# SenseNova U1

**开发商**: 商汤科技 (SenseTime)
**发布时间**: 2026年4月29日
**模型规模**: 8B (dense) / A3B (MoE)
**开源协议**: Apache 2.0
**核心架构**: NEO-Unify

## 概述

SenseNova U1 是商汤科技开源的端到端多模态模型，采用 **NEO-Unify 架构** 直接处理原始像素，无需传统的 Visual Encoder 和 VAE 模块。模型有 8B dense 和 A3B MoE 两个尺寸，在图像理解与图像生成双赛道均达到开源同量级 SoTA，部分指标接近商业闭源大模型。^[raw/articles/2026-04-30-sensenova-u1-infographic-model.md]

## NEO-Unify 架构

主流多模态模型的处理流程依赖"翻译官"模式：图像先经过 Visual Encoder 转为 token 给模型理解，生成的 token 再经 VAE 转回像素。U1 的 NEO-Unify 架构直接去掉这两个组件，让模型直接读取和输出原始像素，自主学习近乎无损的视觉表征。这种架构设计在已开源的多模态模型中较为罕见。^[raw/articles/2026-04-30-sensenova-u1-infographic-model.md]

## 核心能力

### 信息图生成

信息图是 U1 的主打能力。在高文字密度、精细排版要求下，U1 的测评得分与 Qwen-Image 2.0、Seedream 4.5 等大模型持平，但延迟显著更低。生成一张 2K 信息图约需十几秒，大幅低于 [[gpt-image-2]] 的几十秒，单位时间产能优势明显。^[raw/articles/2026-04-30-sensenova-u1-infographic-model.md]

### 图文交错（带图思考）

U1 最独特的能力是"图文交错"——一次推理输出中包含多张图像与正文段落的连贯混排。模型在推理过程中自动生成中间示意图，将复杂逻辑可视化。代表案例包括发型设计方案（上传照片→分析特征→多张推荐图配文字说明→对比图）和悬崖图书馆建筑设计（四个视角连贯建筑图+设计说明）。该能力是 [[gpt-image-2]]、Nano Banana 2、Seedream 5 等闭源模型不具备的，它们均采用"一次 prompt 出一张图"的单点模式。^[raw/articles/2026-04-30-sensenova-u1-infographic-model.md]

### 风格切换与稳定性

实测中 U1 能稳定复现多种风格，包括 Anthropic 编辑插画风（米白底、炭黑手绘线、赤陶橙强调色）和技术蓝图深色风格。人物和风格一致性在多次输出中得到保持，训练语料多样性较好，无明显默认风格倾向。^[raw/articles/2026-04-30-sensenova-u1-infographic-model.md]

## 应用场景

- **自媒体与独立创作者**：每日文章配图、信息图、海报产出，支持快速批量化尝试多版本
- **数据敏感性行业（医疗、金融、法务）**：本地部署确保内部数据不上云，闭源 API 在此场景下不可用
- **Agent 长链路场景**：一次任务需 10-50 张图（教程、报告、绘本、漫画），本地低成本运行使链路可行

商汤计划将 U1 接入"办公小浣熊"，推动产品化落地。^[raw/articles/2026-04-30-sensenova-u1-infographic-model.md]

## 已知局限

- 部分文字渲染存在少量错别字（如 "Karpathy" 写成 "Karpthy"、"蒸馏" 写成 "漓"），可通过 prompt 工程规避
- 复杂图表的绝对稳定性仍有提升空间^[raw/articles/2026-04-30-sensenova-u1-infographic-model.md]

## 开源与上手

- 开源代码: [GitHub - OpenSenseNova/SenseNova-U1](https://github.com/OpenSenseNova/SenseNova-U1)
- Skill 调用: [SenseNova Skills](https://github.com/OpenSenseNova/SenseNova-Skills)
- HuggingFace: [sensenova-u1](https://huggingface.co/collections/sensenova/sensenova-u1)
- 模型 8B 体量，硬件要求不高，支持 vLLM 和 sglang 推理框架^[raw/articles/2026-04-30-sensenova-u1-infographic-model.md]

## 竞品对比

相较于 [[gpt-image-2]]（闭源、API 调用、单图模式、延迟较高），U1 的核心差异在于：开源可本地部署、支持图文交错生成、延迟更低（10-20秒 vs 40秒+）、边际成本近零。而 [[lovart]] 等平台依赖 [[gpt-image-2]] 等闭源模型作为底层引擎，U1 的开源属性为类似工具链提供了本地化替代方案。虽在单张极致画质上不及商业闭源模型，但在隐私敏感、批量生产、Agent 工作流等场景中具有独特优势。^[raw/articles/2026-04-30-sensenova-u1-infographic-model.md]
