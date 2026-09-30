---
id: source-20260930-ming-image-design
title: Ming-Image-0.1-Design - 视觉设计生成与图层分解
type: source
status: active
created: 2026-09-30
updated: 2026-09-30
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/文生图
  - 主题/UI设计
  - 主题/图层分解
  - 主题/Agent Skills
author: inclusionAI / 蚂蚁百灵
source_type: repo
source_url: https://github.com/inclusionAI/Ming-Image
source_author: inclusionAI（蚂蚁集团百灵）
source_date: 2026-09-30
summary: 蚂蚁百灵开源的视觉设计系列：两个 6B 模型分别做文生 UI/海报等完整设计（含 RGBA）与扁平图可编辑图层分解；配套 Ling UI Design / Image-to-Editable-PPT Skills，MIT 许可。
related: "[[[50 来源资料/代码仓库/AI/UI设计与美学/UI设计风格 Skills 资源清单|UI设计风格 Skills 资源清单]],[[50 来源资料/代码仓库/AI/UI设计与美学/ui-ux-pro-max-skill - UI设计智能 Skill|ui-ux-pro-max-skill]],[[50 来源资料/代码仓库/AI/多模态与模型/GenClaw - 代码驱动 Agent 图像生成|GenClaw]],[[40 知识导航/AI 工具使用|AI 工具使用]]]"
---

# Ming-Image-0.1-Design - 视觉设计生成与图层分解

## Source summary

**Ming-Image-0.1-Design** 是蚂蚁集团百灵（inclusionAI）开源的**视觉设计生成 + 可编辑图层分解**系列。核心价值不是「画得更炫」，而是让 AI 产出能进入真实设计/前端/演示文稿工作流的资产。

