---
source_url: https://mp.weixin.qq.com/s/ZyTehtxkbgPEXDjDRrdP4w
ingested: 2026-04-29
sha256: placeholder
---

# MCP 已死？不不不，Agent 进生产都绕不开 MCP

**作者**: 池建强
**来源**: MacTalk 微信公众号

## 核心论点

Agent 要连接外部系统通常有三种方式——直接 API 调用、CLI，以及 MCP。尽管 CLI + Skills 近期非常热门，很多人（包括作者）曾认为 MCP 会被放弃（因为上下文消耗大、效率不高），但 Anthropic 的最新文章《Building agents that reach production systems with MCP》指出：构建生产级别的 Agent，目前还是 MCP 更靠谱。

## 三种集成方式对比

### 1. 直接 API 调用
- 早期验证方便，一个 Agent 接一个服务，接口清晰
- 规模化后出现 **M×N Integration Problem**（M 个 Agent × N 个服务）：认证、参数描述、错误恢复、权限分配、异常处理都需要重复做
- 开始是工程问题，最终演变为组织成本问题

### 2. CLI + Skills
- 对开发者友好，对本地环境友好
- 大量成熟系统已有 CLI（Git、Kubernetes、Cloudflare、AWS 等）
- 飞书、钉钉、企微都有命令行工具，opencli 等通用命令行可访问各大网站
- 优势：快、轻、贴近本地环境
- 局限：Web 和移动端运行环境、凭证文件、权限隔离复杂；个人助理（如 Cowork）无法直接运行 CLI

### 3. MCP（Model Context Protocol）
- 正在进入成熟期，将 Agent 和外部系统之间的连接层标准化
- MCP Server 暴露能力，客户端通过协议发现工具、处理认证、理解语义并完成调用
- 需要更多前期投入，写 MCP Server 比封装 API 或运行 CLI 更麻烦
- 生产系统看重准确性、可靠性和安全性，而不仅仅是开发成本

## Anthropic 的关键判断

生产级 Agent 越来越多地运行在云端——既能持续运行，又能扩展和统一管理，也更容易接入企业流程。Agent 要访问的系统也大多在云端（数据仓库、工单系统、CRM、代码仓库、监控平台、基础设施服务）。

此时代：
- CLI 的本地优势变成限制
- 直接 API 调用容易把每个 Agent 和每个系统绑成一团
- **远程 MCP Server**可以服务多个兼容的客户端/服务端（Claude、Codex、SOLO 等），一次编写，到处被调用

## MCP 工具设计原则

### 任务单元设计
- 不要将 API 一比一映射为 MCP 工具（会导致工具碎片化，大模型需要自己判断调用顺序）
- 更好的方式：把工具设计成**任务单元**（接近人的完整目标）
  - 反面：暴露"获取对话、解析消息、创建 issue、关联附件"四个原子工具
  - 正面：暴露 `create_issue_from_conversation()`，内部串联多次调用

### 核心金句
> **MCP 是给 AI Agent 用的 UI，不是给开发者用的 SDK**

每个 tool 应对应用户/Agent 想要达成的一个完整目标，而不是对应后端一个原子操作。

### 示例：订单追踪
- 糟糕封装：暴露三个接口（查订单、查物流、查预计送达时间）→ 大模型加载三份描述、发起三次往返、塞满对话历史
- 好封装：暴露一个 `track_order(email)`，内部串联调用，返回 "订单 #12345678 已由 老池 发出，周一送达"

## 推荐实践
- 尽可能构建**远程 MCP Server**（基于 Streamable HTTP 协议）
- 一次安装，Web、移动端和云托管 Agent 都能用
- 不需要依赖本机环境，不需要每个 Agent 构建沙箱运行 shell

## 未来展望
> 未来做产品，一方面要考虑人怎么用，另一方面要想想，Agent 怎么用。谁都不能得罪，都得 Friendly（芙蓉得利）。
