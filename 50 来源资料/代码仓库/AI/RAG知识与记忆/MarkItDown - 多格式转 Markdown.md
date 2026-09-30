---
id: source-20260924-markitdown
title: MarkItDown - 多格式转 Markdown
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/文档解析
  - 主题/Markdown
  - 主题/RAG
  - 主题/知识库
  - 主题/PDF
  - 主题/MCP
  - 主题/LLM预处理
author: Microsoft
source_type: repo
source_url: https://github.com/microsoft/markitdown
source_author: Microsoft
source_date: 2026-09-24
summary: 微软开源的轻量 Python 工具：把 PDF/Office/图片/音频/HTML/ZIP/YouTube/EPUB 等转为面向 LLM 的 Markdown，保留标题、列表、表格与链接结构；支持 CLI/Python API/Docker/插件与 markitdown-mcp，可选 Azure Document Intelligence / Content Understanding。MIT。
related:
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/Docling - 文档理解与结构化解析|Docling]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/Synthadoc - LLM 知识编译引擎|Synthadoc]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层|RAGFlow]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# MarkItDown - 多格式转 Markdown

## Source summary

**MarkItDown** 是微软开源的轻量 **Python 文档转 Markdown** 工具，目标是把各类办公与媒体文件转成 **LLM / 文本分析流水线** 友好的 Markdown（保留标题、列表、表格、链接等结构）。它更接近 [textract](https://github.com/deanmalmgren/textract) 一类抽取工具，但刻意面向「给模型读」而非「给人看的高保真排版还原」。

- GitHub：[`microsoft/markitdown`](https://github.com/microsoft/markitdown)（约 **186.7k Stars / 13.8k Forks**，MIT）
- PyPI：[`markitdown`](https://pypi.org/project/markitdown/)
- 语言：Python（**3.10–3.14**）
- 安装：`pip install 'markitdown[all]'`
- 仓库内相关包：`markitdown`（核心）、`markitdown-mcp`、`markitdown-ocr`、`markitdown-sample-plugin`

> 头条标题写「180K Star」属营销口径；以仓库实时 Star 为准。头条原文：[GitHub 180K Star！微软出手了：PDF、Word 一键变 Markdown](https://www.toutiao.com/article/7683051205891490346/)（切入点：AI 落地 / RAG / Agent 读不懂杂乱办公文档）

## 核心能力

| 能力 | 说明 |
|------|------|
| **多格式转 MD** | PDF、PowerPoint、Word、Excel、图片（EXIF + OCR）、音频（元数据 + 语音转写）、HTML、CSV/JSON/XML、ZIP（遍历内容）、YouTube URL、EPUB 等 |
| **结构保留** | 输出强调标题层级、列表、表格、链接，便于 LLM 理解文档骨架 |
| **CLI** | `markitdown file.pdf -o out.md`；也支持管道输入 |
| **Python API** | `MarkItDown().convert(...)`；可按场景选 `convert_local` / `convert_stream` / `convert_response` |
| **可选依赖** | `[pdf]` `[docx]` `[pptx]` `[xlsx]` `[xls]` `[outlook]` `[audio-transcription]` `[youtube-transcription]` `[az-doc-intel]` `[az-content-understanding]` `[all]` |
| **插件生态** | 默认关闭；`--use-plugins` / `enable_plugins=True`；社区用 `#markitdown-plugin` 发现 |
| **OCR 插件** | `markitdown-ocr`：用已有 `llm_client`/`llm_model` 对嵌入图片做视觉 OCR，无额外 ML 二进制依赖 |
| **Azure 增强** | Document Intelligence、Content Understanding（更高质量抽取、结构化字段 YAML front matter、音视频） |
| **LLM 看图** | 可为 pptx/图片传入 OpenAI 兼容客户端做图片描述 |
| **MCP** | 仓库含 `markitdown-mcp`，便于 Agent 调用转换能力 |
| **Docker** | 官方提供镜像构建与 stdin/stdout 用法 |

## 解决什么问题

做 RAG / Agent / 知识库时，大模型通常**不能原生读** PDF、Word、PPT 等二进制办公格式。常见做法是先转成文本或 Markdown。MarkItDown 的定位是：

- **轻量、本地优先**的「文件 → Markdown」预处理层
- 输出为 **token 友好、结构清晰** 的 Markdown（主流模型对 MD 训练充分）
- 不追求排版级高保真；复杂版面/扫描件可用 Azure CU / Doc Intel 或对比 [[50 来源资料/代码仓库/AI/RAG知识与记忆/Docling - 文档理解与结构化解析|Docling]]

## 快速开始

```bash
pip install 'markitdown[all]'
markitdown path-to-file.pdf -o document.md
# 或
cat path-to-file.pdf | markitdown
```

```python
from markitdown import MarkItDown

md = MarkItDown(enable_plugins=False)
result = md.convert("test.xlsx")
print(result.markdown)
```

按需安装格式依赖示例：

```bash
pip install 'markitdown[pdf,docx,pptx]'
```

## 适用场景

- RAG / 知识库入库前：PDF、Office 批量转 Markdown 再切分与向量化
- Agent 工具链：通过 CLI / MCP 让 Agent 临时读一份本地文档
- LLM 评测与批处理：统一把异构语料压成 Markdown 文本
- 本地隐私场景：默认本地转换；仅在显式启用 Azure 服务时上云

## 安全注意（官方强调）

MarkItDown 以**当前进程权限**做 I/O（类似 `open()` / `requests.get()`）。在不可信输入、托管服务场景必须：

1. **消毒输入**（路径、URI scheme、内网/元数据地址等）
2. **调用最窄 API**：只需本地文件时用 `convert_local()`；需自控拉取时先 `requests.get` 再 `convert_response()` / `convert_stream()`

## 与同类资源对比（简要）

| 项目 | 侧重点 |
|------|--------|
| **MarkItDown** | 微软出品；轻量「多格式 → Markdown」；CLI/API/插件/MCP；可选 Azure 增强 |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/Docling - 文档理解与结构化解析\\|Docling]] | 更偏文档理解基础设施：版面/表格/统一 DoclingDocument；本地/VLM/MCP/API Server |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/Synthadoc - LLM 知识编译引擎\\|Synthadoc]] | ingest-time 用 LLM 编译成可互链 Wiki，偏知识合成而非底层转换 |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层\\|RAGFlow]] | 产品化 RAG；解析层可选多种引擎 |
| textract | 同类「多格式转文本」前辈；MarkItDown 更强调 Markdown 结构与 LLM 管线 |

选型直觉：**只要快速把办公文档变成给模型吃的 MD → MarkItDown**；需要强版面/表格结构化与本地深度解析 → 优先看 Docling。

## 关键链接

| 类型 | 链接 |
|------|------|
| 仓库 | https://github.com/microsoft/markitdown |
| PyPI | https://pypi.org/project/markitdown/ |
| Issues | https://github.com/microsoft/markitdown/issues |
| 示例插件 | 仓库内 `packages/markitdown-sample-plugin` |
| MCP 包 | 仓库内 `packages/markitdown-mcp` |
| OCR 插件 | 仓库内 `packages/markitdown-ocr` |
| Azure Content Understanding | https://learn.microsoft.com/azure/ai-services/content-understanding/ |
| 头条介绍文 | https://www.toutiao.com/article/7683051205891490346/ |