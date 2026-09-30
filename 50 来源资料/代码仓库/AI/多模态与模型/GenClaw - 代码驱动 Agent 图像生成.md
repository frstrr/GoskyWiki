---
id: source-20260924-genclaw
title: GenClaw - 代码驱动 Agent 图像生成
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/图像生成
  - 主题/文生图
  - 主题/开源资源
author: Ye Junyan (yejy53) et al.
source_type: repo
source_url: https://github.com/yejy53/GenClaw
source_author: yejy53
source_date: 2026-09-24
summary: 代码驱动的 Agent 图像生成：用 SVG/HTML/Python 等可执行草图做可控画布，再调用文生图/图生图模型渲染；规划-工具-反思闭环，适合复杂构图、长文本海报与世界知识 grounding。
related:
  - [[50 来源资料/代码仓库/AI/UI设计与美学/img2threejs - 图片转程序化 Three.js 模型|img2threejs]]
  - [[50 来源资料/代码仓库/AI/多模态与模型/OpenMontage - Agent 化视频制作系统|OpenMontage]]
  - [[50 来源资料/代码仓库/AI/UI设计与美学/handraw-style - 手绘风格编号画廊与双语提示词|handraw-style]]
  - [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
  - [[40 知识导航/AI 工具使用|AI 工具使用]]
---

# GenClaw - 代码驱动 Agent 图像生成

## Source summary

**GenClaw**（[yejy53/GenClaw](https://github.com/yejy53/GenClaw)）探索 **code-driven agentic image generation**：图像生成 Agent 不只改写 prompt，而是先用代码画出可控视觉草图，再调用图像模型做最终渲染。

核心一句话：**先想、用代码草图、再渲染（think → sketch with code → render）**。

- 约 **358 stars / 11 forks**（截至 2026-09-24 GitHub API）
- 语言：**Python**；许可证：**MIT**
- 论文：[arXiv:2605.30248](https://arxiv.org/abs/2605.30248) · [Hugging Face Papers](https://huggingface.co/papers/2605.30248)
- 同作者相关：[Editable-Design](https://github.com/yejy53/Editable-Design)（可编辑视觉设计，2026-08 新闻）

## 解决什么问题

| 痛点 | GenClaw 方案 |
|------|-------------|
| 纯扩散采样隐式、难控数量/布局/文字 | **代码当画笔**：SVG / HTML·CSS / Python / 轻量 3D，把对象计数、空间布局、文字渲染变成可执行、可验证、可调试程序 |
| 一次黑盒出图难迭代 | 对齐人类创作环：构思 → 草图 → 上色 → 精修；中间产物可检查、可改、可回退 |
| 生图能力游离在 Agent 工具箱外 | **Agent Harness for Image Generation**：把规划、工具调用、反思接到图像合成上 |

## Highlights（仓库自述）

1. **Code as a Visual Brush**：合成从隐式采样转向显式、可推理的过程。
2. **Draw as a Human Artist**：构思、检索参考、起草、增量渲染全程可见。
3. **Agent Harness**：让「做图」成为 Agent 一等能力，而非孤立模型。

## 适用场景（Showcase 方向）

- **复杂场景构图**：多物体、计数与空间关系敏感的任务
- **文字渲染 / 海报设计**：长文、版式敏感（code_text_draft）
- **物理推理**类可视化
- **世界知识 grounding**：先检索再生成（search + Tavily）

## 可运行 Agent 实现（仓库现状）

规划 LLM 在工具循环中工作：任务前由 perception 子 LLM 写描述性「画家笔记」（意图 + 规划提示，非路由决策）；规划器读请求、笔记与工具卡，用 todo_write 建计划，再组合规范工具链。

**工具摘要**：

| 工具 | 作用 |
|------|------|
| t2i / i2i | 文生图 / 图生图 |
| code_scene_draft | SVG 布局草稿 |
| code_text_draft | 逐字长文本渲染 |
| search | Tavily 网页/图片检索 |
| reason | 多模态推理 |
| format_prompt / vlm_review | 提示整理 / VLM 审阅 |
| todo_write / tool_search | 规划辅助 |

**典型工具链示例**：

- 空间/计数：code_scene_draft → i2i
- 实体 grounding：search → format_prompt → i2i
- 科学推导：reason → t2i
- 世界知识长文：search → code_text_draft

每次运行写入 runs/YYYYMMDD-HHMMSS-XXXXXX/：trace.jsonl + 各工具中间产物（SVG、检索参考、最终 PNG）。

## 快速开始

安装与环境：

1. git clone https://github.com/yejy53/GenClaw.git && cd GenClaw
2. python -m venv .venv 并激活；pip install -e .（或 conda env create -f environment.yml && conda activate cc-genclaw）
3. playwright install chromium
4. cp config.example.yaml config.yaml

配置要点（config.yaml，gitignore，勿提交密钥）：

| 配置 | 含义 |
|------|------|
| OPENAI_BASE_URL + OPENAI_API_KEY | 统一 OpenAI 兼容网关 |
| OPENAI_MODEL_NAME | 主规划器与子 LLM |
| GEMINI_I2I_MODEL_NAME | 图像生成/编辑模型 |
| TAVILY_API_KEY | search 工具（世界知识任务） |
| 可选 GEMINI_BASE_URL / GEMINI_API_KEY | 把图像模型放到独立端点 |

Web UI（推荐）：

```
chainlit run src/cc_genclaw/ui/chainlit_app.py -w
```

Python 调用骨架：prepare_agent_run → initialize_perception → run_prompt，产物在 run.session.dir。

## 仓库结构（摘要）

```
src/cc_genclaw/
├── agent.py / loop.py     # Agent 主循环
├── config.py / llm.py     # 配置 + OpenAI 兼容客户端
├── perception/            # 画家笔记（非路由）
├── runtime/               # 会话、产物、runner
├── tools/                 # t2i/i2i、code draft、reason、search 等
├── prompts/               # system + perception + tool cards
└── ui/                    # Chainlit
```

## 链接

| 资源 | 链接 |
|------|------|
| GitHub | https://github.com/yejy53/GenClaw |
| 论文（HF） | https://huggingface.co/papers/2605.30248 |
| arXiv | https://arxiv.org/abs/2605.30248 |
| 相关：Editable-Design | https://github.com/yejy53/Editable-Design |
| 许可证 | MIT |

## Citation

```
@article{ye2026genclaw,
  title={GenClaw: Code-Driven Agentic Image Generation},
  author={Ye, Junyan and others},
  journal={arXiv preprint arXiv:2605.30248},
  year={2026}
}
```

## My takeaways

1. **定位差异**：不是又一个文生图 API 包装，而是「代码草图 + Agent 工具环」把布局/计数/文字变成可编程中间态。
2. **与 img2threejs 互补**：img2threejs 偏「图→可运行 3D 代码」；GenClaw 偏「需求→代码草图→最终像素图」。
3. **落地成本**：需要 OpenAI 兼容网关、图像模型与（可选）Tavily；还要 Playwright Chromium 跑 code-draft 渲染。
4. **适合研究/产品化参考**：复杂构图与长文海报是其宣传强项；商用前评估模型供应商与 MIT 依赖链即可。

## Related

- [[50 来源资料/代码仓库/AI/UI设计与美学/img2threejs - 图片转程序化 Three.js 模型|img2threejs - 图片转程序化 Three.js 模型]]
- [[50 来源资料/代码仓库/AI/多模态与模型/OpenMontage - Agent 化视频制作系统|OpenMontage - Agent 化视频制作系统]]
- [[50 来源资料/代码仓库/AI/UI设计与美学/handraw-style - 手绘风格编号画廊与双语提示词|handraw-style - 手绘风格编号画廊与双语提示词]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]