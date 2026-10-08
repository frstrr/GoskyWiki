---
id: source-20261008-sprite-gen
title: sprite-gen - 2D游戏精灵图与动画图集生成Skill
type: source
status: active
created: 2026-10-08
updated: 2026-10-08
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/AI
  - 主题/Agent
  - 主题/资产生成
  - 主题/像素画
  - 主题/精灵图
  - 主题/开源资源
author: aldegad
source_type: repo
source_url: https://github.com/aldegad/sprite-gen
source_author: aldegad
source_date: 2026-10-08
summary: 面向 Codex/Claude 的 Python CLI 与 Agent Skill：输入一张基准静帧，经组件行管线生成干净的 2D 游戏精灵图集与透明运动循环，输出真实 alpha、runtime atlas 与机器可读 frame_layout。
related:
  - [[40

## Source summary

**sprite-gen**（[aldegad/sprite-gen](https://github.com/aldegad/sprite-gen)）是一套面向 **Codex / Claude** 的开源 **2D 游戏精灵图生成 Skill + Python CLI**：给它**一张基准静帧**，按状态逐行驱动生成、锁定角色身份、把色键背景解混为真实 alpha、提取干净透明帧，并烘焙带机器可读 `manifest.json.frame_layout` 的运行时图集；也可走视频管线，为每个运动状态得到无缝透明循环。

核心一句话：**一张静帧进，游戏可用的透明精灵图集 / 运动循环出。**

- 约 **2599 stars / 275 forks**（截至 2026-10-08 GitHub API）
- 语言：**Python**（CPython 3.11+；CI 跑 3.14）；许可证：**Apache-2.0**
- 默认分支：`main`
- 作者：[aldegad](https://github.com/aldegad)
- 文档：多语言 README（含 [简体中文](https://github.com/aldegad/sprite-gen/blob/main/README.zh-Hans.md)）
- Topics：`2d-game` / `sprite-generation` / `sprite-sheet` / `pixel-art` / `codex` / `claude-code` / `ai-tools` / `gamedev`

解决的痛点：直接向图像模型要「sprite sheet」时常出现脸每帧漂移、背景抠不干净、姿势叠在一起偏离网格、引擎无法消费的 PNG。sprite-gen 用组件行管线 + 提取/策展把「可爱演示」收成「可用素材」。

## 核心特性

| 特性 | 说明 |
|------|------|
| **组件行图集管线** | `prepare → gen/gen-set → extract → compose-atlas`，可选 `curation` 策展后再烘焙 |
| **真实 alpha 提取** | 色键背景解混为透明，对照白底验证，减少色键边缘残留 |
| **运行时清单** | `manifest.json.frame_layout`：绝对帧矩形、每状态 fps 与循环标志，引擎按矩形取样无需猜网格 |
| **Breathe** | 把静止 idle 烘焙成确定性挤压/拉伸的呼吸循环 |
| **视频 → 透明循环** | `video-canvas → video → video-frames → video-loop`，可出 GIF/WebP/条带 |
| **像素网格锁定** | Backbone Lattice 统一主体网格，切割贴合像素格 |
| **确定性配色** | `recolor` 按调色板映射烘焙多色变体表 |
| **引擎导出** | `export-aseprite` 等，面向 Phaser / Flame / Aseprite 工作流 |
| **独立素材工具** | cutout、slice-sheet、background-tile、shadow、inspect-motion 等可单独用 |
| **可选场景合成** | `scene-render` / `scene-inspect`：摆放、相机、灯光、共享阴影 |

## 流水线地图

| 流水线 / 工具组 | 输入 → 输出 |
|-----------------|-------------|
| **A · 图集行** | 一张静帧 + 状态列表 → `sprite-sheet-alpha.png` + `manifest.json.frame_layout`（idle 可含 Breathe） |
| **B · 视频 → 循环** | 一张静帧 → 每状态无缝透明 GIF/WebP/条带（Grok Imagine 驱动，按真实周期裁切） |
| **C · 实用工具** | 导入图/网格表 → 透明切片；成品图集 → 可策展运行目录 |
| **D · 后期处理** | 成品表 → 配色变体、图层合成、引擎导出 |
| **E · 素材工具** | 现有 PNG/动画 → 平铺背景、投影阴影、运动/接地测量 |
| **S · 场景（可选）** | 现有素材 + `scene.json` → PNG 帧、MP4/GIF、检查与摆放元数据 |

## 适用边界

### 更适合

- 2D / 像素风游戏需要从单张立绘快速做出 idle/walk/run/jump/attack 等状态精灵
- 已有 Codex / Claude 订阅，想用 Agent Skill 驱动整条精灵管线
- 需要引擎可消费的 atlas + `frame_layout`（而非模型直接吐出的「伪精灵表」）
- 想用视频模型做运动循环，再用工具切透明帧与环点
- 后期要做多色变体、Aseprite/Phaser/Flame 导出或简单场景合成

### 需要注意

- **图像/视频生成依赖外部提供方与凭据**：图像侧可用 `codex`/`grok`（订阅）或显式 `openai`（按调用计费）；视频侧需自备 `grok` 登录或 `XAI_API_KEY`，仓库不附带凭据
- 走/跑等循环位移默认偏实验性，需运动 QA 真正通过后再当成品用
- 侧面朝向（facing）提示词不能保证结果；方向检测默认只记录不纠正，镜像/重生纠正需显式开启
- 视频管线另需 `ffmpeg`、`img2webp`；走跑跳帧修复/周期对齐建议安装 RIFE（`sprite-gen rife install`）
- 与「全自动 3A 多模态资产生成」（如 GameFactory-3A）不同：本库专注 **2D 精灵图 / atlas / 透明循环**

## 快速开始

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -e .
sprite-gen --help
```

**A · 图集行**

```bash
sprite-gen prepare --out-dir <run> --character-id <id> --base-image base.png
sprite-gen gen-set --run-dir <run> --provider codex
sprite-gen extract --run-dir <run>
sprite-gen compose-atlas --run-dir <run>
sprite-gen curation --run-dir <run>   # 可选
```

**B · 视频 → 循环**

```bash
sprite-gen video-set --base side=still.png --states idle,walk,run,jump,attack --out-dir set/
```

**作为 Codex Skill 安装**

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo aldegad/sprite-gen --path . --name sprite-gen
```

面向 Agent 的工作流、门禁与契约见仓库根目录 `SKILL.md`。

## 你实际得到什么

- `sprite-sheet-alpha.png`：真实 alpha 的透明精灵图集
- `manifest.json.frame_layout`：绝对帧矩形 + 每状态 fps / 循环标志
- 每状态 QA 用 GIF / contact sheet，便于在交付前按「运动」评判
- 可选：配色变体、阴影、平铺背景、场景渲染产物

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/aldegad/sprite-gen |
| **英文 README** | https://github.com/aldegad/sprite-gen/blob/main/README.md |
| **简体中文 README** | https://github.com/aldegad/sprite-gen/blob/main/README.zh-Hans.md |
| **Agent Skill 契约** | https://github.com/aldegad/sprite-gen/blob/main/SKILL.md |
| **文档索引** | https://github.com/aldegad/sprite-gen/blob/main/docs/README.md |
| **架构说明** | https://github.com/aldegad/sprite-gen/blob/main/docs/architecture.md |
| **视频管线** | https://github.com/aldegad/sprite-gen/blob/main/docs/video-pipeline.md |
| **用户工作流** | https://github.com/aldegad/sprite-gen/blob/main/docs/user-workflow.md |
| **Issues** | https://github.com/aldegad/sprite-gen/issues |
| **作者主页** | https://github.com/aldegad |
| **许可证** | Apache-2.0 |

## My takeaways

1. **定位**：把「模型生图/生视频」接到「引擎可吃的 2D 精灵资产」——重点在提取、对齐、atlas 与策展，而不只是再包一层提示词。
2. **与 GameFactory-3A 互补**：后者偏多模态 3A 资产 + 多引擎代码；sprite-gen 深耕 2D 精灵行管线与透明循环，更轻、更贴像素/图集场景。
3. **与 Mixamo 分工**：Mixamo 偏 3D 人形绑骨与动作库；sprite-gen 偏 2D 静帧 → 精灵表 / 循环。2D 原型期可优先看本库。
4. **落地门槛**：本机 Python venv +（可选）视频依赖与模型凭据；先跑 atlas 行管线比一上来上视频更稳。
5. **检索关键词**：sprite sheet、精灵图集、透明循环、像素画、Codex Skill、Claude Skill、frame_layout、chroma alpha、Grok Imagine。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架]]
- [[30 知识资源/游戏开发/Mixamo - 免费人形角色动画平台|Mixamo - 免费人形角色动画平台]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]