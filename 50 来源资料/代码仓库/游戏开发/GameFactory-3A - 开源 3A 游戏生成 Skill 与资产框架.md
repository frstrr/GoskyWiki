---
id: source-20260929-gamefactory-3a
title: GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架
type: source
status: active
created: 2026-09-29
updated: 2026-09-29
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/AI
  - 主题/Agent
  - 主题/资产生成
  - 主题/Unity
  - 主题/UE5
  - 主题/开源资源
author: OpenDCAI
source_type: repo
source_url: https://github.com/OpenDCAI/GameFactory-3A
source_author: OpenDCAI
source_date: 2026-09-29
summary: 开源 3A 游戏生成 Skill 与资产框架。由 Coding Agent 读取 Skills 并调用 Pipeline，把游戏需求变成生产级资产与引擎可运行代码；覆盖图片/3D/动作/音频/CG 视频，支持 UE5、Blender、Unity、Godot 4、three.js。
related:
  - [[40 知识导航/游戏开发|游戏开发]]
  - [[30 知识资源/游戏开发/Mixamo - 免费人形角色动画平台|Mixamo]]
  - [[50 来源资料/代码仓库/AI/img2threejs - 图片转程序化 Three.js 模型|img2threejs]]
  - [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
---

# GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架

## Source summary

**3AGameFactory / GameFactory-3A**（[OpenDCAI/GameFactory-3A](https://github.com/OpenDCAI/GameFactory-3A)）是一套面向 Coding Agent 的开源 **3A 游戏生成 Skill 与资产框架**：输入游戏需求，由 Agent 读取 Skills、调用 Pipeline，产出可用于构建的游戏资产与引擎侧代码。

核心一句话：**把游戏需求变成生产可用资产 + 引擎可运行代码。**

- 约 **819 stars / 78 forks**（截至 2026-09-29 GitHub API）
- 语言：**Python**；许可证：**Apache-2.0**
- 默认分支：`main`
- 组织：[OpenDCAI](https://github.com/OpenDCAI)
- 文档：中英文 README（`README.md` / `README_zh.md`）

覆盖能力：图片、3D 资产、动作、音频、CG 视频；游戏构建引擎支持 **UE5、Blender、Unity、Godot 4、three.js**。

## 核心特性

| 特性 | 说明 |
|------|------|
| **Agent 驱动工作流** | 由 Codex / Claude Code / Gemini CLI 等读取 `agent_skills/`，再调用对应 Pipeline |
| **全链路资产生成** | 图片与 T-pose、3D 物体、3D 场景、动作、音频、CG 视频 |
| **玩法与 UI 代码生成** | `gen_mechanic`（引擎原生机制）、`gen_ui`（HUD/菜单/交互） |
| **多引擎适配** | UE5 / Blender / Unity / Godot 4 / three.js 均有 Agent 上下文与参考实现 |
| **清晰入口文档** | `agent_skills/setting_overview.md` 作为游戏生成 Agent 的总路由入口 |
| **贡献者 Harness** | `agent_skills/develop_harness/` 提供 models → operators → pipeline 契约与 CPU smoke |

## 能力地图

| 能力 | 产物 | 主要 Pipeline |
|------|------|---------------|
| 图片与 T-pose 预处理 | 源图像、角色可用输入 | `pipeline/assets_gen/gen_tpose_image/` |
| 3D 物体生成 | 道具、角色、武器、可复用网格 | `pipeline/assets_gen/gen_3d_object/` |
| 3D 场景生成 | 室内重建或组合式环境 | `pipeline/assets_gen/gen_3d_scene/` |
| 动作生成 | 骨骼、生成动作、重定向剪辑 | `pipeline/assets_gen/gen_motion/` |
| 音频生成 | 对话、SFX、环境声、WAV | `pipeline/assets_gen/gen_audio/` |
| CG 视频生成 | 文本/首帧/参考驱动的 MP4 | `pipeline/assets_gen/gen_cg_video/` |
| 玩法生成 | 引擎原生机制与运行时行为 | `pipeline/code_gen/gen_mechanic/` |
| UI 生成 | HUD、菜单、界面与交互 | `pipeline/code_gen/gen_ui/` |
| 完整游戏切片 | 资产 + 玩法 + UI + 评测协同 | Agent 按 `setting_overview.md` 编排 |

## 支持的引擎

| 引擎 | Agent 上下文 | 参考实现 |
|------|--------------|----------|
| UE5 | `agent_skills/engine_context/ue5_api.md` | `engine_adapters/ue5/` |
| Blender | `agent_skills/engine_context/blender_api.md` | `engine_adapters/blender/` |
| Unity | `agent_skills/engine_context/unity3d_api.md` | `engine_adapters/unity3d/` |
| Godot 4 | `agent_skills/engine_context/godot_api.md` | `engine_adapters/godot/` |
| three.js | `agent_skills/engine_context/three_js_api.md` | `engine_adapters/three_js/` |

官方演示覆盖：对战、FPS、赛车、RPG 探索等；资产来源示例包括 Meshy、Hunyuan3D、Mixamo、引擎自带/开源资产库；CG 视频示例提到本地 MiniMax H3（720P）及可选 Seedance 等云端模型。

## 适用边界

### 更适合

- 想用 Coding Agent 从需求快速产出可玩切片 / 原型资产的团队
- 需要统一管线覆盖图像→3D→动作→音频→CG，并落到 UE5/Unity/Godot/Blender/three.js
- 研究或落地「Agent Skills + 游戏资产生成 Pipeline」架构
- 需要参考多引擎 Adapter 与引擎 API 上下文文档

### 需要注意

- 依赖本机/云端模型权重、GPU 与各类第三方引擎/素材许可证，商用前需逐项核对
- 「生成游戏」与「给框架本身贡献代码」是两条路径；后者应从 `develop_harness` 与 CPU smoke 起步
- 演示中部分角色/场景仍混用开源资产与生成资产，不宜默认等同「全自动 3A 量产」

## 快速开始

```text
1. 打开 Coding Agent（Codex / Claude Code / Gemini CLI 等）
2. cd GameFactory-3A
3. 给出游戏需求，并要求 Agent 先阅读 agent_skills/setting_overview.md
```

贡献框架本身：从 `agent_skills/develop_harness/README.md` 开始，先跑 CPU-only smoke，再接模型权重或 GPU。

## 仓库结构（摘要）

```text
GameFactory-3A/
├── agent_skills/        # Agent 工作流、QA Skill、引擎 API 上下文
│   ├── setting_overview.md   # 游戏生成入口
│   ├── asset_qa/ / code_gen/ / develop_harness/ / engine_context/
├── engine_adapters/     # 各引擎参考实现与公开 Adapter API
├── models/              # 本地/云模型封装
├── operators/           # 组合模型的任务逻辑
├── pipeline/
│   ├── assets_gen/      # 图片/3D/场景/动作/音频/CG
│   ├── code_gen/        # 玩法与 UI 代码生成
│   └── common/paths.py  # 输入输出路径唯一来源
├── scripts/             # 环境配置、引擎安装与启动
├── test/ / test_data/   # 契约/集成/smoke 与示例需求；产物在 test_data/outputs/
└── third_party/         # 外部依赖仓（如 trimesh、引擎素材包）
```

生成产物按「游戏 / 运行 / 任务类别 / 任务 ID」落在 `test_data/outputs/`；应通过 `pipeline/common/paths.py` 取路径，避免手写拼接。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/OpenDCAI/GameFactory-3A |
| **英文 README** | https://github.com/OpenDCAI/GameFactory-3A/blob/main/README.md |
| **中文 README** | https://github.com/OpenDCAI/GameFactory-3A/blob/main/README_zh.md |
| **Agent 入口** | https://github.com/OpenDCAI/GameFactory-3A/blob/main/agent_skills/setting_overview.md |
| **贡献 Harness** | https://github.com/OpenDCAI/GameFactory-3A/blob/main/agent_skills/develop_harness/README.md |
| **Unity 引擎上下文** | https://github.com/OpenDCAI/GameFactory-3A/blob/main/agent_skills/engine_context/unity3d_api.md |
| **组织主页** | https://github.com/OpenDCAI |
| **Issues** | https://github.com/OpenDCAI/GameFactory-3A/issues |
| **许可证** | Apache-2.0 |

## Citation

```bibtex
@misc{gamefactory3a,
  title        = {3AGameFactory: Open-Source 3A Game Generation Skills and Asset Framework},
  author       = {OpenDCAI},
  year         = {2026},
  howpublished = {\\url{https://github.com/OpenDCAI/GameFactory-3A}},
  note         = {Open-source software repository}
}
```

## My takeaways

1. **定位**：不是单一生图/生 3D 工具，而是「Coding Agent Skills + 多模态资产 Pipeline + 多引擎 Adapter」的游戏生成框架。
2. **与 unicore 的关系**：unicore 是 Unity 游戏框架；本仓库在 Unity 侧有 `engine_adapters/unity3d/` 与 `unity3d_api.md`，适合作为「Agent 生成 Unity 切片 / 资产」的上游参考，而非直接替代 unicore 运行时。
3. **与 img2threejs 互补**：img2threejs 偏「参考图 → 程序化 Three.js 代码」；GameFactory-3A 偏「游戏需求 → 多引擎资产与玩法/UI 代码」，覆盖面更广、落地依赖更重。
4. **动作链路可对齐 Mixamo**：官方演示大量使用 Mixamo 作角色/动作来源，可与已有 Mixamo 知识资源对照。
5. **落地成本**：环境脚本、模型权重、GPU、第三方许可证齐全后才适合严肃试用；先读 `setting_overview.md`，贡献则走 `develop_harness`。
6. **检索关键词**：3A 游戏生成、Agent Skill、资产生成、Unity/UE5 代码生成、CG 视频、OpenDCAI。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[30 知识资源/游戏开发/Mixamo - 免费人形角色动画平台|Mixamo - 免费人形角色动画平台]]
- [[50 来源资料/代码仓库/AI/img2threejs - 图片转程序化 Three.js 模型|img2threejs - 图片转程序化 Three.js 模型]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
