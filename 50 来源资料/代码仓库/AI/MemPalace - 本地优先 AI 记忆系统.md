---
id: source-20260831-mempalace
title: MemPalace - 本地优先 AI 记忆系统
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/个人知识管理
author: MemPalace
source_type: repo
source_url: https://github.com/MemPalace/mempalace
source_author: MemPalace
source_date: 2026-08-31
summary: 本地优先的开源 AI 记忆系统，逐字存储对话历史并用语义检索召回；LongMemEval R@5 达 96.6%（无需 LLM）；支持 MCP、Claude Code/Cursor/Codex 自动保存钩子，可插拔向量后端。
related:
  - "[[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]"
---

# MemPalace - 本地优先 AI 记忆系统

## Source summary

**MemPalace** 是一个**本地优先（Local-first）**的开源 AI 记忆系统，以逐字原文存储对话历史，通过语义搜索检索，**不做摘要、抽取或改写**。在 LongMemEval 基准上 R@5 达 **96.6%**（纯语义检索，零 API 调用），号称当前 benchmark 表现最好的开源 AI 记忆方案之一。

- GitHub：`MemPalace/mempalace`（MIT 许可，Python 3.9+）
- PyPI：`pip install mempalace` / `uv tool install mempalace`
- 官方文档：**mempalaceofficial.com**（仅此、GitHub、PyPI 为官方来源，谨防仿冒域名）

## 核心概念：The Palace（记忆宫殿）

索引采用结构化隐喻，便于范围化检索而非扁平全文扫描：

| 概念 | 含义 |
|------|------|
| **Wing（翼）** | 人物、项目等高层实体 |
| **Room（房间）** | 主题分类 |
| **Drawer（抽屉）** | 存放原始逐字内容 |

检索层可插拔，默认 ChromaDB；接口定义于 `mempalace/backends/base.py`，可替换后端而不改动上层逻辑。**默认数据不出本机**，除非用户主动 opt-in。

## 核心能力

| 能力 | 说明 |
|------|------|
| **逐字存储 + 语义检索** | 原文入库，语义搜索召回，不依赖 LLM 做压缩 |
| **多存储后端** | Chroma（默认）、sqlite_exact、Milvus、Qdrant、pgvector |
| **知识图谱** | 带有效期窗口的时序实体关系图，SQLite 本地存储 |
| **MCP Server** | 44 个 MCP 工具：读写 palace、知识图谱、跨 wing 导航、drawer 管理、agent 日记与协作 |
| **多 Agent 支持** | 每个 specialist agent 独立 wing + diary，运行时 `mempalace_list_agents` 发现 |
| **Auto-save Hooks** | Claude Code、Codex CLI、**Cursor IDE** 定期保存与压缩前快照 |
| **Docker 部署** | `ghcr.io/mempalace/mempalace:latest`，多架构（amd64 + arm64） |
| **Benchmark 可复现** | 结果与脚本均提交在仓库 `benchmarks/` |

## 安装方式

```bash
# 推荐：uv 隔离安装 CLI
uv tool install mempalace
mempalace init ~/projects/myapp

# 或 pipx / venv + pip
pipx install mempalace
```

Docker（MCP stdio 或 CLI）：

```bash
docker pull ghcr.io/mempalace/mempalace:latest
docker run -i --rm -v mempalace-data:/data ghcr.io/mempalace/mempalace
```

## 快速上手

```bash
# 挖掘内容入库
mempalace mine ~/projects/myapp                    # 项目文件
mempalace mine ~/.claude/projects/ --mode convos   # Claude Code 会话

# 语义搜索
mempalace search "why did we switch to GraphQL"

# 新会话加载上下文
mempalace wake-up
```

## 存储后端一览

| Backend | 模式 | 安装 | 命名空间 | 词法检索 |
|---------|------|------|:--------:|:--------:|
| `chroma`（默认） | 本地 embedded | 内置 | – | ✓ |
| `sqlite_exact` | 本地 exact | 内置 | – | ✓ |
| `milvus` | Local Lite / Server | `mempalace[milvus]` | ✓ | ✓ |
| `qdrant` | Server REST | 内置 | ✓ | ✓ |
| `pgvector` | Server Postgres | `mempalace[pgvector]` | ✓ | ✓ |

