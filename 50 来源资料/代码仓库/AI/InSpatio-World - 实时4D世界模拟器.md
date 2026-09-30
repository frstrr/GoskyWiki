---
id: source-20260831-inspatio-world
title: InSpatio-World - 实时 4D 世界模拟器
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/4D世界模型
  - 主题/视频生成
  - 主题/新视角合成
  - 主题/自动驾驶仿真
  - 主题/具身智能
author: InSpatio Team
source_type: repo
source_url: https://github.com/inspatio/inspatio-world
source_author: InSpatio Team
source_date: 2026-08-31
summary: 基于时空自回归建模的实时 4D 世界模拟器，将单段参考视频转换为可自由探索、导航、重访的动态 4D 世界；支持精确相机轨迹控制、时间冻结等效果，H 系 GPU 可达 24 FPS。
related: []
---

# InSpatio-World - 实时 4D 世界模拟器

## Source summary

**InSpatio-World** 是 InSpatio 团队开源的 **首个以参考视频为条件的 4D 世界模型**，可将单段视频转换为可自由探索、导航与重访的动态 4D 世界。核心采用 **时空自回归（STAR, Spatiotemporal Autoregressive）** 架构与 **状态锚定世界建模（State-Anchored World Modeling）**，解决传统生成模型缺乏空间持久性、物理一致性和长序列漂移的问题。

