---
id: source-20260831-rag-anything
title: RAG-Anything - 多模态一体化 RAG 框架
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/RAG
  - 主题/多模态
  - 主题/知识图谱
  - 主题/文档解析
author: HKUDS
source_type: repo
source_url: https://github.com/HKUDS/RAG-Anything
source_author: HKUDS (香港大学数据智能实验室)
source_date: 2026-08-31
summary: 基于 LightRAG 的多模态一体化 RAG 框架，支持 PDF/Office/图片等混排文档的解析、知识图谱构建与跨模态检索，集成 MinerU/Docling/PaddleOCR 解析器与 VLM 增强查询。
related:
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# RAG-Anything - 多模态一体化 RAG 框架

## Source summary

**RAG-Anything** 是香港大学数据智能实验室（HKUDS）开源的 **All-in-One 多模态 RAG 框架**，构建于 [LightRAG](https://github.com/HKUDS/LightRAG) 之上。它解决传统文本 RAG 无法有效处理图片、表格、公式、图表等混排内容的问题，提供从文档解析到多模态检索的完整流水线。

- GitHub：`HKUDS/RAG-Anything`，约 **23k+ Stars**
- PyPI 包名：`raganything`
- 协议：**MIT**
- 技术报告：[arXiv:2510.12323](http://arxiv.org/abs/2510.12323)

## 核心能力

| 能力 | 说明 |
|------|------|
| **端到端多模态流水线** | 文档摄入 → 解析 → 内容分析 → 知识图谱 → 智能检索 |
| **通用文档支持** | PDF、Office（DOC/DOCX/PPT/PPTX/XLS/XLSX）、图片（JPG/PNG/BMP/TIFF/GIF/WebP）、TXT/MD |
| **专项内容分析** | 图片（VLM 描述）、表格（结构化解读）、公式（LaTeX 解析）、可扩展模态处理器 |
| **多模态知识图谱** | 跨模态实体抽取、关系映射、层级结构保留（belongs_to 链） |
| **混合检索** | 向量相似度 + 图遍历融合，模态感知排序 |
| **VLM 增强查询** | 检索上下文含图片时自动送入 VLM 联合分析 |
| **直接内容列表插入** | 跳过文档解析，直接插入外部预解析的 content_list |
| **批量处理** | `process_folder_complete` 支持多文件并行 |

## 架构流程

```
文档解析 → 内容分析 → 知识图谱 → 智能检索
   ↓           ↓           ↓           ↓
 MinerU/    并发多管道   多模态实体    向量-图融合
 Docling/   模态路由     跨模态关系    模态感知排序
 PaddleOCR
```

### 解析器选择

| 解析器 | 特点 |
|--------|------|
| **MinerU**（默认） | 高保真 PDF/图片/Office 结构提取，OCR + 表格 + 公式，支持 GPU |
| **Docling** | Office/HTML 优化，更好的文档结构保留 |
| **PaddleOCR** | OCR 导向，图片/PDF 文本块提取 |

## 快速开始

```bash
# PyPI 安装（推荐）
pip install raganything
pip install 'raganything[all]'   # 全部可选依赖

# 源码安装
git clone https://github.com/HKUDS/RAG-Anything.git
cd RAG-Anything && uv sync
```

```python
from raganything import RAGAnything, RAGAnythingConfig

config = RAGAnythingConfig(
    working_dir="./rag_storage",
    parser="mineru",
    enable_image_processing=True,
    enable_table_processing=True,
    enable_equation_processing=True,
)
rag = RAGAnything(config=config, llm_model_func=..., vision_model_func=..., embedding_func=...)

await rag.process_document_complete("document.pdf", output_dir="./output")
result = await rag.aquery("文档中的图表显示了什么？", mode="hybrid")
```

## 查询模式

| 类型 | 方法 | 说明 |
|------|------|------|
| 纯文本查询 | `aquery()` / `query()` | hybrid / local / global / naive 模式 |
| VLM 增强查询 | `aquery(vlm_enhanced=True)` | 自动加载检索到的图片送 VLM 分析 |
| 多模态查询 | `aquery_with_multimodal()` | 附带 table/equation/image 等内容联合查询 |

## 依赖与环境

| 依赖 | 用途 |
|------|------|
| [LightRAG](https://github.com/HKUDS/LightRAG) | 底层 RAG 引擎与知识图谱 |
| [MinerU](https://github.com/opendatalab/MinerU) | 默认文档解析器 |
| **LibreOffice** | Office 文档处理（需单独安装） |
| OpenAI 兼容 API | LLM + Embedding + VLM |
| `paddlepaddle` | 使用 PaddleOCR 解析器时需要 |

可选 extras：`[image]`、`[text]`、`[paddleocr]`、`[all]`

## 适用场景

- 学术论文、技术文档、财务报告等 **图文表公式混排** 的知识库构建
- 需要从 PDF 图表/表格中 **跨模态问答** 的场景
- 企业知识管理中统一处理多种格式文档
- 已有外部解析结果（如 MinerU 输出），想 **直接插入 content_list** 跳过重复解析

## 生态关联项目（同 HKUDS 团队）

| 项目 | 说明 |
|------|------|
| [LightRAG](https://github.com/HKUDS/LightRAG) | 轻量快速 RAG，RAG-Anything 的底层基础；2026 已原生集成 RAG-Anything 多模态能力 |
| [VideoRAG](https://github.com/HKUDS/VideoRAG) | 超长视频 RAG |
| [MiniRAG](https://github.com/HKUDS/MiniRAG) | 极简 RAG 实现 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/HKUDS/RAG-Anything |
| **PyPI** | https://pypi.org/project/raganything/ |
| **技术报告** | http://arxiv.org/abs/2510.12323 |
| **多模态故障排查** | 仓库 `docs/multimodal_rag_failure_modes.md` |
| **上下文配置模块** | 仓库 `docs/context_aware_processing.md` |

## My takeaways

1. **多模态 RAG 的完整参考实现**：不是只做文本 chunk，而是图片/表格/公式各有专用处理器并汇入知识图谱，适合复杂文档场景。
2. **与 LightRAG 深度集成**：可复用已有 LightRAG 实例，也支持独立使用；团队生态形成 LightRAG → RAG-Anything → VideoRAG 层次。
3. **解析器可插拔**：MinerU 为主力，Docling/PaddleOCR 可按场景切换，降低 vendor lock-in。
4. **VLM 增强查询是亮点**：检索到含图上下文时自动联合视觉分析，比纯文本描述 caption 更可靠。
5. **部署门槛不低**：需 LLM API + Embedding + 可选 VLM + MinerU/LibreOffice，适合有基础设施的团队而非开箱即用 demo。

## Related

- [[40 知识导航/AI 工具使用|AI 工具使用]]
