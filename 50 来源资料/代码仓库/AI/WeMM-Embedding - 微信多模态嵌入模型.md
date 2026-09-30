---
id: source-20260901-wemm-embedding
title: WeMM-Embedding - 微信多模态嵌入模型
type: source
status: active
created: 2026-09-01
updated: 2026-09-01
tags:
  - 来源/代码仓库
  - 主题/多模态
  - 主题/嵌入模型
  - 主题/检索
  - 主题/视觉语言模型
author: Tencent / WeChat Vision
source_type: repo
source_url: https://github.com/Tencent/WeMM-Embedding
source_author: 腾讯微信视觉团队（WeChat Vision）
source_date: 2026-09-01
summary: 腾讯微信视觉团队开源的通用多模态嵌入模型族，统一表征文本/图像/视频/视觉文档与交错多模态输入，支持 Matryoshka 可变维度，在 MMEB-v2/v3 等基准上达到 SOTA，可用于多模态检索与理解。
related:
  - "[[50 来源资料/代码仓库/AI/RAG-Anything - 多模态一体化RAG框架|RAG-Anything - 多模态一体化RAG框架]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# WeMM-Embedding - 微信多模态嵌入模型

## Source summary

**WeMM-Embedding**（WeChat Multi-Modal Embedding）是腾讯微信视觉团队开源的 **通用多模态嵌入模型族**。它为文本、图像、视频、视觉文档（VisDoc）以及交错多模态输入提供统一向量表征，面向多模态理解与检索，并在多个覆盖不同任务与领域的基准上达到 SOTA。