- GitHub：`inspatio/inspatio-world`，约 **986+ Stars**
- 协议：**Apache-2.0**
- 论文：[arXiv:2604.07209](https://arxiv.org/abs/2604.07209)（2026）
- 模型规模：**1.3B** 参数
- WorldScore-Dynamic 排行榜：**实时/交互式方法第一名**

## 核心能力

| 能力 | 说明 |
|------|------|
| **参考视频条件 4D 世界** | 从单段 `.mp4` 参考视频重建持久世界状态，支持任意视角与时间采样 |
| **自由空间漫游** | 通过 pitch/yaw/displacement 轨迹文件精确控制相机运动，生成新视角视频 |
| **时间控制** | 支持时间冻结（freeze）、慢放、倒放等时序效果 |
| **物理与空间一致性** | 状态锚定 + 隐式时空缓存 + 显式空间约束，长序列探索不漂移 |
| **实时推理** | H 系 NVIDIA GPU 可达 **24 FPS**（1.3B + TAE + compile）；单卡 RTX 4090 约 **10 FPS** |
| **三阶段流水线** | Florence-2 字幕 → DA3 深度估计 → InSpatio-World v2v 推理 |
| **丰富轨迹预设** | 环绕轨道、推拉变焦（dolly zoom）、仅旋转（三脚架 pan/tilt）等 |
| **自动驾驶场景** | `--relative_to_source --rotation_only` 适配车载视角仿真 |

## 技术架构

### 三大核心组件

1. **World State Anchoring（世界状态锚定）** — 以参考视频构建持久局部世界状态，保证空间持久与物理恒定
2. **Spatiotemporal Autoregression（时空自回归）** — 基于世界状态做精确时空采样，支持自由导航
3. **Joint Distribution Matching Distillation（JDMD）** — 平衡真实世界保真度与合成可控性，稳定泛化

### 推理流水线

```
Step 1: Florence-2-large  →  视频字幕生成
Step 2: DA3 (Depth-Anything-3)  →  深度估计 + 点云渲染
Step 3: InSpatio-World-1.3B  →  video-to-video 新视角合成
```

### 依赖模型

| 模型 | 用途 | 来源 |
|------|------|------|
| **InSpatio-World-1.3B** | v2v 推理主模型 | [HuggingFace](https://huggingface.co/inspatio/world) |
| **Wan2.1-T2V-1.3B** | 文本编码器 + VAE + 基座 | [HuggingFace](https://huggingface.co/Wan-AI/Wan2.1-T2V-1.3B) |
| **DA3** | 深度估计 | [HuggingFace](https://huggingface.co/depth-anything/DA3NESTED-GIANT-LARGE) |
| **Florence-2-large** | 视频字幕 | [HuggingFace](https://huggingface.co/microsoft/Florence-2-large) |
| **TAEHV**（可选） | 推理加速 Tiny Auto Encoder | [GitHub](https://github.com/madebyollin/taehv) |

## 环境与依赖

| 要求 | 说明 |
|------|------|
| Python | 3.10 |
| CUDA | 12.1 |
| FlashAttention-2 | 推荐，通过 pip wheel 安装 |
| FlashAttention-3 | 可选，Hopper GPU（H100/H800）+ nvcc ≥ 12.3 |
| 操作系统 | Linux（推理脚本为 bash） |

```bash
conda env create -f environment.yml
conda activate inspatio_world
bash scripts/download.sh   # 下载全部 checkpoint
```

## 快速开始

```bash
# 1. 放入参考视频
mkdir -p my_videos && cp your_video.mp4 my_videos/

# 2. 运行完整流水线
bash run_test_pipeline.sh \
  --input_dir ./my_videos \
  --traj_txt_path ./traj/x_y_circle_cycle.txt

# 3. 结果输出到 ./output/my_videos/x_y_circle_cycle/
```

### 轨迹文件格式

3 行纯文本，空格分隔关键帧值，自动插值到输出帧数：

```
<pitch 度>   正值=向上环绕，负值=向下
<yaw 度>     正值=向左，负值=向右
<displacement>  相对位移尺度（pitch/yaw 非零时控制轨道半径；均为零时为推拉变焦）
```

### 常用参数

| 参数 | 说明 |
|------|------|
| `--traj_txt_path` | 相机轨迹文件（必填） |
| `--freeze_repeat N` | 时间冻结 N 帧 |
| `--freeze_frame idx` | 指定冻结帧（默认中间帧） |
| `--relative_to_source` | 轨迹相对初始视角 |
| `--rotation_only` | 仅旋转不位移（三脚架模式） |
| `--use_tae` | 启用 TAE 加速 |
| `--compile_dit` | torch.compile 加速（首次预热较慢） |
| `--skip_step1/2/3` | 跳过已完成步骤 |

## 适用场景

- **具身智能训练**：在动态一致虚拟世界中训练 agent 物理直觉与决策
- **自动驾驶仿真**：从车载视频生成可控新视角，模拟场景演化
- **4D 相册 / 沉浸式媒体**：静态视频转可探索 4D 体验
- **实时交互世界观察**：WorldScore-Dynamic 基准下的 SOTA 实时方法
- **游戏/引擎参考**：单视频重建可导航 3D+时间环境的 AI 管线思路（非直接 Unity 插件）

## 生态与致谢

| 项目 | 关系 |
|------|------|
| [Wan2.1](https://github.com/Wan-Video/Wan2.1) | 骨干网络基座 |
| [Self-Forcing](https://github.com/guandeh17/Self-Forcing) | 训练代码参考 |
| [Depth-Anything-3](https://github.com/ByteDance-Seed/depth-anything-3) | 深度估计 |
| [Florence-2](https://github.com/anyantudre/Florence-2-Vision-Language-Model) | 视频字幕 |
| [ReCamMaster](https://github.com/KlingAIResearch/ReCamMaster) | 相机控制灵感 |
| [TAEHV](https://github.com/madebyollin/taehv) | 推理加速 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/inspatio/inspatio-world |
| **项目主页** | https://inspatio.github.io/inspatio-world/ |
| **在线 Demo** | https://world.inspatio.com/ |
| **HuggingFace 模型** | https://huggingface.co/inspatio/world |
| **论文** | https://arxiv.org/abs/2604.07209 |
| **Discord 社区** | https://discord.gg/SyyjR3Z57w |
| **HyperAI 容器 Demo** | https://app.hyper.ai/console/Open-Resources/containers/g5iE8HAbMKH |
| **WorldScore 排行榜** | https://paperswithcode.com/benchmark/worldscore |

## My takeaways

1. **4D 世界模型新范式**：不是逐帧生成像素，而是维护锚定于参考视频的持久世界状态，再从状态中采样任意时空观测——与 unicore 等游戏框架追求的「持久世界状态」理念高度相关。
2. **工程化完整度高**：三阶段流水线、轨迹文件、skip 步骤、TAE/compile 加速、自动驾驶专用参数均已封装，可直接 `bash run_test_pipeline.sh` 跑通。
3. **硬件门槛明确**：需 CUDA GPU + 多模型 checkpoint（约数 GB），Linux 环境；非轻量 demo，适合有 GPU 集群的研究/仿真团队。
4. **实时性能有竞争力**：1.3B 模型在 H 系 GPU 达 24 FPS，4090 约 10 FPS，是目前少数宣称「实时交互式 4D 世界」的开源方案。
5. **与 Unity/游戏引擎的关系**：提供的是 **AI 侧视频→4D 世界** 管线，非 Unity 插件；若 unicore 需「从视频重建可探索场景」或「AI 驱动世界模拟」，可作为后端推理参考，需自行对接渲染/引擎层。
6. **相机轨迹控制精细**：文本轨迹文件定义 pitch/yaw/displacement，支持环绕、推拉、冻结等电影级运镜，适合需要可控视角合成的仿真场景。

## Related

- （待补充：若后续建立 `30 知识资源/AI/4D世界模型` 或 `40 知识导航/世界模拟` 主题页，可在此链接）