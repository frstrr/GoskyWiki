---
id: source-20260924-paxm
title: PAXM - Coding Agent 中立记忆适配器
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/CLI
  - 主题/MCP
  - 主题/AI编程Agent
  - 主题/Agent记忆
  - 主题/开源资源
author: pax-beehive
source_type: repo
source_url: https://github.com/pax-beehive/paxm
source_author: pax-beehive
source_date: 2026-09-24
summary: 本地优先、provider 中立的 Coding Agent 记忆适配器：用同一套 CLI/MCP/hooks 把决策与会话上下文持久化，默认 SQLite 零账号起步，可换 Zep/Mem0/MemOS/OpenViking/Team Memory 或自定义 JSON-RPC，跨 Codex、Claude Code、OpenCode、Pi、Cursor 等复用。
related:
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# PAXM - Coding Agent 中立记忆适配器

## Source summary

**PAXM**（仓库名 `paxm`，PAX Memory）是一个**本地优先的 memory adaptor**：把 Codex、Claude Code、OpenCode、Pi、Cursor、TRAE、Kimi Code、ZCode、Kiro、Cline 以及任意 MCP 客户端的记忆读写，统一路由到可插拔的 memory provider。默认本地 **SQLite**，不要求先申请账号、API key，也不要求额外 embedding/LLM 调用；之后可无缝换 provider，而不必给每个 Agent 重接一套记忆层。

