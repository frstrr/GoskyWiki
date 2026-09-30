---
id: source-20260907-awesome-astra-prompts
title: awesome-astra-prompts - GPT-6 Astra 提示词精选
type: source
status: active
created: 2026-09-07
updated: 2026-09-07
tags:
  - 来源/代码仓库
  - 主题/提示词工程
  - 主题/AI
  - 主题/3D生成
  - 主题/游戏开发
  - 主题/Blender
  - 主题/开源资源
author: TripoGrowthLab
source_type: repo
source_url: https://github.com/TripoGrowthLab/awesome-astra-prompts
source_author: TripoGrowthLab
source_date: 2026-09-07
summary: Tripo 策展的 GPT-6 Astra 提示词与 3D 示例精选列表：游戏、Blender 场景与交互世界；约 153 例、14 语种、含预览与源码链接，每日两次自动同步。适合做 Astra/AI 生成 3D 与游戏原型时的灵感与提示词参考。
related:
  - "[[50 来源资料/代码仓库/AI/Vibe Coding CN - AI结对编程指南|Vibe Coding CN - AI结对编程指南]]"
  - "[[50 来源资料/代码仓库/AI/img2threejs - 图片转程序化 Three.js 模型|img2threejs - 图片转程序化 Three.js 模型]]"
---

# awesome-astra-prompts - GPT-6 Astra 提示词精选

## Source summary

