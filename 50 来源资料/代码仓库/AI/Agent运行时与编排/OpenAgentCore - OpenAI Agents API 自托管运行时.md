---
id: source-20261008-openagentcore
title: OpenAgentCore - OpenAI Agents API 自托管运行时
type: source
status: active
created: 2026-10-08
updated: 2026-10-08
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/Agent运行时
  - 主题/自托管
author: MiniMax-AI
source_type: repo
source_url: https://github.com/MiniMax-AI/OpenAgentCore
source_author: MiniMax-AI
source_date: 2026-10-08
summary: MiniMax 开源的自托管 OpenAI Agents API 实现（Go）。同一套 API 可挂 Codex / Claude Code / MiniMax Code 等原生 Harness，沙箱可选 Docker、microsandbox、E2B 或本机，组件协议化可替换。
related:
  - [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
  - [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
  - [[50 来源资料/代码仓库/AI/Agent运行时与编排/Agent Substrate - AI Agent 大规模运行时|Agent Substrate - AI Agent 大规模运行时]]
  - [[50 来源资料/代码仓库/AI/Agent运行时与编排/JIT-Agent - 即时生成 Agent Harness|JIT-Agent - 即时生成 Agent Harness]]
---

# OpenAgentCore - OpenAI Agents API 自托管运行时

## Source summary

**OpenAgentCore** 是 MiniMax 开源的 **自托管 OpenAI Agents API 实现**：在自有基础设施上跑 AI Agent，对外暴露与 OpenAI Agents API 兼容的接口，并内置多种原生 Harness。

- 开源项目，GitHub：[MiniMax-AI/OpenAgentCore](https://github.com/MiniMax-AI/OpenAgentCore)
- 官网：[https://openagentcore.dev/](https://openagentcore.dev/)（中文：[/zh/](https://openagentcore.dev/zh/)）
- 语言：**Go**；许可证：**MIT**
- 约 **201 stars / 23 forks**（截至 2026-10-08）
- Topics：`agent-runtime`、`openai-agents-api`、`coding-agent`、`sandbox`、`self-hosted`、`codex`、`claude-code`、`mcp`
- 口号：**One core. Many agents.**

## 解决什么问题

想自建「类 OpenAI Agents」能力时，常见分裂：

| 方式 | 问题 |
|------|------|
| 直接调 OpenAI 托管 Agents API | 数据与执行环境不在自己侧，成本与合规受限 |
| 各家 Coding Agent（Codex / Claude Code 等）各自为政 | 客户端与协议不统一，难做统一编排与运维 |
| 自研一整套 Agent 协议 | 学习成本高，生态工具接不上 |

OpenAgentCore 的思路：**协议对齐 OpenAI Agents API**，执行层可插拔（Harness / 沙箱 / 模型供应商），用同一套 SDK 或 HTTP 客户端对接自建实例。

## 核心能力

| 能力 | 说明 |
|------|------|
| **兼容 OpenAI Agents API** | `/v1` 协议对齐；可用官方 OpenAI Agent SDK 或普通 HTTP，只改 endpoint |
| **多原生 Harness** | Session 可选 [Codex](https://github.com/openai/codex)、[Claude Code](https://code.claude.com/docs/en/overview)、[MiniMax Code](https://github.com/MiniMax-AI/minimax-code) |
| **可选执行环境** | 托管沙箱：Docker / [microsandbox](https://github.com/zerocore-ai/microsandbox) / [E2B](https://e2b.dev/)；也可跑在自有 Linux / macOS / Windows |
| **可替换组件** | 沙箱、Harness、模型供应商均通过既定协议接入 |
| **双 API 面** | Agents API（`/v1`，应用侧）+ Core API（`/core/v1`，运维/Web 控制台） |
| **Web 管理控制台** | 部署概览、Agent 监控、域名/HTTPS、模型与 Project API key 管理 |

## 架构要点

```
应用 / OpenAI SDK ──► Agents API (/v1)
管理员 Web ─────────► Core API (/core/v1)
                         │
                      Core（持久化执行状态）
                         │
                      Runtime（在 Environment 中跑所选 Harness）
                         │
              沙箱 / 本机节点 + 配置的模型供应商
```

各边界协议化，便于单独替换沙箱、Harness 或模型后端。详见[架构说明](https://openagentcore.dev/zh/docs/architecture)。

## 安装与上手

Linux / macOS（需 Docker，见[前置条件](https://openagentcore.dev/zh/docs/getting-started/install#prerequisites)）：

```sh
curl -fsSL https://github.com/MiniMax-AI/OpenAgentCore/releases/latest/download/install.sh | bash
```

Windows PowerShell：

```powershell
irm https://github.com/MiniMax-AI/OpenAgentCore/releases/latest/download/install.ps1 | iex
```

建议步骤：

1. 用安装器生成的 Core key 登录 Web，配置域名与 HTTPS
2. 设置默认模型，签发 Project API key
3. 添加执行资源（节点 / E2B / 本机）
4. 用 OpenAI SDK [跑第一个 Session](https://openagentcore.dev/zh/docs/getting-started/quickstart)

## 文档导航（官方）

| 目标 | 入口 |
|------|------|
| 安装与运维 | [安装](https://openagentcore.dev/zh/docs/getting-started/install) · [运维](https://openagentcore.dev/zh/docs/getting-started/operations) |
| 基于 API 开发 | [快速开始](https://openagentcore.dev/zh/docs/getting-started/quickstart) · [Agents API](https://openagentcore.dev/zh/docs/api/public-agent-api) |
| 完整示例 | [Examples](https://openagentcore.dev/zh/docs/examples) |
| 本机跑 Agent | [自托管执行](https://openagentcore.dev/zh/docs/getting-started/self-hosted) |
| Harness 能力边界 | [Harness capabilities](https://openagentcore.dev/zh/contracts/agents-api/harness-capabilities) |
| 接入新组件 | [开发指南](https://openagentcore.dev/zh/docs/development) |

## 适用场景

- 需要 **OpenAI Agents API 兼容**、又希望 **数据与执行在自有基础设施** 的团队
- 想用统一控制面调度 **Codex / Claude Code / MiniMax Code** 等多 Harness
- 要在 Docker / E2B / 本机之间切换沙箱与算力的自建 Agent 平台

**不太适合**：只想本地临时试一个 CLI Agent（直接用对应官方工具更轻）；或完全不关心 OpenAI Agents 协议、只做单机脚本编排。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/MiniMax-AI/OpenAgentCore |
| **官网** | https://openagentcore.dev/ |
| **中文文档入口** | https://openagentcore.dev/zh/docs/getting-started/ |
| **英文 README** | https://github.com/MiniMax-AI/OpenAgentCore/blob/main/README.md |
| **中文 README** | https://github.com/MiniMax-AI/OpenAgentCore/blob/main/README.zh-CN.md |
| **Issues** | https://github.com/MiniMax-AI/OpenAgentCore/issues |
| **贡献指南** | https://github.com/MiniMax-AI/OpenAgentCore/blob/main/CONTRIBUTING.md |
| **MiniMax Code（相关 Harness）** | https://github.com/MiniMax-AI/minimax-code |

## My takeaways

1. **定位是「兼容层 + 运行时」**：不是又做一个专用 Agent SDK，而是让现有 OpenAI Agents 客户端指向自建后端。
2. **Harness 可插拔**是核心卖点：同一 Session 协议下可换 Codex / Claude Code / MiniMax Code，适合多工具并存的团队。
3. **与 Agent Substrate 互补**：Substrate 偏 K8s 上大规模 Actor 超售；OpenAgentCore 偏 API 兼容与多 Harness 自托管，更贴近「私有化 Agents 平台」。
4. **新项目、更新活跃**（2026-09 建仓，持续推送）；接入前建议对照官方 Harness 能力矩阵与运维文档评估生产就绪度。

## Related

- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/Agent Substrate - AI Agent 大规模运行时|Agent Substrate - AI Agent 大规模运行时]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/JIT-Agent - 即时生成 Agent Harness|JIT-Agent - 即时生成 Agent Harness]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/FastClaw - Go 轻量多 Agent 运行时|FastClaw - Go 轻量多 Agent 运行时]]