- GitHub：[pax-beehive/paxm](https://github.com/pax-beehive/paxm)
- 约 **421 stars** / **20 forks**（2026-09-24 GitHub API）；许可证 **Apache-2.0**；主语言 **Go**
- Topics：`agent-memory`、`local-first`、`mcp`、`sqlite`、`codex`、`claude-code`、`opencode` 等
- 最新 Release（API）：[v0.2.6](https://github.com/pax-beehive/paxm/releases/tag/v0.2.6)（可用 `PAXM_VERSION` 固定版本安装）
- 中文入口：[docs/README.zh-CN.md](https://github.com/pax-beehive/paxm/blob/main/docs/README.zh-CN.md)
- 同系列：[DSH Plugin Hub](https://dshpluginhub.ai)（DeepSeek Harness 插件精确版本与一键安装）

定位对比：[[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace]] 偏「本地记忆系统本体」（宫殿索引、语义检索、自有存储）；**PAXM 偏适配器层**——Agent 侧契约稳定，底层存储可换，强调跨 Agent 共享同一记忆通路。

## 解决什么问题

| 痛点 | PAXM 方案 |
|------|-----------|
| 每个新会话都要重讲项目决策/约定 | `remember` / 被动 hook 落库，后续 `recall` 或被动注入 |
| 换 Codex ↔ Claude Code ↔ Cursor 记忆不通 | 一条记忆路径跨多 Agent；Codex 写下的决策 Claude/MCP 可召回 |
| 绑死某一家 memory SaaS | 默认 SQLite；可接 Zep、Mem0、MemOS、OpenViking、Team Memory 或私有 JSON-RPC |
| 记忆层延迟拖垮 Agent | 被动写入先入本地 durable queue；召回有超时预算，部分失败不挡会话 |
| 凭据/hook 被工具静默接管 | 密钥与 hook 信任归用户；MCP 不暴露 setup/凭据管理工具 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **主动记忆** | CLI / MCP / skill：显式 `remember`、`recall`、`history`、`config doctor`、`dashboard` |
| **被动记忆** | Agent 生命周期 hooks：回复前召回相关上下文，回合结束后持久化；provider 慢/挂不阻塞会话 |
| **默认 SQLite** | 内置 FTS5 + BM25 回合记忆；无外部 LLM/embedding；适合零配置跑通闭环 |
| **多 Provider** | SQLite、Zep、Mem0（自托管/Cloud）、MemOS（自托管/Cloud）、OpenViking、Team Memory、自定义 JSON-RPC；可多实例并存 |
| **Profile 路由** | `ltm` / `stm` / `default` 等读写 profile：必选/尽力、权重、阈值、超时、tier、scope |
| **身份与来源** | `user_id` / `agent_id` / `session_id` / `turn_id` + visibility `scope`；被动注入含本地时间，超 12h 再刷新 |
| **MCP Server** | `paxm mcp serve --agent <name>`，工具：`paxm_recall` / `paxm_remember` / `paxm_history` / `paxm_config_doctor` |
| **历史回填** | `paxm backfill scan/run`，可后台、可断点续跑 |
| **可观测** | `paxm dashboard`（本机指标、日志、会话与召回检查）；遥测默认存哈希与长度而非原文查询 |

## Agent 与 Provider 矩阵（摘要）

### Agent 表面

| Agent/客户端 | 主动 | 被动召回 | 被动写入 |
| --- | :---: | :---: | :---: |
| Codex | CLI, MCP, skill | Hook | Hook |
| Claude Code | CLI, MCP, skill | Hook | Hook |
| Pi | CLI, MCP, skill | Extension | Extension |
| OpenCode | CLI, MCP | Plugin | Plugin |
| Cursor | MCP | — | Hook |
| TRAE / TRAE CN | MCP | Hook | Hook |
| Kimi Code / ZCode / Kiro / Cline | MCP | Hook | Hook |
| 任意 MCP 客户端 | MCP tools | — | — |

### Memory Provider

| Provider | 模式 | 备注 |
| --- | --- | --- |
| SQLite | 默认内置 | 零 setup；无需 API key / LLM / embeddings |
| Zep | 内置 | user 或 graph 作用域 |
| Mem0 / Mem0 Cloud | 内置 | 自托管 REST 或托管 Platform API |
| MemOS / MemOS Cloud | 内置 | memory cube / OpenMem Token |
| OpenViking | 内置 | 自托管会话抽取 + 语义搜索 |
| Team Memory | 内置 | 本地 JSON-RPC；可经 `paxl device` 发现凭证 |
| 自定义 JSON-RPC | Adapter | 接入已有/私有记忆系统 |

## 快速开始

```bash
# 安装最新发布版（可用 PAXM_VERSION 固定版本）
curl -fsSL https://github.com/pax-beehive/paxm/releases/latest/download/install.sh | bash
paxm setup
paxm config doctor
```

`paxm setup` 主要问两类问题：启用哪些 provider、哪些 Agent 开被动记忆；本机已检测到的 Agent 会预勾选。默认配置：`~/.config/paxm/config.yaml`。

先跑通显式闭环再依赖被动 hook：

```bash
paxm remember --profile ltm --text "生产发布必须走 GitHub Actions，不能从笔记本直接发布"
paxm recall --query "生产环境怎么发布？"
paxm history --days 7
```

常用 profile：`ltm`（长期决策/约定）、`stm`（短期任务，默认可过期）、`default`（跟当前配置）。

### Codex plugin（最短主动+被动闭环）

```bash
codex plugin marketplace add pax-beehive/paxm --ref paxm-memory-v0.1.4
codex plugin add paxm-memory@pax-agent-nexus
curl -fsSL https://github.com/pax-beehive/paxm/releases/latest/download/install.sh | bash
paxm setup --integration codex-plugin
```

新开 Codex task，在 `/hooks` 提示时信任 Pax Agent neXus hooks。插件负责 skill 与被动 hook，**不**替你写密钥或绕过 hook 信任。

### Claude Code plugin

```bash
curl -fsSL https://github.com/pax-beehive/paxm/releases/latest/download/install.sh | bash
claude plugin marketplace add pax-beehive/paxm
claude plugin install paxm-claude@pax-memory
paxm setup --integration claude-plugin
```

含 active-memory skills、MCP server，以及 `SessionStart` / `UserPromptSubmit` / `PostToolUse` / `PostToolUseFailure` / `Stop` 五个生命周期 hook。

### MCP 直连

```bash
paxm mcp serve --agent codex
```

把 `codex` 换成配置中的 agent 名即可。

> Windows 可用仓库 Releases 中的平台二进制；安装脚本以 bash 为主，按 [Releases](https://github.com/pax-beehive/paxm/releases/latest) 选择对应资产。SQLite 父目录必须可写（需建 WAL/SHM），只读 sandbox 可能报 SQLite error 14。

## 工作方式（架构一句话）

```text
AI agents  ->  CLI / MCP / skills / hooks  ->  paxm  ->  任意 memory provider
```

被动召回默认约 **800ms** 总预算、**250ms** 单 provider；健康 provider 的部分结果仍可返回。被动写入先提交本地 durable queue，再异步投递 provider。

## 设计与运维要点

- **用户掌控**：凭据、hook 信任、路由、数据位置、禁用/卸载/回滚均由用户负责；勿把真实密钥写进仓库或文档。
- **Team Memory**：可用 `paxl device connect onprem` 登记设备；无显式 `TEAM_MEMORY_API_KEY` 时由 paxl provision agent 凭证，缓存在 `~/.config/paxm/credentials/`（0600），不进 `config.yaml`。
- **Mem0 注意**：`score_semantics` 需区分 similarity vs distance；`search_scope_payload` 需按 Mem0 版本选 `auto` / `filters` / `top_level`。
- **JSON-RPC provider**：stdout 只能出一条 JSON-RPC 响应，日志走 stderr，否则易「无响应/解析失败」。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/pax-beehive/paxm |
| **Releases** | https://github.com/pax-beehive/paxm/releases |
| **中文使用指南** | https://github.com/pax-beehive/paxm/blob/main/docs/README.zh-CN.md |
| **英文 README** | https://github.com/pax-beehive/paxm/blob/main/README.md |
| **配置参考** | https://github.com/pax-beehive/paxm/blob/main/docs/config.md |
| **架构说明** | https://github.com/pax-beehive/paxm/blob/main/docs/architecture.md |
| **Agent 集成矩阵** | https://github.com/pax-beehive/paxm/blob/main/docs/agent-integrations.md |
| **Provider adapter contract** | https://github.com/pax-beehive/paxm/blob/main/docs/provider-adapter-contract.md |
| **JSON-RPC 协议（中）** | https://github.com/pax-beehive/paxm/blob/main/docs/jsonrpc-provider-protocol.zh-CN.md |
| **JSON-RPC 协议（英）** | https://github.com/pax-beehive/paxm/blob/main/docs/jsonrpc-provider-protocol.md |
| **LoCoMo 评测说明** | https://github.com/pax-beehive/paxm/blob/main/evals/locomo/README.md |
| **DSH Plugin Hub** | https://dshpluginhub.ai |
| **许可证** | Apache-2.0 |

## My takeaways

1. **适配器 vs 记忆系统**：若已有 Mem0/自建记忆服务，或想定「多 Agent 共用一条记忆通路」，优先看 PAXM；若要「开箱即用的本地宫殿式记忆本体」，对照 [[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace]]。
2. **默认 SQLite 降低试用成本**：先本地跑通 remember/recall，再决定是否上云端/团队 provider，符合「先闭环、后换仓」。
3. **被动 hook + 超时预算** 对日常 Coding Agent 更关键：记忆不该拖慢主会话。
4. **跨 Codex / Claude / Cursor** 的统一记忆，正好补上「换工具就丢上下文」的缺口；与 wiki 里「Agent 记忆系统」扩展方向一致。
5. **注意权限边界**：MCP 故意不暴露 setup/凭据；生产上仍要把 hook 信任与密钥管理当一等公民。

## Related

- [[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]