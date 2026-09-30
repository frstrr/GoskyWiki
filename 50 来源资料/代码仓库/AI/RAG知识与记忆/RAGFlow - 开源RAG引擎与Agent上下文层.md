---
id: source-20260901-ragflow
title: RAGFlow - 开源 RAG 引擎与 Agent 上下文层
type: source
status: active
created: 2026-09-01
updated: 2026-09-01
tags:
  - 来源/代码仓库
  - 主题/RAG
  - 主题/Agent
  - 主题/文档解析
  - 主题/知识库
  - 主题/企业知识管理
author: InfiniFlow
source_type: repo
source_url: https://github.com/infiniflow/ragflow
source_author: InfiniFlow
source_date: 2026-09-01
summary: InfiniFlow 开源的领先 RAG 引擎，融合深度文档理解、可解释分块、可编排 Agent 与多源知识同步，为企业级 LLM 提供高质量上下文层；支持 Docker 私有化与云服务。
related:
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/RAG-Anything - 多模态一体化RAG框架|RAG-Anything]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# RAGFlow - 开源 RAG 引擎与 Agent 上下文层

## Source summary

**RAGFlow** 是 InfiniFlow 开源的 **Retrieval-Augmented Generation（RAG）引擎**，将前沿 RAG 与 Agent 能力融合，为 LLM 构建高质量上下文层。面向从个人到企业的规模化场景，提供可落地的 RAG 工作流、深度文档理解（DeepDoc）、可编排摄入流水线，以及预置 Agent 模板。

- GitHub：`infiniflow/ragflow`（约 **89k+ Stars**，社区活跃度很高）
- 官网 / 文档：[ragflow.io](https://ragflow.io/)
- 云服务试用：[cloud.ragflow.io](https://cloud.ragflow.io)
- 协议：开源（以仓库 LICENSE 为准）
- 文档引擎默认 Elasticsearch，可选切换为同团队的 [Infinity](https://github.com/infiniflow/infinity/)

## 核心能力

| 能力 | 说明 |
|------|------|
| **深度文档理解（DeepDoc）** | 从复杂版式非结构化数据中抽取知识，强调「Quality in, quality out」 |
| **模板化分块** | 多种可解释的 chunk 模板，便于人工干预与可视化 |
| **可追溯引用** | 关键引用可视化，降低幻觉，支持 grounded answers |
| **异构数据源** | Word / PPT / Excel / TXT / 图片 / 扫描件 / 结构化数据 / 网页等 |
| **自动化 RAG 工作流** | 可配置 LLM 与 Embedding；多路召回 + 融合重排；直观 API 便于业务集成 |
| **Agent 与 MCP** | 支持 agentic workflow、MCP；Agent 侧有 Memory；含代码执行组件（沙箱，可选 gVisor） |
| **可编排摄入流水线** | 支持编排式 ingestion；文档解析可选 MinerU、Docling 等 |
| **多源同步** | Confluence、S3、Notion、Discord、Google Drive 等 |
| **多聊天渠道** | 飞书、Discord、Telegram、Line 等 |
| **多模态理解** | 可用多模态模型理解 PDF/DOCX 中的图片 |

## 架构要点

- **上下文引擎 + 预置 Agent 模板**：把复杂数据转为高保真、可生产的 AI 系统上下文
- **文档存储 / 检索引擎**：默认 Elasticsearch；可切换 Infinity（会清空现有数据，需谨慎）
- **依赖组件（自托管常见栈）**：MinIO、Elasticsearch/Infinity、Redis、MySQL 等（Docker Compose 一键拉起）

## 快速开始（自托管）

硬件建议：CPU ≥ 4 核、RAM ≥ 16 GB、Disk ≥ 50 GB；Docker ≥ 24.0.0、Compose ≥ v2.26.1；开发源码环境需 Python ≥ 3.13。

```bash
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/docker
# 建议 checkout 与镜像一致的稳定 tag，例如 v0.27.1
docker compose -f docker-compose.yml up -d
docker logs -f docker-ragflow-cpu-1
```

- 默认 HTTP 端口 80，浏览器访问 `http://服务器IP` 即可登录
- 在 `service_conf.yaml.template` 中配置 `user_default_llm` 与 `API_KEY`
- 官方镜像主要为 x86；ARM64 需自行构建镜像
- 自 `v0.22.0` 起默认提供 slim 镜像（不再附带 embedding 模型包），需外接 LLM/Embedding 服务

## 配置入口

| 文件 | 用途 |
|------|------|
| `docker/.env` | 端口、MySQL/MinIO 密码等基础环境 |
| `docker/service_conf.yaml.template` | 后端服务与默认 LLM 等 |
| `docker/docker-compose.yml` | 编排与端口映射 |

改配置后需重启容器生效。切换文档引擎到 Infinity：停止并 `down -v` → `.env` 设 `DOC_ENGINE=infinity` → 再 `up -d`（`-v` 会清数据）。

## 适用场景

- 需要 **开箱可用的企业级 RAG 产品**（Web UI + API），而不是只拿一个 Python RAG 库
- 文档版式复杂、强调 **解析质量、分块可解释、引用可追溯**
- 要做 **RAG + Agent（含 MCP / Memory / 多渠道）** 的一体化平台选型
- 愿意用 Docker 私有化，或先用官方 Cloud 试用

## 与同类资源对比（简要）

| 项目 | 侧重点 |
|------|--------|
| **RAGFlow** | 产品化 RAG 引擎：DeepDoc、模板分块、Agent/MCP、多源同步与多渠道；Stars 体量最大 |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架\\|WeKnora]] | 腾讯企业知识平台：RAG + ReAct Agent + 自治 Wiki + 国内 IM/飞书语雀生态 |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/RAG-Anything - 多模态一体化RAG框架\\|RAG-Anything]] | 库/框架向多模态 RAG（LightRAG + MinerU/Docling），偏研发嵌入 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/infiniflow/ragflow |
| **官网** | https://ragflow.io/ |
| **文档（Quickstart）** | https://ragflow.io/docs/dev/ |
| **配置说明** | https://ragflow.io/docs/dev/configurations |
| **Release notes** | https://ragflow.io/docs/dev/release_notes |
| **Cloud 试用** | https://cloud.ragflow.io |
| **Roadmap 2026** | https://github.com/infiniflow/ragflow/issues/12241 |
| **Discord** | https://discord.gg/NjYzJD3GM3 |
| **同团队 Infinity** | https://github.com/infiniflow/infinity/ |
| **Issues** | https://github.com/infiniflow/ragflow/issues |

## My takeaways

1. **定位清晰：可生产的 RAG「上下文层」产品**，不是最小 demo；DeepDoc + 可解释分块 + 引用可视化是核心卖点。
2. **Agent 能力已成一等公民**：MCP、Memory、可编排 workflow、多聊天渠道，适合「知识库 + 助手」一体部署。
3. **社区与 Stars 极高**，资料、镜像与文档相对成熟；自托管对机器资源与 Docker 运维有一定门槛。
4. **与 WeKnora / RAG-Anything 可互补选型**：要产品化中文企业 IM/Wiki 看 WeKnora；要多模态解析库嵌入看 RAG-Anything；要通用开源 RAG 平台优先评估 RAGFlow。
5. **部署注意点**：默认 ES、可选 Infinity；slim 镜像需自备模型服务；ARM 需自建镜像；代码执行功能依赖 gVisor。

## Related

- [[50 来源资料/代码仓库/AI/RAG知识与记忆/RAG-Anything - 多模态一体化RAG框架|RAG-Anything - 多模态一体化 RAG 框架]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora - LLM 知识管理框架]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
