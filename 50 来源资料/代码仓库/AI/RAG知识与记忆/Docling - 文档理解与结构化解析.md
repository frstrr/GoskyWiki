---
id: source-20260924-docling
title: Docling - 文档理解与结构化解析
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/文档解析
  - 主题/RAG
  - 主题/知识库
  - 主题/PDF
  - 主题/MCP
  - 主题/企业知识管理
author: IBM Research / LF AI & Data
source_type: repo
source_url: https://github.com/docling-project/docling
source_author: IBM Research Zurich（现属 Linux Foundation LF AI & Data）
source_date: 2026-09-24
summary: IBM Research 发起、现属 LF AI & Data 的开源文档理解库：把 PDF/Office/网页/音视频等复杂文档解析为可结构化导出的 DoclingDocument，保留版面、阅读顺序与表格结构；支持本地/air-gapped、VLM、MCP/API Server，以及 LangChain/LlamaIndex/Haystack/CrewAI 集成，专为 RAG 与 Agent 预处理设计。
related:
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层|RAGFlow]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/RAG-Anything - 多模态一体化RAG框架|RAG-Anything]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/Synthadoc - LLM 知识编译引擎|Synthadoc]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# Docling - 文档理解与结构化解析

## Source summary

**Docling** 是面向生成式 AI 的开源**文档处理与理解**库：把 PDF、Office、网页、邮件、音视频等异构材料解析成统一的 `DoclingDocument`，再导出 Markdown / HTML / JSON / DocTags 等，供 RAG、知识库与 Agent 使用。定位不是「又一个 PDF 转 MD 工具」，而是 **Document → Structured Data → AI** 链路中的基础设施层。

- GitHub：[`docling-project/docling`](https://github.com/docling-project/docling)（约 **67.7k Stars / 4.9k Forks**，MIT）
- 文档站：[docling-project.github.io/docling](https://docling-project.github.io/docling/)
- 论文：[arXiv:2408.09869](https://arxiv.org/abs/2408.09869)（Docling Technical Report）
- 发起：IBM Research Zurich；现为 [LF AI & Data Foundation](https://lfaidata.foundation/projects/) 托管项目
- 语言：Python（**3.10+**；自 2.70.0 起不再支持 3.9）
- 安装：`pip install docling`

> 来源文章标题写「205K Star」偏营销口径；以仓库实时 Star 为准。头条原文：[GitHub 205K Star！这个项目火了！AI终于能读懂复杂文档](https://www.toutiao.com/article/7687425528471355904/)（作者：前端Hardy，2026-09）

## 核心能力

| 能力 | 说明 |
|------|------|
| **多格式解析** | PDF、DOCX、PPTX、XLSX、HTML、EPUB、图片、邮件（EML/MSG）、Apple Pages/Keynote、ODT/ODS/ODP、LaTeX、纯文本/Markdown、XBRL、音频（WAV/MP3）、视频（MP4/AVI/MOV/MKV/WebM）等 |
| **高级 PDF 理解** | 版面布局、阅读顺序、表格结构、代码块、公式、图片分类；OCR 支持扫描件 |
| **统一文档模型** | `DoclingDocument` 表达结构，可无损 JSON 与多种导出格式 |
| **表格保真** | 保留行列结构，可导出 Markdown / HTML / 结构化 JSON——企业手册、参数表、故障代码表场景关键 |
| **本地 / 隔离部署** | 可完全本地执行，适合合同、专利、财务等敏感数据与 air-gapped 环境 |
| **VLM 管线** | 支持 GraniteDocling 等视觉语言模型，布局理解超出传统 OCR |
| **ASR / 视频** | 音视频可结合 ASR 转录与关键帧抽取 |
| **生态集成** | LangChain、LlamaIndex、Haystack、CrewAI 等即插即用 |
| **MCP Server** | Agent 可主动调用文档解析能力（发现 PDF → 解析 → 再分析） |
| **API 服务** | `docling-serve` 可将 Docling 作为服务部署 |
| **CLI** | `docling document.pdf` 一键出 Markdown；也可处理 URL |

## 为什么适合 RAG

传统痛点往往不是「模型不够强」，而是：**切分前文档结构已乱**。粗暴按 Token 切分容易把表格、故障码与处理方法拆散到不同 Chunk。

Docling 的价值是：**先理解结构，再交给 Chunk / Embedding / RAG**。它本身不是完整 RAG 产品（对比 [[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层|RAGFlow]]），但解决了 RAG 链路最容易被忽略的第一步。[[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层|RAGFlow]]、[[50 来源资料/代码仓库/AI/RAG知识与记忆/RAG-Anything - 多模态一体化RAG框架|RAG-Anything]] 等项目已将其列为可选文档解析器。

## 快速开始

```bash
pip install docling
docling https://arxiv.org/pdf/2206.01062
```

```python
from docling.document_converter import DocumentConverter

source = "https://arxiv.org/pdf/2408.09869"
converter = DocumentConverter()
result = converter.convert(source)
print(result.document.export_to_markdown())
```

VLM 管线示例：

```bash
docling --pipeline vlm --vlm-model granite_docling https://arxiv.org/pdf/2206.01062
```

## 适用场景

- 企业知识库 / RAG：PDF 手册、合同、报表需**结构化后再入库**
- 私有化 / 内网：原始文件不能上云，要本地解析 + 本地向量库
- Agent 工作流：通过 MCP 让 Agent 自主拉取并理解文档
- 研发嵌入：在自建流水线中替换「粗 OCR + 纯文本」的弱解析层

## 与同类资源对比（简要）

| 项目 | 侧重点 |
|------|--------|
| **Docling** | 文档理解基础设施：版面/表格/多格式 → 结构化导出；可本地、可 MCP |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层\|RAGFlow]] | 产品化 RAG 引擎（DeepDoc + Agent/MCP）；解析可选 Docling |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/RAG-Anything - 多模态一体化RAG框架\|RAG-Anything]] | 多模态 RAG 框架；解析器可选 MinerU / Docling / PaddleOCR |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/Synthadoc - LLM 知识编译引擎\|Synthadoc]] | ingest-time 编译为可互链 Wiki，偏知识合成而非底层版面解析 |
| MinerU 等 | 同属文档解析赛道；选型时可按语言、版式、许可与部署成本对比 |

## 关键链接

| 类型 | 链接 |
|------|------|
| 仓库 | https://github.com/docling-project/docling |
| 文档 | https://docling-project.github.io/docling/ |
| PyPI | https://pypi.org/project/docling/ |
| 技术报告 | https://arxiv.org/abs/2408.09869 |
| MCP 用法 | https://docling-project.github.io/docling/usage/mcp/ |
| API Server | https://docling-project.github.io/docling/usage/api_server/ |
| Discord | https://docling.ai/discord |
| 头条介绍文 | https://www.toutiao.com/article/7687425528471355904/ |