通过 `--backend`、`MEMPALACE_BACKEND` 或 `config.json` 的 `"backend"` 字段配置。

## 基准表现（仓库可复现）

**LongMemEval — 检索召回 R@5（500 题）：**

| 模式 | R@5 | 是否需要 LLM |
|------|-----|:------------:|
| Raw（纯语义检索） | **96.6%** | 否 |
| Hybrid v4（450 题 held-out） | **98.4%** | 否 |
| Hybrid v4 + LLM rerank | ≥99% | 任意 capable 模型 |

其他：LoCoMo R@10 60.3%→88.9%（hybrid v5）、ConvoMem 92.9%、MemBench R@5 80.3%。完整方法论见仓库 `benchmarks/BENCHMARKS.md`。

## 与 AI 编程 Agent 的集成

| 工具 | 集成方式 |
|------|----------|
| **Claude Code** | Auto-save hooks + `mine --mode convos` 回填 JSONL  transcript |
| **Codex CLI** | Auto-save hooks |
| **Cursor IDE** | 专用 cursor-hooks：会话启动 recall + 压缩前 transcript 快照 |
| **MCP 客户端** | stdio MCP server（本地 Python 或 Docker） |
| **Antigravity / 本地模型** | 见官方 getting-started 指南 |

> **注意**：Claude Code 会话默认 30 天过期，未接 hooks 则不会自动保存。最短路径见 [Claude Code retention checklist](https://mempalaceofficial.com/guide/claude-code-retention.html)。

## 系统要求

- Python 3.9+
- 向量库（默认 ChromaDB）
- 嵌入模型磁盘约 300 MB（`embeddinggemma-300m` 多语言推荐，或 `all-MiniLM-L6-v2` 英文 ~30 MB）
- 可选：OpenAI 兼容 `/v1/embeddings` 端点做远程嵌入（LM Studio、Ollama、vLLM 等）

## 适用场景

- 需要**跨会话长期记忆**的 AI 编程/对话 Agent（Claude Code、Cursor、Codex）
- 对**数据隐私**敏感，要求记忆本地存储、零云 API
- 希望用 MCP 把记忆能力接入现有 Agent 工具链
- 需要**可复现 benchmark** 评估记忆检索质量的开源方案

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/MemPalace/mempalace |
| **PyPI 包** | https://pypi.org/project/mempalace/ |
| **官方文档** | https://mempalaceofficial.com |
| **Getting Started** | https://mempalaceofficial.com/guide/getting-started.html |
| **CLI 参考** | https://mempalaceofficial.com/reference/cli.html |
| **MCP 工具列表** | https://mempalaceofficial.com/reference/mcp-tools.html |
| **架构概念 The Palace** | https://mempalaceofficial.com/concepts/the-palace.html |
| **Cursor Hooks 指南** | https://mempalaceofficial.com/guide/cursor-hooks.html |
| **Discord 社区** | https://discord.com/invite/ycTQQCu6kn |
| **Docker 镜像** | ghcr.io/mempalace/mempalace:latest |

## My takeaways

1. **Local-first + 逐字存储**是差异化核心：不做 LLM 摘要，检索质量靠结构化索引（wing/room/drawer）+ 语义向量，benchmark 数字扎实且可复现。
2. **与编程 Agent 生态贴合**：原生支持 Claude Code / Cursor / Codex hooks 和 MCP，适合作为 Agent 的「外置长期记忆层」。
3. **后端可插拔**：从嵌入式 Chroma 到 Milvus/Qdrant/pgvector，可按部署规模选型；Docker 一键跑 MCP 降低集成门槛。
4. **防仿冒提醒**：官方仅 GitHub + PyPI + mempalaceofficial.com，其他 `.tech`/`.net` 等变体域名可能是钓鱼站。

## Related

- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
