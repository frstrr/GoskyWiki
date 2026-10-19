---
id: source-20261019-hy-memory
title: Hy-Memory - 腾讯混元智能体记忆框架
type: source
status: active
created: 2026-10-19
updated: 2026-10-19
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/记忆系统
author: Tencent Hunyuan
source_type: repo
source_url: https://github.com/Tencent-Hunyuan/Hy-Memory
source_author: Tencent-Hunyuan
source_date: 2026-10-16
summary: 腾讯混元开源的生产级 Agent 记忆框架（Hy Memory 2.0）：模型/引擎/应用三层全开源；支持聊天、工作、代码三类记忆；配套 MemHub 跨 AI 工具共享记忆与 Hy-Memory-30B-A3B 专用记忆模型。
related:
  - '[[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]'
  - '[[50 来源资料/代码仓库/AI/RAG知识与记忆/PAXM - Coding Agent 中立记忆适配器|PAXM - Coding Agent 中立记忆适配器]]'
---

# Hy-Memory - 腾讯混元智能体记忆框架

## Source summary

**Hy Memory（Hy-Memory）** 是腾讯混元推出的**生产级双系统认知记忆引擎**与 Python SDK（`hy-memory`），面向可自演化的 LLM Agent。2.0 于 **2026-10** 开源，采用**模型 · 框架 · 应用**三层架构，可独立使用也可组合为完整 Memory 方案。

- GitHub：`Tencent-Hunyuan/Hy-Memory`（README 标注 Apache-2.0，Python）
- PyPI：`pip install hy-memory`（2.0.x）
- 官方站：https://memory.hunyuan.tencent.com/
- 头条报道：腾讯混元记忆框架 Hy Memory 2.0 发布开源（专用模型性能超 Opus 等对照）

## 三层开源架构

| 层级 | 组件 | 面向 | 作用 |
|------|------|------|------|
| **MODEL** | Hy-Memory-30B-A3B | 记忆系统开发者/研究者 | 专为记忆读写训练的模型（A3B 激活） |
| **ENGINE** | Hy-Memory SDK | Agent 开发者 | 记忆的存储、更新与召回，SDK 接入 |
| **APP** | MemHub | AI 工具用户 | 把记忆带到 Claude Code / Cursor / Codex / OpenCode 等工具 |

> 三层可单独使用，也可组合成完整 Agent Memory 方案。

## 三类记忆场景

据官方介绍，Hy-Memory 是业界较早系统提出**工作记忆 + 代码记忆**的记忆框架之一：

| 类型 | 说明 |
|------|------|
| **Chat Memory（升级）** | 从零散事实升级为完整事件脉络；跨对话以事件关联时间/地点/人物，多线索问答前合并去重、剥离冲突 |
| **Working Memory（新增）** | 记住任务做到哪里、经验何时适用；多维度记录目标/阶段结果/可复用经验与适用条件，复用前做条件判断 |
| **Coding Memory（新增）** | 记住代码为什么这样改；长程任务继续前先定位当前事项，再召回相关决定、经验与历史过程 |

## 核心能力（框架层）

- 生产级双系统认知记忆管线（Chat / Working / Coding）
- Python SDK：`HyMemoryClient`，支持 `add` / `search` 等公开 API
- 可插拔后端：核心含 ChromaDB、Kuzu；可选 Qdrant、FAISS、Neo4j、MySQL、本地 rerank
- 默认 LLM / Embedding 走 OpenAI-compatible 接口
- 已在腾讯内部金融、社交娱乐、游戏及自选股、iWiki、腾讯新闻等业务落地

## 安装与最小示例

```bash
pip install hy-memory==2.0.3
# 可选：qdrant / faiss / graph / mysql / rerank / all
export OPENAI_API_KEY=sk-your-key
```

```python
from hy_memory import HyMemoryClient

with HyMemoryClient(mode='pro') as client:
    client.add('用户喜欢科幻电影', user_id='user_001')
    result = client.search('用户喜欢什么？', user_ids=['user_001'])
    for channel, memories in result['memories'].items():
        for memory in memories:
            print(channel, memory['score'], memory['content'])
```

> 持久化业务上下文用 `meta`；`metadata` 仅为兼容用的处理选项。

## MemHub（应用层）

**MemHub** 是基于 Hy Memory 的本地优先桌面/CLI 产品：把用户授权的 AI 应用（Claude Code、Cursor、Codex、OpenCode、Qwen Code 等）的对话与编码痕迹统一到本地数据目录，实现**跨工具共享、可治理的记忆召回**。

- 仓库：https://github.com/Tencent-Hunyuan/MemHub

## 评测与论文线索

- 框架侧对照常见开源记忆方案（如 Mem0、MemOS 等），在 Chat / Working / Coding 多基准上表现突出（详见官网评测口径说明）
- 模型侧：Hy-Memory-30B-A3B，Hugging Face：`tencent/Hy-Memory-30B-A3B`
- 论文：Hy-Memory 2.0 → https://arxiv.org/pdf/2610.22196 ；1.0（DCPM）→ arXiv 2606.09483（EMNLP 2026）
- Coding 基准 CMB 已提交 ICLR 2027

## 适用场景

- 需要为 Agent 接入**可演进的长期记忆**（聊天偏好 + 任务进度 + 代码决策）
- 希望**一套记忆跨多个 AI 编程工具**复用（经 MemHub）
- 研究/自建记忆系统时，需要专用记忆模型 + 可替换存储后端的开源底座
- 对标 Mem0 / MemPalace / PAXM 等记忆方案时做选型参考

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub · Hy-Memory** | https://github.com/Tencent-Hunyuan/Hy-Memory |
| **GitHub · MemHub** | https://github.com/Tencent-Hunyuan/MemHub |
| **官方介绍站** | https://memory.hunyuan.tencent.com/ |
| **PyPI** | https://pypi.org/project/hy-memory/ |
| **Hugging Face 模型** | https://huggingface.co/tencent/Hy-Memory-30B-A3B |
| **论文 2.0** | https://arxiv.org/pdf/2610.22196 |
| **使用文档** | https://github.com/Tencent-Hunyuan/Hy-Memory/blob/main/docs/usage.md |
| **来源报道（头条）** | https://www.toutiao.com/article/7697209791894028826/ |

## My takeaways

1. **三层全开源**是差异化：不只给 SDK，还给出专用记忆模型与跨工具应用 MemHub，选型时可按「只接引擎 / 只要模型 / 要跨工具管家」拆开用。
2. **工作记忆 + 代码记忆**直接对准 Agent 长程任务与编程场景，和纯 Chat 记忆方案（如部分 RAG/对话摘要方案）定位不同。
3. 与 wiki 内 [[MemPalace - 本地优先 AI 记忆系统]]、[[PAXM - Coding Agent 中立记忆适配器]] 可对照：MemPalace 偏本地逐字检索；PAXM 偏 Coding Agent 中立适配；Hy-Memory 偏腾讯混元生产级三层记忆体系。
4. 落地时注意：需配置 LLM/Embedding 密钥与后端存储；业务上下文请写入 `meta` 而非 `metadata`。

## Related

- [[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/PAXM - Coding Agent 中立记忆适配器|PAXM - Coding Agent 中立记忆适配器]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/_分类说明|RAG知识与记忆]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]