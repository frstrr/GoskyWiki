---
id: source-20260901-weknora
title: WeKnora - LLM 知识管理框架
type: source
status: active
created: 2026-09-01
updated: 2026-09-01
tags:
  - 来源/代码仓库
  - 主题/RAG
  - 主题/Agent
  - 主题/知识库
  - 主题/Wiki
  - 主题/企业知识管理
author: Tencent
source_type: repo
source_url: https://github.com/Tencent/WeKnora
source_author: Tencent（腾讯）
source_date: 2026-09-01
summary: 腾讯开源的企业级 LLM 知识管理框架，一体化提供 RAG 问答、ReAct Agent 推理与 Agent 自治 Wiki；支持多源同步、IM/嵌入发布、空间 RBAC 与私有化 Docker 部署。
related:
  - "[[50 来源资料/代码仓库/AI/RAG-Anything - 多模态一体化RAG框架|RAG-Anything]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# WeKnora - LLM 知识管理框架

## Source summary

**WeKnora（维娜拉）** 是腾讯开源的、基于大语言模型的**企业级知识管理框架**，面向文档理解、语义检索与智能推理场景。官网产品入口：[weknora.weixin.qq.com](https://weknora.weixin.qq.com)。

三大核心能力：

1. **RAG 快速问答** — 基于知识库的日常知识查询
2. **ReAct Agent 智能推理** — 自主编排知识检索、MCP 工具与网络搜索，完成复杂多步任务
3. **Wiki 模式** — Agent 从原始文档自治生成相互链接的 Markdown 知识库与可视化知识图谱，支持人工编辑、版本历史与一键回滚

- GitHub：`Tencent/WeKnora`（约 **21k+ Stars**）
- 协议：**MIT**
- 当前版本脉络：v0.7.x（文档站、文件夹树、分块/Wiki 版本历史、MCP Server 1.1.x 等）
- 亦作为[微信对话开放平台](https://chatbot.weixin.qq.com)的核心技术框架之一

## 核心能力

### 智能对话

| 能力 | 说明 |
|------|------|
| **ReAct 推理** | 渐进式多步推理，编排知识检索、MCP、网络搜索 |
| **RAG 问答** | 知识库快速问答，引用浮层 + RAG 流水线进度 |
| **Wiki 模式** | 自动生成结构化互链 Markdown Wiki + 知识图谱；可编辑、diff、回滚 |
| **工具调用** | 内置工具、MCP（含 OAuth2）、`@Skill / @MCP` 按轮次范围化 |
| **临时附件** | 会话级图片/文档异步解析，一次性问答 |
| **推荐问题** | 基于知识库自动生成推荐与追问 |

### 知识管理

| 能力 | 说明 |
|------|------|
| **知识库类型** | FAQ / 文档 / Wiki；文件夹导入、URL 导入、多标签、在线录入 |
| **文件夹树** | 保留上传目录结构，树形浏览、重命名、重新归档 |
| **分块编辑** | 可视化编辑检索分块，版本快照、diff、回滚，自动重建索引 |
| **数据源同步** | 飞书知识库/云盘、Lark、Notion、语雀、RSS（更多持续接入） |
| **文档格式** | PDF / Word / Markdown / HTML / EPUB / MHTML / 图片 / CSV / Excel / PPT / JSON 等 |
| **检索策略** | BM25 / Dense / GraphRAG / 父子分块 / pgvector HNSW 等 |

### 集成与平台

| 能力 | 说明 |
|------|------|
| **模型厂商** | OpenAI / Azure / Anthropic / DeepSeek / Qwen / 智谱 / 混元 / 豆包 / Gemini / MiniMax / NVIDIA / Ollama 等 |
| **向量库** | pgvector / Elasticsearch / OpenSearch / Milvus / Weaviate / Qdrant / Doris / 腾讯云 VectorDB |
| **对象存储** | 本地 / COS / TOS / MinIO / S3 / OSS / KS3 / OBS；每空间多实例 |
| **IM 渠道** | 企业微信 / 飞书 / Lark / QQBot / Slack / Telegram / 钉钉 / Mattermost / 微信 / 云之家 |
| **网站嵌入** | Widget 发布智能体（域名白名单、限流、安全 Token 交换） |
| **MCP Server** | PyPI：`tencent-weknora-mcp`，约 29 个工具；stdio / SSE / HTTP |
| **权限** | 空间 RBAC（Owner/Admin/Contributor/Viewer）+ 资源归属 + 审计日志 + 范围化 API Key |
| **可观测性** | Langfuse 全链路追踪；运行时任务队列面板与 Worker 池治理 |
| **部署** | Docker Compose / Kubernetes(Helm) / 本地开发；可私有化 |

## 快速开始

环境：Docker、Docker Compose、Git。

```bash
git clone https://github.com/Tencent/WeKnora.git
cd WeKnora
cp .env.example .env
docker compose pull
docker compose up -d
```

启动后访问 `http://localhost`（API 默认 `:8080`）。

可选 Compose Profile：`full` / `neo4j` / `minio` / `langfuse`。

版本升级：在 `.env` 设 `WEKNORA_VERSION` 后执行 `docker compose pull && docker compose up -d`（仅 `up -d` 可能复用旧镜像）。

## 生态扩展

| 扩展 | 说明 |
|------|------|
| **Chrome 插件** | 浏览器选中文本/图片/整页一键入库 |
| **微信小程序** | 移动端配置 API、选库、URL 导入与问答 |
| **ClawHub Skill** | 经 REST 上传文档、混合检索、管理知识条目 |
| **DeepSeek Harness 插件** | `@wxg-prc-cpg/dsh-weknora`，为编码 Agent 提供 search/read/ask/list 只读工具 |
| **weknora CLI** | 命令行与 Agent Skills、会话控制等 |

## 适用场景

- 企业/团队要把分散文档沉淀为**可查询、可推理、可持续演进**的私有知识资产
- 需要 **RAG + Agent + 自动 Wiki** 一体化，而不是只做向量检索 demo
- 要对接飞书/Notion/语雀等知识源，并在企微/飞书/Slack 等 IM 或网站 Widget 中提供问答
- 要求 **私有化部署、RBAC、审计、可观测性** 的生产级知识平台选型

## 与同类资源对比（简要）

| 项目 | 侧重点 |
|------|--------|
| **WeKnora** | 产品化知识平台：RAG + Agent + Wiki + IM/嵌入 + RBAC |
| [[50 来源资料/代码仓库/AI/RAG-Anything - 多模态一体化RAG框架|RAG-Anything]] | 多模态文档解析与知识图谱 RAG 流水线（库/框架向） |
| [[50 来源资料/代码仓库/AI/MemPalace - 本地优先 AI 记忆系统|MemPalace]] | Agent 对话长期记忆，本地优先语义检索 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/Tencent/WeKnora |
| **产品官网** | https://weknora.weixin.qq.com |
| **微信对话开放平台** | https://chatbot.weixin.qq.com |
| **官方文档站源码** | 仓库 `website-docs/`（VitePress） |
| **API 文档** | 仓库 `docs/api/README.md` |
| **路线图** | 仓库 `docs/ROADMAP.md` |
| **FAQ / 排查** | 仓库 `docs/QA.md` |
| **MCP 配置** | 仓库 `mcp-server/MCP_CONFIG.md` |
| **MCP PyPI** | https://pypi.org/project/tencent-weknora-mcp/ |
| **Chrome 插件** | https://chromewebstore.google.com/detail/jpemjbopikggjlmikmclgbmkhhopjdgd |
| **ClawHub Skill** | https://clawhub.ai/lyingbug/weknora |
| **DeepSeek Harness 插件** | https://www.npmjs.com/package/@wxg-prc-cpg/dsh-weknora |
| **Issues** | https://github.com/Tencent/WeKnora/issues |
| **Changelog** | 仓库 `CHANGELOG.md` |

## My takeaways

1. **定位是「知识平台」而非纯 RAG SDK**：Web UI、多空间 RBAC、IM/嵌入发布、任务队列与 Langfuse 都齐，适合作为自托管企业知识中台候选。
2. **Wiki 模式是差异点**：不只检索原文片段，还能让 Agent 把文档沉淀成可维护的互链 Wiki + 图谱，并支持版本回滚。
3. **中国企业场景贴合度高**：飞书/语雀/企微/混元/COS 等一等公民集成，文档与社区也有完善中文材料。
4. **部署友好但体量不小**：Docker Compose 可快速起，但生产需认真配置鉴权、内网暴露、存储与向量后端；README 明确建议勿直接公网裸奔。
5. **生态入口多**：Chrome 插件、小程序、MCP、ClawHub Skill、DeepSeek Harness 插件，便于接到现有 Agent/IM 工作流。

## Related

- [[50 来源资料/代码仓库/AI/RAG-Anything - 多模态一体化RAG框架|RAG-Anything - 多模态一体化 RAG 框架]]
- [[50 来源资料/代码仓库/AI/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]