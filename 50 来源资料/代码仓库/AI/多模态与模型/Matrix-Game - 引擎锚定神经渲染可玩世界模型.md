
---
id: source-20261019-matrix-game
title: Matrix-Game - 引擎锚定神经渲染可玩世界模型
type: source
status: active
created: 2026-10-19
updated: 2026-10-19
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/世界模型
  - 主题/视频生成
  - 主题/神经渲染
  - 主题/游戏开发
  - 主题/开源资源
aliases:
  - Matrix-Game
  - Matrix-Game 4.0
  - Skywork Matrix-Game
author: Skywork AI (昆仑万维)
source_type: repo
source_url: https://github.com/SkyworkAI/Matrix-Game
source_author: SkyworkAI
source_date: 2026-10-19
summary: 昆仑万维 Skywork AI 开源的交互式世界模型系列。4.0 将可执行游戏引擎与因果视频模型结合：引擎负责玩法/物理/输入，模型对流式低模 RGB 做神经渲染；支持消费级 RTX 50 系单卡实时可玩，同源开源推理、浏览器 Demo 与游戏源码。
related:
  - "[[50 来源资料/代码仓库/AI/多模态与模型/InSpatio-World - 实时4D世界模拟器|InSpatio-World]]"
  - "[[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]"
  - "[[40 知识导航/游戏开发|游戏开发]]"
---

# Matrix-Game - 引擎锚定神经渲染可玩世界模型

## Source summary

**Matrix-Game**（[SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game)）是昆仑万维 **Skywork AI** 开源的交互式世界模型系列。最新 **Matrix-Game 4.0** 的核心思路是：

> **代码搭建游戏，AI 实时渲染世界** — 游戏引擎维护世界状态并输出廉价低模代理画面；Coding Agent 把玩家意图写成新规则与提示词；少步数因果视频模型在单张消费级 GPU 上将每个块渲染成成品画面。

