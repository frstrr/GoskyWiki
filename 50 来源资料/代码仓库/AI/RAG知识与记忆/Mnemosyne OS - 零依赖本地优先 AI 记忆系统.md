---
id: source-20261111-mnemosyne-os
title: Mnemosyne OS - 零依赖本地优先 AI 记忆系统
type: source
status: active
created: 2026-11-11
updated: 2026-11-11
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/记忆系统
  - 主题/MCP
  - 主题/开源资源
author: Hu Jingkun (胡景堃) / FrankHu-HK
source_type: repo
source_url: https://github.com/FrankHu-HK/mnemosyne
source_author: FrankHu-HK
source_date: 2026-11-11
summary: 本地优先、零第三方依赖的 AI Agent 长期记忆运行时：SQLite 落盘、52 个 MCP 工具（含 Mem0 兼容名）、哈希链审计、热温冷遗忘分层与中文检索优化；可作库/CLI/HTTP/MCP，另有 SaaS（ai-memory.net）。
related:
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/PAXM - Coding Agent 中立记忆适配器|PAXM - Coding Agent 中立记忆适配器]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/Hy-Memory - 腾讯混元智能体记忆框架|Hy-Memory - 腾讯混元智能体记忆框架]]"
---

# Mnemosyne OS - 零依赖本地优先 AI 记忆系统

## Source summary

**Mnemosyne OS** 是面向 AI Agent 的**本地优先（Local-first）长期记忆运行时**：核心宣称**零第三方依赖**（`install_requires` 为空），记忆落在本机 **SQLite**，可不装向量库、不拉模型、不绑云账号即可读写。暴露方式包括 **Python 库 / CLI / HTTP API / MCP Server**（约 **52** 个能力工具，另含 Mem0 兼容工具名）。

