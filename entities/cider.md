---
title: Cider
created: 2026-05-07
updated: 2026-05-07
type: entity
tags:
  - tool
  - open-source
  - inference
  - china-ecosystem
sources:
  - raw/articles/2026-05-07-mac-ai-workstation.md
---

# Cider

**类型**: MLX 推理加速框架
**开发商**: 明略科技 (Mininglamp Technology)
**开源协议**: GitHub 开源
**仓库**: Mininglamp-AI/cider

## 概述

Cider 是明略科技基于 MLX 框架开发的推理加速工具，专为 Apple Silicon 优化。通过 fused kernels 和 Metal 4 TensorOps API 实现 W8A8/W4A8 量化推理加速，目前仅支持 M5 Pro 芯片。^[raw/articles/2026-05-07-mac-ai-workstation.md]

## 核心特性

### 量化加速

- **W8A8 量化**: 矩阵运算加速 1.82–1.86x
- **W4A8 量化**: 更高压缩率，适合显存受限场景
- 基于 fused kernels 优化，减少 kernel launch 开销
- 利用 Metal 4 TensorOps API 实现底层硬件加速

### 实验性功能

- **ANE + GPU 并行模块**: 实验性的 Apple Neural Engine 与 GPU 协同推理，进一步榨取 Apple Silicon 性能^[raw/articles/2026-05-07-mac-ai-workstation.md]

### 易用性

提供一行代码 API，快速接入：

```python
from cider import convert_model
model = convert_model(model)
```

## 硬件要求

- **仅支持 M5 Pro** 芯片
- 依赖 Metal 4 TensorOps API（macOS 26+）
- 需要 MLX 框架运行环境

## 生态定位

Cider 是 Private AI（本地 AI）生态中"速度"支柱的核心工具，与 [[mano-p]]（场景）和 [[hermes-agent]] 等本地 Agent 框架共同构建 Apple Silicon 上的本地 AI 能力。类似 [[openclaw]] 等项目也在探索 Apple Silicon 上的 AI 推理优化。
