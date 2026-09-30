---
id: source-20260909-minicpm
title: MiniCPM - 端侧大语言模型
type: source
status: active
created: 2026-09-09
updated: 2026-09-09
tags:
  - 来源/代码仓库
  - 主题/大语言模型
  - 主题/端侧AI
  - 主题/本地部署
  - 主题/Agent
author: OpenBMB / 面壁智能
source_type: repo
source_url: https://github.com/OpenBMB/MiniCPM
source_author: OpenBMB（面壁智能 · 清华 NLP · 人大高瓴）
source_date: 2026-09-09
summary: OpenBMB 开源的端侧 / 本地部署 LLM 系列。最新主力为 MiniCPM5-2B（同尺寸开源 SOTA），另含 MiniCPM5-1B、MiniCPM-SALA（百万上下文混合注意力）、MiniCPM4/4.1（稀疏注意力加速）。标准 Llama 架构，配套部署/微调 Cookbook 与 Agent Skills，权重与代码 Apache-2.0。
related:
  - "[[50 来源资料/代码仓库/AI/VoxCPM - 语音克隆 TTS|VoxCPM - 语音克隆 TTS]]"
---

# MiniCPM - 端侧大语言模型

## Source summary

**MiniCPM** 是 OpenBMB（面壁智能）开源的 **端侧 / 资源受限场景大语言模型系列**，口号是 *small yet powerful*。当前主推 **MiniCPM5**（1B / 2B 稠密 Transformer），在同尺寸开源对比中达到 SOTA；仓库还覆盖 **MiniCPM-SALA**（稀疏+线性混合注意力、百万上下文）与 **MiniCPM4 / 4.1**（可训练稀疏注意力 InfLLM-V2、端侧加速）。

- GitHub：`OpenBMB/MiniCPM`，约 **10.6k+ Stars**
- 语言：以文档 / Jupyter / Skills 为主（模型权重在 HuggingFace / ModelScope）
- 协议：**Apache License 2.0**（代码与模型权重）
- 架构亮点（MiniCPM5）：标准 `LlamaForCausalLM`，主流引擎可直接加载，无需自定义算子或模型代码 fork
- Topics / 定位：on-device LLM · 本地部署 · coding agent · 工具调用 · 长上下文

## 核心能力

| 能力 | 说明 |
|------|------|
| **同尺寸 SOTA（MiniCPM5-2B）** | 公开对比平均分约 53.9，可与部分 4B 级模型竞争；代码、数学、长文本、工具调用、Agent 任务优势明显 |
| **端侧友好体量** | MiniCPM5-1B / 2B 面向本地助手、coding agent、资源受限推理 |
| **原生长上下文** | MiniCPM5-2B 上下文 **131,072**；SALA 可达 **1M+** tokens |
| **双模式思考（1B）** | chat template 支持 `enable_thinking`，同一权重可在思考 / 非思考间切换 |
| **工具调用** | MiniCPM5 以 XML 产出 tool call；推荐 SGLang + `minicpm5` parser 转 OpenAI 兼容 `tool_calls` |
| **开放训练数据** | 配套 UltraData 体系开源（UltraX、Code、SFT-Agent、RL 等） |
| **部署 / 微调资产齐全** | 多后端 Cookbook + Cursor/Claude Code 可用的 Agent Skills |
| **多芯片适配** | FlagOS / FlagRelease 已发布多厂商芯片版本（含昇腾、海光等） |

## 系列一览（按当前重点）

