---
id: source-20260924-codebase-memory-mcp
title: codebase-memory-mcp - 代码库知识图谱 MCP
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/MCP
  - 主题/代码智能
  - 主题/知识图谱
  - 主题/AI编程Agent
author: DeusData
source_type: repo
source_url: https://github.com/DeusData/codebase-memory-mcp
source_author: DeusData
source_date: 2026-09-24
summary: 本地 MCP 代码智能服务：把代码库索引成持久化知识图谱（函数/类/调用链/路由等）；tree-sitter 多语言 + Hybrid LSP；单静态二进制、零外部依赖；结构查询可大幅减少 Agent 读文件 Token。
related:
  - "[[50 来源资料/代码仓库/AI/Headroom - LLM 上下文压缩中间件|Headroom]]"
  - "[[50 来源资料/代码仓库/AI/OpenMontage - Agent 化视频制作系统|OpenMontage]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# codebase-memory-mcp - 代码库知识图谱 MCP

## Source summary

**codebase-memory-mcp**（DeusData）是面向 AI Coding Agent 的 **本地 MCP 代码智能引擎**：把仓库解析成持久化知识图谱，Agent 用图查询回答「谁调用谁 / 路由在哪 / 跨服务怎么连」等问题，而不是一轮轮打开文件扫代码。

- 开源项目，GitHub：[DeusData/codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp)
- 作者组织：DeusData
- 实现：以 **C / 原生可执行文件**为主；单静态二进制、零语言运行时依赖、无需 API Key
- 约 **42k+ stars / 3.4k+ forks**（截至 2026-09-24）
- 许可证：**MIT**
- 主页：[deusdata.github.io/codebase-memory-mcp](https://deusdata.github.io/codebase-memory-mcp/)
- 论文预印本：arXiv [2603.27277](https://arxiv.org/abs/2603.27277)

头条《省92%Token…》将其与 Headroom 并列为「一个记住代码结构、一个压缩上下文」的组合。

## 解决什么问题

| 痛点 | 方案 |
|------|------|
| Agent 每次从零读仓库、结构问题烧 Token | 持久化图谱 + MCP 工具查询，官方称结构类问题可少约 **120× Token**（示例量级） |
| 多语言仓库解析碎片化 | **162** 种语言 tree-sitter 语法内置（文中写 158，随版本增加） |
| 只要语法不要语义 | **Hybrid LSP** 对 Python/TS/JS/PHP/C#/Go/C/C++/Java/Kotlin/Rust/Perl 等做类型感知解析 |
| 不想上云向量库 | 本地处理；可内置 on-device embedding（如 nomic-embed-code），无 Ollama/Docker 硬依赖 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **知识图谱** | 函数、类、调用链、HTTP 路由、跨服务链接等 |
| **索引速度** | 普通仓库毫秒～秒级；Linux kernel 量级约数分钟（官方宣传） |
| **查询延迟** | 结构查询亚毫秒级（官方） |
| **MCP 工具** | 约 15 个工具面，适配 Claude Code / Cursor / Codex / OpenCode / Windsurf 等大量客户端 |
| **可视化** | 内置图可视化 UI（如 localhost:9749） |
| **语义边** | SEMANTICALLY_RELATED / SIMILAR_TO 等增强边 |
| **安全定位** | 读本地代码、写 Agent 配置；全本地；发布流程含 VirusTotal / SLSA 等信任材料（见 SECURITY.md） |

## 安装与接入（概要）

从 [Releases](https://github.com/DeusData/codebase-memory-mcp/releases/latest) 下载对应平台原生包，运行 `install` 即可；也可按文档配置各 Agent 的 MCP。详细以官方 README / 主页为准。

## 适用场景

- 大仓库结构探索、调用链追踪、跨文件影响分析
- 希望 Agent **先问图、再按需读文件**，降低 Token
- 与 [[50 来源资料/代码仓库/AI/Headroom - LLM 上下文压缩中间件|Headroom]] 联用：图谱减「盲目读取」，压缩减「已读体积」

**不太适合**：只要全文语义搜索 RAG（它偏结构图）；不能接受工具读写本地 Agent 配置；极小脚本仓库收益有限。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/DeusData/codebase-memory-mcp |
| **主页** | https://deusdata.github.io/codebase-memory-mcp/ |
| **llms.txt** | https://github.com/DeusData/codebase-memory-mcp/blob/main/docs/llms.txt |
| **Benchmark** | https://github.com/DeusData/codebase-memory-mcp/blob/main/docs/BENCHMARK.md |
| **论文** | https://arxiv.org/abs/2603.27277 |
| **介绍文章（头条）** | https://www.toutiao.com/article/7685280926175117876/ |

## My takeaways

1. **定位**：结构分析后端 + MCP，不是聊天机器人；智能仍在外层 Agent。
2. **Token 策略**与 Headroom 正交：一个减少「读什么」，一个减少「读进去多大」。
3. 选型看清版本：语言数从 158→162、工具数与客户端面在持续涨。

## Related

- [[50 来源资料/代码仓库/AI/Headroom - LLM 上下文压缩中间件|Headroom - LLM 上下文压缩中间件]]
- [[50 来源资料/代码仓库/AI/OpenMontage - Agent 化视频制作系统|OpenMontage - Agent 化视频制作系统]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