- 开源仓库：[inclusionAI/Ming-Image](https://github.com/inclusionAI/Ming-Image)
- 组织：蚂蚁百灵 / inclusionAI
- 语言：Python
- 约 **142 stars / 12 forks**（截至 2026-09-30）
- 许可证：**MIT**
- 触发整理来源：[知乎专栏介绍](https://zhuanlan.zhihu.com/p/2087921395170796957)

系列包含 **两个 6B 参数模型** + **两个 Agent Skills**。

## 系列组成

| 组件 | 作用 |
|------|------|
| **Ming-Image-0.1-Design** | 从文字/结构化提示词生成 UI、信息图、海报等完整视觉设计；原生 RGBA 透明背景 |
| **Ming-Image-0.1-Design-Layer** | 将扁平设计图分解为 2–9 个独立可编辑 RGBA 图层 |
| **Ling UI Design Skill** | 提示词/截图 → 设计参考 → 资产 → 前端代码 → 浏览器视觉校验 |
| **Image-to-Editable-PPT Skill** | 幻灯片/页面图 → 原生可编辑 PPTX（文本/形状/颜色/布局），非单纯贴图 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **文生设计** | 结构化提示词（官方提到可达约 8K tokens），控制布局、排版、配色、图像引用等 |
| **RGBA 原生输出** | 直接生成透明通道资产，可用固定前缀触发（如「带透明通道，4通道RGBA图像」） |
| **语义图层分解** | 非像素抠图，而是设计师友好的语义层（标题/卡片/主体/背景等） |
| **榜单表现** | Design 在 Artificial Analysis UI/UX Design 开放权重榜位居前列；Layer 在 Crello 测试集多项评估表现强，且较 20B Qwen 基线更快 |
| **Agent 闭环** | Skills 把生成接到编码与 PPT 编辑，而不是停在一张静图 |

## 模型与权重

| 模型 | Hugging Face | ModelScope | Demo |
|------|--------------|------------|------|
| Design | [inclusionAI/Ming-Image-0.1-Design](https://huggingface.co/inclusionAI/Ming-Image-0.1-Design) | [ModelScope](https://www.modelscope.cn/models/inclusionAI/Ming-Image-0.1-Design) | [HF Space](https://huggingface.co/spaces/hugging-apps/ming-image-0-1-design-demo) |
| Layer | [inclusionAI/Ming-Image-0.1-Design-Layer](https://huggingface.co/inclusionAI/Ming-Image-0.1-Design-Layer) | [ModelScope](https://www.modelscope.cn/models/inclusionAI/Ming-Image-0.1-Design-Layer) | [HF Space](https://huggingface.co/spaces/Xiaolong-Wang/Ming-Image-0.1-Design-Layer) |

社区另有 ComfyUI 权重包：[Kijai/Ming-Image-ComfyUI](https://huggingface.co/Kijai/Ming-Image-ComfyUI)

## 快速上手（官方 CLI）

```bash
git clone https://github.com/inclusionAI/Ming-Image
cd Ming-Image
pip install -r requirements.txt

# 文生设计
python infer.py \\
  --model inclusionAI/Ming-Image-0.1-Design \\
  --task text-to-image \\
  --prompt assets/t2i_four_seasons_cabin_prompt.json \\
  --width 2048 --height 2048 \\
  --output-dir outputs/t2i

# 图层分解
python infer.py \\
  --model inclusionAI/Ming-Image-0.1-Design-Layer \\
  --task layer-decompose \\
  --input-image assets/layer_samples/card_making_input.png \\
  --prompt assets/layer_samples/card_making_prompt.txt \\
  --resolution 1024 \\
  --output-dir outputs/layers
```

推荐设置（官方）：Design 默认/推荐约 `2048×2048`，采样步数 12，CFG 1.0，BF16；验证过的本地推理配置偏向 **单卡 ≥80 GiB**。部署也可参考 [vLLM-Omni recipes](https://github.com/vllm-project/vllm-omni/blob/main/recipes/inclusionAI/Ming-Image.md)。

提示词增强（PE）可用 `Ling-3.0-flash-VL` 或 `qwen3.8-27B`，先把短需求扩成 Figma 风格结构化 JSON，再喂给 `--prompt`。

## 适用场景

- UI/仪表盘/海报/信息图**快速出稿**与透明素材生产
- 把静图拆成可进 Figma / 合成链路的**图层资产**
- Agent 做「设计 → 代码 / 可编辑 PPT」闭环
- 需要 **MIT** 开放权重、可自托管的设计向生成模型

**不太适合 / 注意**：自由散文式提示词效果通常弱于结构化提示；Layer 分解约 2–9 层，超复杂稿可能合并细节；半透明边缘或需人工微调；Skills 需接入支持 Skill 的 Agent 宿主，单独跑无效；硬件门槛偏高。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/inclusionAI/Ming-Image |
| **知乎介绍** | https://zhuanlan.zhihu.com/p/2087921395170796957 |
| **官方 Blog（微信）** | https://mp.weixin.qq.com/s/VGdtxfM8kbHIQJw50VD_Sw |
| **Ling UI Design Skill** | https://github.com/inclusionAI/ling-cookbook/tree/main/resources/recommended-skills/ling-ui-design |
| **Image-to-Editable-PPT Skill** | https://github.com/inclusionAI/ling-cookbook/tree/main/resources/recommended-skills/image-to-editable-ppt |
| **Design HF** | https://huggingface.co/inclusionAI/Ming-Image-0.1-Design |
| **Layer HF** | https://huggingface.co/inclusionAI/Ming-Image-0.1-Design-Layer |
| **ComfyUI 包** | https://huggingface.co/Kijai/Ming-Image-ComfyUI |

## My takeaways

1. **定位**：设计工作流基础设施——生成 + 可编辑分解 + Skills，而不是通用文生图玩具。
2. **可编辑性优先**：Layer 与 PPT Skill 解决的是「能改、能进工具链」，这比多几分美学分数更实用。
3. **小而专**：6B 专精设计任务，对开源侧「设计生成 × 图层」交叉空白是一次补位；落地时优先学官方结构化提示与 PE 流程。

## Related

- [[50 来源资料/代码仓库/AI/UI设计与美学/UI设计风格 Skills 资源清单|UI设计风格 Skills 资源清单]]
- [[50 来源资料/代码仓库/AI/UI设计与美学/ui-ux-pro-max-skill - UI设计智能 Skill|ui-ux-pro-max-skill - UI设计智能 Skill]]
- [[50 来源资料/代码仓库/AI/UI设计与美学/taste-skill - 反AI土味前端设计 Skill|taste-skill - 反AI土味前端设计 Skill]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
- [[50 来源资料/代码仓库/AI/多模态与模型/GenClaw - 代码驱动 Agent 图像生成|GenClaw - 代码驱动 Agent 图像生成]]