| 系列 | 定位 | 代表模型 | 备注 |
|------|------|----------|------|
| **MiniCPM5** | 端侧稠密 SOTA | [MiniCPM5-2B](https://huggingface.co/openbmb/MiniCPM5-2B)、[MiniCPM5-1B](https://huggingface.co/openbmb/MiniCPM5-1B) | 2026-09 / 2026-05；2B 为当前最新主力 |
| **MiniCPM-SALA** | 稀疏+线性混合注意力 | [MiniCPM-SALA](https://huggingface.co/openbmb/MiniCPM-SALA) | 百万上下文；相对稠密基线约 3.5× 推理加速 |
| **MiniCPM4 / 4.1** | 可训练稀疏注意力 + 混合思考 | [MiniCPM4.1-8B](https://huggingface.co/openbmb/MiniCPM4.1-8B)、[MiniCPM4-8B](https://huggingface.co/openbmb/MiniCPM4-8B) | InfLLM-V2；端侧芯片上长文本生成可显著加速；推荐 [CPM.cu](https://github.com/OpenBMB/CPM.cu) |
| **早期型号** | 历史开源 | MiniCPM3-4B、MiniCPM-2B、MoE、BitCPM4 等 | 见仓库 README 折叠区 / `docs/README-legacy-cn.md` |

### MiniCPM5-2B 规格速览

| 项 | 值 |
|----|-----|
| 架构 | 标准 `LlamaForCausalLM` |
| 参数量 | 约 2.52B（非 Embedding ≈ 1.98B） |
| 层数 | 42 |
| 注意力（GQA） | Q 16 / KV 2 |
| 上下文 | 131,072 |
| 后训练要点 | SFT → RL（JustRL II critic）→ OPD 蒸馏合并多专家；推理/通用约 ↑10.96，Agent 约 ↑6.96 |

提供 BF16 / GGUF / MLX / GPTQ / DSpark 等多种格式。

## 部署与微调（Cookbook + Skills）

MiniCPM5 官方强调：**无需 fork 模型代码**。仓库提供分后端单页 cookbook，以及供 coding agent 使用的 Skills。

**部署后端示例：** Transformers · vLLM · SGLang · llama.cpp · Ollama · LM Studio · MLX · ArcLight · vLLM Ascend

**微调框架示例：** TRL+PEFT · LLaMA-Factory · ms-swift · unsloth

快速体验（vLLM）：

```bash
pip install "vllm>=0.21"
vllm serve openbmb/MiniCPM5-2B --port 8000
```

推荐采样参数：`temperature=1.0, top_p=0.95`。工具调用优先用 SGLang（`--tool-call-parser minicpm5`）。

## 适用场景

- **本地 / 端侧助手**：希望在消费级或边缘设备上跑可用中文+代码能力的小模型
- **Coding Agent / 工具调用**：需要紧凑模型做本地 agent、XML tool call + OpenAI 兼容服务
- **长上下文任务**：SALA / MiniCPM4.1 路线做超长文档或百万级上下文推理
- **可复现研究 / 数据闭环**：需要 UltraData 开源语料与完整训练配方参考
- **国产 / 多芯片部署**：关注 FlagOS 多厂商适配或昇腾 vLLM

## 限制与注意

- 仓库内容以 **模型文档与工程资产** 为主，权重在 HF / ModelScope，需单独下载
- MiniCPM4 系高效推理更依赖 **CPM.cu / 稀疏算子** 等配套；MiniCPM5 则更「标准架构、开箱即用」
- 语言模型输出不代表开发者立场；商用与安全合规需自行评估
- 多模态视觉能力在姊妹仓 **MiniCPM-V**，本仓以文本 LLM 为主

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/OpenBMB/MiniCPM |
| **中文 README** | https://github.com/OpenBMB/MiniCPM/blob/main/README-cn.md |
| **技术报告 (MiniCPM4)** | https://arxiv.org/pdf/2506.07900 |
| **MiniCPM 知识库（飞书）** | https://modelbest.feishu.cn/wiki/UtWxwcERfiRIpIkBOjuc3h9tn1D |
| **MiniCPM-V（多模态姊妹仓）** | https://github.com/OpenBMB/MiniCPM-V |
| **UltraData** | https://ultradata.openbmb.cn/ |
| **在线 Demo（MiniCPM5-2B）** | https://huggingface.co/spaces/openbmb/MiniCPM5-2B-Demo |
| **HF MiniCPM5-2B** | https://huggingface.co/openbmb/MiniCPM5-2B |
| **HF MiniCPM5-1B** | https://huggingface.co/openbmb/MiniCPM5-1B |
| **HF MiniCPM-SALA** | https://huggingface.co/openbmb/MiniCPM-SALA |
| **ModelScope MiniCPM5-2B** | https://www.modelscope.cn/models/OpenBMB/MiniCPM5-2B |
| **部署 Cookbook 目录** | https://github.com/OpenBMB/MiniCPM/tree/main/docs/deployment |
| **微调 Cookbook 目录** | https://github.com/OpenBMB/MiniCPM/tree/main/docs/finetune |
| **Agent Skills** | https://github.com/OpenBMB/MiniCPM/tree/main/skills |
| **CPM.cu（4.x 高效推理）** | https://github.com/OpenBMB/CPM.cu |
| **面壁智能** | https://modelbest.cn/ |
| **Discord** | https://discord.gg/3cGQn9b3YM |

## My takeaways

1. **选型锚点**：要「小而强、标准架构、本地部署」→ 优先看 **MiniCPM5-2B**；要「极致长上下文效率」→ **SALA**；要「8B 级端侧稀疏加速」→ **MiniCPM4.1 + CPM.cu**。
2. **工程友好度高**：MiniCPM5 走标准 Llama 路径，vLLM/SGLang/Ollama/llama.cpp 等路径齐全，还有 Skills，适合快速落地而非只看论文。
3. **数据与训练透明度**：UltraData + RL/OPD 配方开源，便于复现与二次训练，不只是发权重。
4. **同生态可联动**：同团队有 [[50 来源资料/代码仓库/AI/VoxCPM - 语音克隆 TTS|VoxCPM]]（TTS）与 MiniCPM-V（视觉）；做本地 AI 链路时可一并检索。
5. **工具调用记清路径**：生产侧工具调用优先 **SGLang + minicpm5 parser**，避免自己解析 XML。

## Related

- [[50 来源资料/代码仓库/AI/VoxCPM - 语音克隆 TTS|VoxCPM - 语音克隆 TTS]]