- GitHub：`Tencent/WeMM-Embedding`，约 **1.0k+ Stars**
- 语言：Python
- 协议：腾讯原创代码为 **Apache License 2.0**（第三方组件保留各自协议）
- 技术报告：[arXiv:2608.24053](https://arxiv.org/abs/2608.24053) / 仓库内 PDF：`assets/WeMM_Embedding_tech_report.pdf`
- Topics：`embedding-models` · `multimodal` · `multimodal-llm`

## 核心能力

| 能力 | 说明 |
|------|------|
| **统一多模态表征** | 文本、图像、视频、视觉文档、交错多模态输入共用一套嵌入空间 |
| **模型族（2B / 4B / 9B）** | 按规模与维度需求选型，均可处理上述模态 |
| **Matryoshka（MRL）可变维度** | 可截断到更小维度后重新 L2 归一化，兼顾效果与存储/检索成本 |
| **推理友好** | 支持 Transformers、Sentence Transformers；推荐 `transformers==5.2.0` 以复现预处理行为 |
| **服务化部署** | 提供 vLLM / SGLang 服务示例与脚本封装 |
| **评测配套** | 含 MMEB-v3 评测代码（基于 VLM2Vec 管线的最小改动） |

> 注意：**当前不支持音频输入**；MMEB-v3 中 Audio 相关任务记为 0。

## Model Zoo

| 模型 | Matryoshka 维度 | Hugging Face |
|------|-----------------|--------------|
| WeMM-Embedding-2B | `64, 128, 256, 512, 1024, 2048` | [tencent/WeMM-Embedding-2B](https://huggingface.co/tencent/WeMM-Embedding-2B) |
| WeMM-Embedding-4B | `64, 128, 256, 512, 1024, 2560` | [tencent/WeMM-Embedding-4B](https://huggingface.co/tencent/WeMM-Embedding-4B) |
| WeMM-Embedding-9B | `64, 128, 256, 512, 1024, 2048, 4096` | [tencent/WeMM-Embedding-9B](https://huggingface.co/tencent/WeMM-Embedding-9B) |

嵌入取自专用 `<emb>` token 位置的最后一层隐状态，再做 L2 归一化。

## 性能速览（官方报告）

### MMEB-v2（78 数据集）

图像/视频任务用 Hit@1，视觉文档用 NDCG@5；越高越好。

| 模型 | Size | AVG | Image | Video | VisDoc |
|------|-----:|----:|------:|------:|-------:|
| Qwen3-VL-Embedding | 2B | 73.2 | 75.0 | 61.9 | 79.2 |
| **WeMM-Embedding** | **2B** | **77.9** | **79.6** | **70.8** | **80.7** |
| **WeMM-Embedding** | **4B** | **79.2** | **80.8** | **72.1** | **82.0** |
| Qwen3-VL-Embedding | 8B | 77.8 | 80.1 | 67.1 | 82.4 |
| **WeMM-Embedding** | **9B** | **80.6** | **81.9** | **74.3** | **83.3** |

MRL 压缩示例：2B 模型在 **256 维**时，图像与视频性能仍可保留全维约 **98.7%**。

### MMEB-v3（190 任务）

V3-All 含文本 / Agent / MCMR / Audio 等；不支持的任务计 0。

| 模型 | Size | V3-All | Text | Agent | MCMR | Audio |
|------|-----:|-------:|-----:|------:|-----:|------:|
| Qwen3-VL-Embedding | 2B | 50.9 | 39.2 | 39.3 | 42.0 | 0.0 |
| **WeMM-Embedding** | **2B** | **56.0** | **45.3** | **45.1** | **42.5** | **0.0** |
| **WeMM-Embedding** | **4B** | **58.2** | **47.9** | **49.0** | **41.9** | **0.0** |
| Qwen3-VL-Embedding | 8B | 53.5 | 42.5 | 38.4 | 38.0 | 0.0 |
| **WeMM-Embedding** | **9B** | **59.5** | **48.8** | **51.0** | **49.3** | **0.0** |

## 快速开始

```bash
pip install -r requirements.txt
```

### Transformers

```bash
python examples/transformers_inference.py \\
  --model /path/to/WeMM-Embedding-2B \\
  --image /path/to/image.jpg \\
  --video /path/to/video.mp4 \\
  --dimension 2048
```

省略 `--dimension` 则使用完整维度。

### Sentence Transformers

```bash
python examples/sentence_transformers_inference.py \\
  --model tencent/WeMM-Embedding-2B \\
  --image /path/to/image.jpg \\
  --video /path/to/video.mp4 \\
  --dimension 2048
```

也可直接使用 Hugging Face model id；文本/图/视频经 `SentenceTransformer.encode()`，MRL 用 `--dimension` 选择。

### Matryoshka 截断

```python
embedding = torch.nn.functional.normalize(embedding[..., :d], dim=-1)
```

### Serving（已测版本）

- vLLM `0.27.0`：`--runner pooling` + `embedding_chat_template.jinja`
- SGLang `0.5.9`：需先跑 `scripts/patch_sglang_video.py`，再以 `--is-embedding` 启动

封装脚本：`scripts/serve_vllm.sh`、`scripts/serve_sglang.sh`。

## 适用场景

- **多模态检索**：图搜文、文搜图、视频检索、视觉文档（扫描件/版面）检索
- **统一向量库**：希望文本/图/视频进同一 embedding 空间，减少多塔模型维护成本
- **成本敏感检索**：用 MRL 降维（如 256）换存储与索引开销，同时尽量保性能
- **需要公开权重与可复现评测**的多模态 embedding 选型对比（相对部分仅榜单、不发权重的闭源提交）

## 限制与注意

- **无音频**：不适合语音/音频检索主路径
- **依赖与版本敏感**：官方建议固定 `transformers==5.2.0`；服务侧注意 vLLM/SGLang 版本
- **算力门槛**：2B/4B/9B 推理与视频帧采样（评测侧提到 64 帧）对 GPU 有要求
- 第三方依赖请单独核对各自许可证

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/Tencent/WeMM-Embedding |
| **技术报告 (arXiv)** | https://arxiv.org/abs/2608.24053 |
| **HF 2B** | https://huggingface.co/tencent/WeMM-Embedding-2B |
| **HF 4B** | https://huggingface.co/tencent/WeMM-Embedding-4B |
| **HF 9B** | https://huggingface.co/tencent/WeMM-Embedding-9B |
| **Issues** | https://github.com/Tencent/WeMM-Embedding/issues |
| **MMEB-v3 评测说明** | 仓库 `mmeb_v3_eval/README.md` |
| **上游评测管线参考** | https://github.com/TIGER-AI-Lab/VLM2Vec |

## My takeaways

1. **定位清晰**：不是 RAG 框架，而是可直接接入检索系统的 **多模态 embedding 底座**；与 [[50 来源资料/代码仓库/AI/RAG-Anything - 多模态一体化RAG框架|RAG-Anything]] 等框架互补（后者偏文档解析+图谱，前者偏统一向量）。
2. **公开权重 + 强榜单**：在 MMEB-v2/v3 上相对 Qwen3-VL-Embedding 等同尺寸开源方案有明显优势，适合作为多模态检索候选模型。
3. **MRL 实用价值高**：256 维仍能保留绝大部分图/视频性能，利于大规模索引落地。
4. **模态边界要记牢**：文本/图/视频/VisDoc 强，**音频空白**；若业务含语音需另配音频 embedding。
5. **工程路径齐全**：Transformers / Sentence-Transformers / vLLM / SGLang 示例齐全，从实验到 serving 成本较低。

## Related

- [[50 来源资料/代码仓库/AI/RAG-Anything - 多模态一体化RAG框架|RAG-Anything - 多模态一体化RAG框架]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]