---
id: source-20260909-fastclaw
title: FastClaw - Go 轻量多 Agent 运行时
type: source
status: active
created: 2026-09-09
updated: 2026-09-09
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/多智能体
  - 主题/Agent运行时
author: fastclaw-ai
source_type: repo
source_url: https://github.com/fastclaw-ai/fastclaw
source_author: fastclaw-ai
source_date: 2026-09-09
summary: 用 Go 编写的轻量 AI Agent 运行时 / Agent Factory：单二进制、任意 LLM、多智能体、沙箱隔离、云就绪；自带 Dashboard、Skills（ClawHub）、OpenAI 兼容 API，定位为 OpenClaw 的轻量替代方案之一。
related:
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/Edict - 三省六部制 Multi-Agent 编排|Edict - 三省六部制 Multi-Agent 编排]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
---

# FastClaw - Go 轻量多 Agent 运行时

## Source summary

**FastClaw** 是用 **Go** 编写的轻量 **AI Agent 运行时（Agent Factory）**：负责创建、管理与运行 AI Agent。每个 Agent 有独立人格（SOUL.md）、记忆、Skills 与工具；框架统一处理 LLM 通信、工具执行、沙箱隔离与会话管理。

- 开源项目，GitHub：[fastclaw-ai/fastclaw](https://github.com/fastclaw-ai/fastclaw)
- 官网：[https://fastclaw.ai](https://fastclaw.ai)
- 语言：**Go 1.25+**（前端 Dashboard 用 pnpm 构建后嵌入）
- 约 **1,336 stars / 222 forks**（截至 2026-09-09）
- 许可证：README 标注 **MIT**（GitHub License 元数据为 Other/NOASSERTION，以仓库 LICENSE 为准）
- Topics：`agent-factory`、`agent-runtime`、`multi-agent`、`openclaw-alternative`
- 口号：**Single binary · Any LLM · Multi-agent · Sandbox · Cloud-ready**

## 解决什么问题

需要一个「开箱即用」的 Agent 工厂：不是只写对话脚本，而是把人格文件、记忆、Skills、沙箱、多模型与管理台打包成可部署运行时。

| 痛点 | FastClaw 方案 |
|------|---------------|
| 多 Agent 难管 | 每 Agent 独立 workspace（SOUL / MEMORY / skills / sessions） |
| 模型碎片化 | 统一对接 OpenAI / Anthropic / Ollama / OpenRouter 等，支持按 Agent 覆盖模型 |
| 工具执行不安全 | E2B 云沙箱或 Docker 沙箱；执行后文件同步回持久存储 |
| 缺少管理面 | 内置 Dashboard（默认 `http://localhost:18953`） |
| 生态扩展 | Skills 可从 ClawHub / skills.sh / GitHub 安装；支持 MCP 与插件 |

与 [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw]] 关系：仓库自标 **openclaw-alternative**——更偏 **Go 单二进制运行时 + 沙箱 + API**，OpenClaw 更偏 **多 IM 入口 + ClawHub 生态**；二者 Skills 思路可对照，但产品形态不同。

## 核心能力

| 能力 | 说明 |
|------|------|
| **Agent Factory** | 创建/管理多 Agent；人格 SOUL.md、身份 IDENTITY.md、长期记忆 MEMORY.md |
| **多 LLM** | OpenAI、Anthropic、Ollama、OpenRouter、Groq、DeepSeek、Mistral 及任意 OpenAI 兼容 API；Prompt cache |
| **内置工具** | exec、read/write_file、list_dir、web_fetch、web_search、memory_search |
| **沙箱** | E2B 或 Docker；技能与工作区自动 hydrate，工具调用后镜像回持久存储 |
| **Skills** | 内置 code-runner、image-gen、data-analysis、translation、web-search、skill-creator；可装 ClawHub / skills.sh |
| **MCP / 插件** | MCP server；JSON-RPC 子进程插件 |
| **记忆** | MEMORY.md 由 heartbeat 自动更新；会话上下文与 thinking 内容可保留供抽取 |
| **API** | OpenAI 兼容 `/v1/chat/completions`（流式）；Web chat SSE；Agent/Session/Provider/Skills/API Key 管理 |
| **存储** | `file` 或 `postgres`；Skills 始终落文件；配置可始终用文件引导 |

## 架构与数据目录

```text
~/.fastclaw/
  fastclaw.json              # 全局配置（gateway、storage、providers、defaults）
  apikeys.json               # 外部访问 API Key
  skills/                    # 共享 Skills（内置 + 安装）
  agents/
    <agent>/agent/
      agent.json             # Agent 配置（模型覆盖等）
      SOUL.md / MEMORY.md
      sessions/
      skills/                # Agent 私有 Skills
```

**边界**：SOUL / MEMORY / Skills / Sessions 由 FastClaw 管；用户账号、计费、业务产出文件由上层应用（如 ChatClaw）或 S3 等自行管理。

## Dashboard 能力速览

- **Agents**：创建与管理，每人设人格与模型
- **Skills**：从 ClawHub 或 GitHub 安装共享技能
- **Models**：配置各 LLM Provider
- **Settings**：存储、沙箱、Gateway
- Agent 面板内：**Chat / Files / Skills / Models / Sessions**

## 安装与快速体验

```bash
curl -fsSL https://raw.githubusercontent.com/fastclaw-ai/fastclaw/main/install.sh | bash
fastclaw    # 打开安装向导 http://localhost:18953
```

首次运行会引导配置 LLM Provider、创建默认 Agent，并生成 **admin token**（Dashboard 登录用，需保存）。

本地 Gateway：

```bash
./fastclaw gateway
```

Docker：`cd deploy/docker && ./start.sh`  
也可按 README 做 Kubernetes 挂载配置与 `FASTCLAW_STORAGE_DSN`。

从源码构建：先 `web` 下 `pnpm install && pnpm build`，再 `go build -o fastclaw ./cmd/fastclaw`。

## 适用场景

- 想要 **单二进制** 部署的多 Agent 运行时，而不是重型 Python 编排栈
- 需要 **沙箱隔离** 执行工具（E2B / Docker）并同步产物
- 希望提供 **OpenAI 兼容 API**，把 Agent 能力嵌进现有应用
- 需要 Web Dashboard 管理人格、Skills、模型与会话
- 评估 **OpenClaw 替代/互补**（Go 运行时 vs IM 入口生态）

**不太适合**：强依赖飞书/Slack 等 IM 原生入口（看 OpenClaw）；需要「三省六部」式制度化审核编排（看 [[50 来源资料/代码仓库/AI/Agent运行时与编排/Edict - 三省六部制 Multi-Agent 编排|Edict]]）；只要编码 Agent Skills 方法论（看 Superpowers）。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/fastclaw-ai/fastclaw |
| **官网** | https://fastclaw.ai |
| **Issues** | https://github.com/fastclaw-ai/fastclaw/issues |
| **安装脚本** | https://raw.githubusercontent.com/fastclaw-ai/fastclaw/main/install.sh |
| **ClawHub Skills** | https://clawhub.ai |
| **skills.sh** | https://skills.sh |
| **组织主页** | https://github.com/fastclaw-ai |

## My takeaways

1. **定位清晰**：Agent Factory + 运行时，不是又一个「聊天 Demo」；人格/记忆/技能文件化，便于版本与审计。
2. **与 OpenClaw 对照记**：同属 Claw 系 Skills 思路（ClawHub），但 FastClaw 强调 Go 单二进制、沙箱与 API；OpenClaw 强调多平台 IM。
3. **云就绪路径短**：file → postgres、本地 → Docker/K8s，适合从本机向服务化演进。
4. **上层应用边界明确**：账号计费与业务文件不塞进运行时，利于做 ChatClaw 类产品封装。
5. **License 元数据不一致**：README 写 MIT，GitHub API 显示 Other——使用/商用前核对仓库 LICENSE 文件。

## Related

- [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/Edict - 三省六部制 Multi-Agent 编排|Edict - 三省六部制 Multi-Agent 编排]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