- GitHub：[`FrankHu-HK/mnemosyne`](https://github.com/FrankHu-HK/mnemosyne)（**MIT**，主语言 Python；约 **40★ / 6 forks**，2026-11-11 观测）
- PyPI：`pip install mnemosyne-os`
- 最新 Release（API）：[v8.1.0](https://github.com/FrankHu-HK/mnemosyne/releases/tag/v8.1.0)（2026-10-20）
- 官网 / Handbook：[ai-memory.net](https://ai-memory.net) · [AI Memory Handbook](https://ai-memory.net/ai-memory-handbook/)
- 头条介绍文：全球顶尖+国内唯一：AI 记忆界「极简主义」开源了（介绍 7.0 零依赖、审计链、MCP、遗忘分层等）
- 可复现评测仓库：[MnemosyneOS/mnemosyne-bench](https://github.com/MnemosyneOS/mnemosyne-bench)

定位对比：[[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace]] 偏「宫殿索引 + 语义检索记忆系统」；[[50 来源资料/代码仓库/AI/RAG知识与记忆/PAXM - Coding Agent 中立记忆适配器|PAXM]] 偏跨 Agent 的**适配器层**；**Mnemosyne** 偏**可自托管的记忆引擎本体**，强调零依赖冷启动、审计完整性与 Mem0 工具名兼容。

## 解决什么问题

| 痛点 | Mnemosyne 方案 |
|------|----------------|
| Vector DB / CUDA / 复杂依赖劝退 | 核心零第三方依赖；干净 Python 即可跑；可选再挂 LLM/Embedding/向量库 |
| Token / 部署成本高、Serverless 难塞 | 本地 SQLite；Serverless / 离线友好；热温冷分层压缩低价值记忆 |
| 记忆黑盒、难审计合规 | SHA-256 **哈希链账本**，`verify_integrity` 可定位篡改条目 |
| 已接 Mem0 MCP，不想改提示词 | 提供与常规 agent-memory 同名的 11 个兼容工具 |
| 中文检索体验差 | 二分词 + FTS5 + 内置同义词词典 |
| 误写入密钥 / Prompt 注入 | 写入前 Security Notary：凭据脱敏、不可见 Unicode、HTML 注入检测 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **零依赖核心** | 无 numpy/torch/外置向量库亦可运行；内置规则抽取与内置 embedder |
| **四维作用域隔离** | `user_id` / `agent_id` / `run_id` / `app_id`；物理上可按 scope 分 SQLite 文件 |
| **多信号检索** | 语义 + BM25 + 实体图 + 时间 + 标签，带词法回退 |
| **图谱记忆** | 实体链接与多跳遍历内置 SQLite，无需外置图库 |
| **热 / 温 / 冷分层** | 「遗忘经济学」：低价值记忆降级压缩，而非静默删除；AIC 无损压缩可 `expand` 回原文并验哈希 |
| **哈希链审计** | 链式账本；支持完整性校验与审计查询 |
| **MCP Server** | stdio；约 52 能力工具（含 retain/recall/audit、会话摘要、冲突解决、代码考古、注入管线等） |
| **Mem0 兼容工具名** | `add_memory` / `search_memories` 等 11 个，便于替换指向 |
| **安全公证** | 写入前脱敏与注入检测 |
| **可插拔后端（可选）** | LLM / Embedder / Vector / Graph / Reranker 多 provider；失败时显式降级到内置实现 |

## 基准表现（仓库宣称，可复现）

| Benchmark | Mnemosyne OS | Mem0（其公开数字） |
|-----------|:------------:|:-----------------:|
| LongMemEval | **96.2** | 94.4 |
| LoCoMo | **94.8** | 92.5 |
| BEAM 1M | **68.5** | 64.1 |
| BEAM 10M | **53.9** | 48.6 |

复现脚本：`scripts/verify_recall_quality.py`；独立 harness 见 `MnemosyneOS/mnemosyne-bench`。

## 安装与快速上手

```bash
pip install mnemosyne-os
```

```python
from mnemosyne import Memory

m = Memory()  # 内置 embedder + 规则抽取；离线、无 API Key
m.add("I prefer dark mode and use vim keybindings. My name is Alice.",
      user_id="alice")

for hit in m.search("what does alice prefer", filters={"user_id": "alice"})["results"]:
    print(f"{hit['score']:.3f}  {hit['memory']}")
```

CLI 示例：

```bash
mnemosyne init
mnemosyne add "I prefer dark mode and vim keybindings" --user-id alice
mnemosyne search "what does alice prefer" --user-id alice
```

MCP 接入示例：

```json
{
  "mcpServers": {
    "mnemosyne": {
      "command": "python",
      "args": ["-m", "mnemosyne.webui.mcp_server",
               "--brain-dir", "./mem", "--namespace", "default"],
      "env": { "MNEMOSYNE_MCP_TOKEN": "<random 32+ chars>" }
    }
  }
}
```

HTTP / 控制台：`mnemosyne-web --port 9090`（控制台与 REST 同端口）。

## 使用形态一览

| 形态 | 入口 | 备注 |
|------|------|------|
| Python 库 | `Memory` / `AsyncMemory` / `MemoryClient` | `from_config` 可挂 OpenAI、Qdrant 等；`describe()["degraded"]` 看降级原因 |
| CLI | `mnemosyne ...` | 适合脚本与 Agent 工具循环（`--agent` JSON 信封） |
| MCP | `python -m mnemosyne.webui.mcp_server` | Claude Desktop / Hermes / DeepSeek Harness 等 |
| HTTP | `/v3/memories/add/` 等 | 自托管 REST + Web 控制台 |
| SaaS | [ai-memory.net](https://ai-memory.net) | Free / Pro / Team；**开源自托管不锁功能** |

## MCP 工具分层（摘要）

- **检索与治理**：`retain` / `recall` / `forget` / `capsule` / `expand` / `audit` / `doctor` / `graph_query` / `temporal_query` 等
- **记忆形成（较新）**：`session_summarize` / `conflict_resolve` / `playbook_save` / `code_change_record` / `code_archaeology` 等
- **运行时注入**：`profile_card` / `memory_inject` / `recall_catalog` / `expand_memory` / `memory_hot_path` 等
- **客户端兼容**：`add_memory` / `search_memories` / `get_memory` / `update_memory` / `delete_memory` 等

## 适用场景

- 要给 Agent 加**可自托管、可审计**的长期记忆，又不想先搭向量库
- 已用 Mem0 风格 MCP，希望**同名工具**切到本地引擎
- Serverless / 离线 / 嵌入式等**依赖敏感**环境
- 需要中文友好检索、写入前脱敏、热温冷分层的企业或隐私场景
- 与 MemPalace / Hy-Memory / PAXM 选型对比时，作为「零依赖引擎」候选

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/FrankHu-HK/mnemosyne |
| **PyPI** | https://pypi.org/project/mnemosyne-os/ |
| **官网 / SaaS** | https://ai-memory.net |
| **Handbook** | https://ai-memory.net/ai-memory-handbook/ |
| **Bench 仓库** | https://github.com/MnemosyneOS/mnemosyne-bench |
| **最新 Release** | https://github.com/FrankHu-HK/mnemosyne/releases/tag/v8.1.0 |
| **头条介绍文** | https://www.toutiao.com/article/7677858660780900898/ |

## My takeaways

1. **差异化在「核心真零依赖」**：不先装向量库/模型也能跑通记忆闭环；需要更强召回时再按组件插拔，且降级原因显式可见。
2. **审计链 + 遗忘分层 + 安全公证**把「可合规、可追溯」推到记忆层，而不只是检索分数。
3. **MCP 面很全**（含会话形成、代码考古、注入管线），且兼容 Mem0 工具名，迁移成本低。
4. 与 MemPalace 同属 local-first 记忆赛道：选型时看你更要「零依赖/审计账本」还是「宫殿索引 + 多向量后端 + Agent hooks」。
5. 头条文偏 7.x 卖点叙事；以仓库 README / Release（当前观测 v8.1.0）为准核对工具数量与 API。

## Related

- [[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/PAXM - Coding Agent 中立记忆适配器|PAXM - Coding Agent 中立记忆适配器]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/Hy-Memory - 腾讯混元智能体记忆框架|Hy-Memory - 腾讯混元智能体记忆框架]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/_分类说明|RAG知识与记忆]]