**awesome-astra-prompts** 是一份由 [Tripo](https://www.tripo3d.ai/) / [TripoGrowthLab](https://github.com/TripoGrowthLab) 策展的 **GPT-6 Astra** 提示词与 3D 示例 Awesome 列表，定位为「下一个游戏、场景或交互世界的起点」。

- GitHub：`TripoGrowthLab/awesome-astra-prompts`（约 **29 Stars**，2026-09 新建）
- 官网模型页：https://www.tripo3d.ai/3d-prompts/models/gpt-6-astra
- 规模（README 宣称）：**153 个示例 · 14 种语言 · 6 个带源码的示例**
- 更新节奏：**每日两次**自动同步（UTC 00:23 / 12:23，即北京时间约 08:23 / 20:23）
- 协议：仓库工具与原创文档为 **MIT**；示例图片与第三方内容仍归原作者，见 `RIGHTS.md`
- 技术栈：JavaScript（渲染/同步脚本）+ CMS 驱动生成 Markdown 目录

核心价值：**不是可运行框架**，而是可检索的 Astra 提示词画廊——带作者署名、预览视频/图、原文链接，部分条目附 GitHub 源码或 Live Demo。

## 核心能力

| 能力 | 说明 |
|------|------|
| **场景覆盖广** | 游戏原型、Blender 场景、Three.js/浏览器交互世界、Unreal/Unity 示例、建筑可视化、产品演示等 |
| **可复用提示词** | 每条给出可复制 Prompt 文本，并链到 Tripo 详情页与原帖（多为 X/Twitter） |
| **多语言** | 同一条目维护 14 种语言的标题与 Prompt（在 Growth CMS 中完成后再发布） |
| **预览与溯源** | 预览图/视频、原帖、可选源码仓与 Demo，便于判断质量与复现路径 |
| **自动化策展管线** | CMS 发布 → GitHub Actions 同步 → 校验链接/校验和 → 一次提交更新目录与资源 |
| **欢迎贡献** | 通过 Issue 建议示例（原帖、预览、Prompt、仓库）；维护者在 CMS 中策展，勿直接改生成的 Markdown |

## 内容形态速览

| 类型 | 典型目标 | 工具/引擎线索（README 中高频） |
|------|----------|--------------------------------|
| 可玩游戏原型 | 一句话/少轮对话做出可玩切片 | Unity、Unreal、Three.js、浏览器 WebGL、Godot、Roblox |
| Blender 场景 | 建筑、机车、角色表情切换、可编辑世界 | Blender / Cycles |
| 浏览器交互 | 落地页、展览、模拟器、小游戏 | Three.js、WebGPU、C#/WASM |
| 参考图/资产驱动 | 照片→可编辑场景、资产组装与绑定 | Tripo P2 + Blender 等 |

**精选 Featured（README 首页）：**

1. Surviving society of autonomous Unreal humans（Matt Shumer）
2. Kaiju city battle（Majid Manzarpour）
3. Switchable character expressions in Blender（Nano）
4. One-shot Minecraft-style world（Flavio Adamo）

**带 GitHub 源码标记的示例（目录前列，便于深挖）：**

- Procedural living ocean and storm simulation
- Gogh Strike multiplayer FPS
- Cathedral hack-and-slash arena
- Anti-gravity combat racer
- Interactive dual-ring energy core
- Cluj-Napoca Union Square in voxels

完整列表与实时数量以仓库 README / [Astra 模型页](https://www.tripo3d.ai/3d-prompts/models/gpt-6-astra) 为准。

## 仓库工作方式（维护视角）

| 环节 | 说明 |
|------|------|
| 内容真相源 | Growth CMS（已发布记录才同步；草稿不进公开集合） |
| 生成本地产物 | 多语言目录、README、源码索引、预览图资源 |
| 本地检查 | `npm ci` → `npm run check`（Node.js ≥ 22.16） |
| 同步命令 | 配置只读 CMS Key 后：`npm run sync` / `npm run sync -- --check` |
| 勿手改 | 生成的 Markdown/图片是产物；改内容走 CMS，改展示走 `scripts/lib/render.mjs` 等 |

布局灵感来自 [YouMind Awesome GPT Image 2](https://github.com/YouMind-OpenLab/awesome-gpt-image-2)。

## 适用场景

- 用 **GPT-6 Astra** 做游戏/3D 世界时，快速找**可抄的提示词结构**与效果标杆
- 调研 AI 生成在 Blender / Three.js / Unity / Unreal 上的**现实产出边界**
- 需要「原帖 + Prompt + 预览 +（偶有）源码」的可溯源案例，而不是零散截图
- 与 unicore 等 Unity 框架结合：参考 Unity/可玩原型类条目的提示写法与范围控制

## 与同类资源对比

| 项目 | 侧重 | 关系 |
|------|------|------|
| **awesome-astra-prompts** | Astra 专用、3D/游戏/场景提示词画廊 | 本条目 |
| [[50 来源资料/代码仓库/AI/Vibe Coding CN - AI结对编程指南\\|Vibe Coding CN]] | 软件/游戏 AI 结对编程方法论与提示词体系 | 流程与工程方法互补；本仓更偏「一句话出 3D 世界」案例 |
| [[50 来源资料/代码仓库/AI/img2threejs - 图片转程序化 Three.js 模型\\|img2threejs]] | 图片→程序化 Three.js 代码 Skill | 偏编码代理与可维护 3D 代码；本仓偏 Astra 生成结果策展 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/TripoGrowthLab/awesome-astra-prompts |
| **Astra 模型 / 提示词站** | https://www.tripo3d.ai/3d-prompts/models/gpt-6-astra |
| **贡献说明** | https://github.com/TripoGrowthLab/awesome-astra-prompts/blob/main/CONTRIBUTING.md |
| **建议新示例（Issue）** | https://github.com/TripoGrowthLab/awesome-astra-prompts/issues/new |
| **同步工作流** | https://github.com/TripoGrowthLab/awesome-astra-prompts/actions/workflows/sync-prompts.yml |
| **权利与移除请求** | https://github.com/TripoGrowthLab/awesome-astra-prompts/blob/main/RIGHTS.md |
| **策展组织** | https://github.com/TripoGrowthLab |
| **版式参考：Awesome GPT Image 2** | https://github.com/YouMind-OpenLab/awesome-gpt-image-2 |

## My takeaways

1. **定位是灵感与 Prompt 索引，不是引擎依赖**：需要 Astra 生成 3D/游戏时优先翻这里；不要指望直接当 Unity 包引入。
2. **带源码的条目优先深挖**：当前公开标注源码的示例不多，适合对照「纯 Prompt 效果」与「可二次开发仓库」。
3. **更新靠 CMS + Actions**：Star 虽不高但同步频率高；以 README 与 Tripo 详情页为最新事实来源。
4. **对 unicore / 游戏原型有用**：Unity、可玩浏览器游戏、一句话开放世界类条目可直接借鉴 Prompt 粒度（目标、交互、镜头、约束写清）。
5. **版权注意**：MIT 只管策展工具与文档；复用示例素材前核对原作者授权与 `RIGHTS.md`。

## Related

- [[50 来源资料/代码仓库/AI/Vibe Coding CN - AI结对编程指南|Vibe Coding CN - AI结对编程指南]]
- [[50 来源资料/代码仓库/AI/img2threejs - 图片转程序化 Three.js 模型|img2threejs - 图片转程序化 Three.js 模型]]
- [[50 来源资料/代码仓库/游戏开发/AbilityKit - 模块化游戏战斗工具集|AbilityKit - 模块化游戏战斗工具集]]