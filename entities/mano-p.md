---
title: Mano-P
created: 2026-05-07
updated: 2026-05-07
type: entity
tags:
  - tool
  - open-source
  - china-ecosystem
  - computer-vision
sources:
  - raw/articles/2026-05-07-mac-ai-workstation.md
---

# Mano-P

**类型**: GUI-VLA Agent 模型
**开发商**: 明略科技 (Mininglamp Technology)
**开源协议**: Apache 2.0
**仓库**: Mininglamp-AI/Mano-P

## 概述

Mano-P 是明略科技推出的 GUI-VLA（GUI Vision-Language-Action）Agent 模型，采用纯视觉 GUI 理解方式，无需依赖 DOM、accessibility tree 等结构化接口，直接通过屏幕视觉感知进行 GUI 操作。^[raw/articles/2026-05-07-mac-ai-workstation.md]

## 性能表现

### OSWorld 基准测试

- **排名第 1**: 58.2% 准确率
- 在真实桌面环境 GUI 操作任务中达到业界领先水平^[raw/articles/2026-05-07-mac-ai-workstation.md]

### WebRetriever Protocol I

- **得分**: 41.7
- 超越 Gemini 2.5 Pro CU 的表现^[raw/articles/2026-05-07-mac-ai-workstation.md]

## 模型规格

| 指标 | 数值 |
|------|------|
| 模型大小 | 4B 参数 |
| Prefill 速度 | 476 tok/s（M4 Pro） |
| 峰值内存 | 4.3 GB |
| 理解方式 | 纯视觉（无 DOM/AT） |

## 技术特点

- **纯视觉 GUI 理解**: 不依赖操作系统提供的结构化 UI 接口，直接从屏幕像素理解界面
- **轻量高效**: 4B 参数模型可在 Apple Silicon 上流畅运行
- **开源友好**: Apache 2.0 协议，可自由商用^[raw/articles/2026-05-07-mac-ai-workstation.md]

## 生态定位

Mano-P 是 Private AI 生态中"场景"支柱的代表工具，解决本地 AI 的实际操作能力问题。与 [[cider]]（推理加速）配合可在 Apple Silicon 上构建完整的本地 Agent 能力，也可与 [[hermes-agent]] 等 Agent 框架集成扩展其应用场景。