- **仓库**：https://github.com/SkyworkAI/Matrix-Game
- **项目页**：https://matrix-game-v4-0.github.io/page/
- **模型与资产**：https://huggingface.co/Skywork/Matrix-Game-4.0
- **许可证**：4.0 代码 **Apache-2.0**（早期版本目录保留各自许可；模型/资产另有声明）
- **触发收录**：头条介绍 [昆仑万维诞生世界上第一款消费级显卡视频大模型游戏｜同源开源](https://www.toutiao.com/article/7696803392270549544/)

与「纯像素外推」的世界模型不同：4.0 用 **Harness** 包住视频模型，由引擎显式持有可玩状态，模型只负责神经渲染。

## 核心能力

| 能力 | 说明 |
|------|------|
| **引擎锚定玩法** | 玩法、物理、输入留在可执行引擎；模型不隐式「猜」规则 |
| **流式神经渲染** | 因果少步视频模型（1～2 步去噪）按块生成，条件含低模 RGB、动态 caption、参考帧 |
| **三场景模型** | Medieval Castle / Ancient Egypt（第三人称射击）+ City Racing（驾驶） |
| **消费级 GPU 实时** | 官方给出 RTX 5090 / 5080 / 5070 Ti 优化预设；5080-sage 约 67 FPS、5070 Ti-sage 约 58 FPS（864×480，稳态、不含视频 I/O） |
| **同源开源** | 推理代码、浏览器 Demo、游戏源码与权重/资产分发同发 |
| **Coding Agent 扩展** | 可先改地形/NPC/规则并本地试玩（无需加载权重），再接入渲染管线 |

## 版本脉络

| 版本 | 时间（官方 News） | 定位 |
|------|-------------------|------|
| 1.0 | 2025-05 | 系列首个开源世界模型 |
| 2.0 | 2025-08 | 实时交互长视频生成 |
| 3.0 | 2026-03 | 实时流式交互 + 长程记忆 |
| **4.0** | **2026-10** | 引擎锚定 + 流式神经渲染；消费级卡可玩 |

## 架构要点（4.0）

1. **Game engine**：按规则演化状态，输出低模代理帧（proxy）
2. **Coding Agent**：规则管不到的地方写成新规则注册进引擎，并检索状态写 prompt
3. **Video model**：读 proxy + 动态描述 + 参考帧；因果 DiT 每步生成 1 latent（4 帧）
4. **呈现层**：HUD / 音效与模型解耦，保持帧对齐

项目页强调：只靠逐帧外推像素时世界状态是隐式的；4.0 刻意用引擎持有状态，避免「出视野即遗忘」。

## 快速上手（摘要）

```bash
git clone https://github.com/SkyworkAI/Matrix-Game.git
cd Matrix-Game/Matrix-Game-4
python3.12 -m venv .venv && source .venv/bin/activate
# 安装依赖见官方 README（PyTorch / CUDA 版本与 preset 绑定）
# 从 Hugging Face 下载场景权重与游戏资产到 resources/
matrix-game download --manifest releases/models.json \
  --repo-id Skywork/Matrix-Game-4.0 --scene ancient-egypt --resources resources

# 离线渲染低模视频
matrix-game render --scene ancient-egypt --input lowpoly.mp4 \
  --resources resources --preset realtime --prompt-mode fixed --output outputs/egypt
```

本地只玩引擎、不加载模型：

```bash
bash scripts/build_game.sh
bash scripts/play.sh --scene ancient-egypt
```

硬件注意：模型推理官方主路径为 **Linux + NVIDIA GPU + Python 3.12**；浏览器客户端可不带 GPU。单用户可信 Demo，默认绑 loopback，勿直接暴露公网。

## 场景与资源

| 场景 | Hugging Face 路径要点 |
|------|----------------------|
| Medieval Castle | `fps-castle` |
| Ancient Egypt | `fps-egypt` |
| City Racing | `driving_game`（支持 fixed / dynamic prompt） |

权重、sink 图、游戏资产在 [Skywork/Matrix-Game-4.0](https://huggingface.co/Skywork/Matrix-Game-4.0)；共享 T5/VAE 等由 pinned 基座版本自动拉取。

## 适用场景

- 研究「世界模型 + 可玩引擎 + 神经渲染」拆分架构
- 在消费级 RTX 
上体验/评测实时交互生成游戏
- 用 Coding Agent 改规则/关卡后做原型，再接渲染模型
- 对照纯视频世界模型（如 InSpatio-World）看状态持有方式差异

## 边界与代价

- 推理与在线 Demo 对驱动、CUDA、显存与磁盘要求高；资源需单独下载并校验
- 官方强调 `--fps` 是采集目标，不等于端到端 action-to-photon 延迟
- 安全模型是单用户本机 Demo，不是公网云游戏服务
- 商用前核对代码 Apache-2.0 与模型/第三方资产各自许可

## 相关链接

| 链接 | 说明 |
|------|------|
| https://github.com/SkyworkAI/Matrix-Game | 主仓库（含 1.x～4.0 目录） |
| https://github.com/SkyworkAI/Matrix-Game/tree/main/Matrix-Game-4 | 4.0 代码与快速开始 |
| https://matrix-game-v4-0.github.io/page/ | 项目主页与演示 |
| https://huggingface.co/Skywork/Matrix-Game-4.0 | 模型权重与游戏资产 |
| https://github.com/SkyworkAI/Matrix-Game/blob/main/Matrix-Game-4/docs/Matrix_Game_4_0.pdf | 技术报告 PDF |
| https://www.toutiao.com/article/7696803392270549544/ | 中文介绍来源 |

## Citation

```bibtex
@misc{zhang2026matrixgame4,
  title        = {Matrix-Game 4.0: Engine-Grounded Neural Rendering for Real-Time Interactive Worlds},
  author       = {Zhang, Jihai and Qian, Runjia and Kong, Kaiqin and Jiang, Biao and
                  Wang, Puyi and Chen, Siru and Li, Tianle and Chen, Guanjie and Wu, Junyi and
                  Chen, Chubin and Li, Yuanhang and Huang, Kaichen and Zhang, Haobo and
                  Yu, Wei and Chen, Guibin and Cheng, Yu and Zhou, Yahui},
  howpublished = {Technical report},
  year         = {2026},
  url          = {https://matrix-game-v4-0.github.io/page/}
}
```

## My takeaways

1. **定位**：不是「纯生成视频假装成游戏」，而是引擎持状态 + 模型做实时神经渲染的可玩 Harness。
2. **与 InSpatio-World**：后者偏参考视频→可探索 4D；Matrix-Game 4.0 偏可执行游戏 + 流式渲染，更接近「能玩的世界模型」。
3. **与 GameFactory-3A**：GameFactory 走 Agent Skills 产资产/引擎代码；Matrix-Game 提供可运行的引擎+渲染闭环，二者可互补。
4. **落地关键词**：消费级 GPU、RTX 5090/5080/5070 Ti、流式神经渲染、Coding Agent、同源开源。

## Related

- [[50 来源资料/代码仓库/AI/多模态与模型/InSpatio-World - 实时4D世界模拟器|InSpatio-World - 实时 4D 世界模拟器]]
- [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架]]
- [[40 知识导航/游戏开发|游戏开发]]
