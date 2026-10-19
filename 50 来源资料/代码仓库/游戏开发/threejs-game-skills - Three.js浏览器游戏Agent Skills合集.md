---
id: source-20261019-threejs-game-skills
title: threejs-game-skills - Three.js浏览器游戏Agent Skills合集
type: source
status: active
created: 2026-10-19
updated: 2026-10-19
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/AI
  - 主题/Agent
  - 主题/Three.js
  - 主题/开源资源
  - 主题/资产生成
author: majidmanzarpour
source_type: repo
source_url: https://github.com/majidmanzarpour/threejs-game-skills
source_author: Majid Manzarpour
source_date: 2026-06-14
summary: 面向 Codex / Claude Code 的九技能合集：用 threejs-game-director 总导演调度玩法、AAA 画面、UI、调试、QA 与可选 3D/图像/音频生成，把想法做成可玩的 Three.js 浏览器游戏。
related:
  - "[[40 知识导航/游戏开发|游戏开发]]"
  - "[[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]"
  - "[[50 来源资料/代码仓库/游戏开发/sprite-gen - 2D游戏精灵图与动画图集生成Skill|sprite-gen]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[30 知识资源/游戏开发/AI驱动的3D游戏制作流水线|AI驱动的3D游戏制作流水线]]"
discovery_source: https://www.toutiao.com/article/7697053592645894690/
---

# threejs-game-skills - Three.js浏览器游戏Agent Skills合集

## Source summary

**threejs-game-skills**（[majidmanzarpour/threejs-game-skills](https://github.com/majidmanzarpour/threejs-game-skills)）是一套面向 **Codex / Claude Code** 的开源 **Agent Skills 合集**：安装后让 Agent 使用 `threejs-game-director` 总导演，自动调度玩法、AAA 风格画面、UI、资产生成、音效、调试与发版验收，把想法做成**可玩、可验的 Three.js 浏览器游戏**。

核心一句话：**一套九技能 Skill 包 = AI「游戏开发小队」，总导演拆任务、分派、验收。**

- 约 **2465 stars / 242 forks**（公开检索口径，GitHub API 当时限流未复核）
- 许可证：**MIT**；作者：[Majid Manzarpour](https://github.com/majidmanzarpour)
- 默认分支：`main`
- 技术栈：Vite + TypeScript + Three.js（脚手架打进相关 Skill 目录）
- 发现来源：头条文 [这个skill合集，轻松做出爆款3D游戏](https://www.toutiao.com/article/7697053592645894690/)（作者：玩物天地）

解决的痛点：让 Agent「随便写个 Three.js 小游戏」往往画面粗糙、系统散乱、难自测。本合集把导演路由、专家技能、脚手架、确定性测试钩子与 Playwright 模板打包在一起，让 Agent 能端到端交付并自验。

## 核心特性

| 特性 | 说明 |
|------|------|
| **总导演路由** | `threejs-game-director` 统一入口：定范围与品质条、专家路由、有界委托、连续性笔记、资产恢复与本轮证据 |
| **九技能一包** | 玩法 / AAA 画面 / UI / 调试性能 / QA 发版 / 3D / 图像 / 音频 + 导演，Codex 与 Claude Code 共用同一套 `SKILL.md` |
| **按变更缩放验收** | 整局游戏走完整生产过检；小修 HUD 则保持小范围，避免过度流程 |
| **内置脚手架** | 空目录起步时使用打包的 Vite/TS/Three 模板；含 `__THREE_GAME_TEST_HOOKS__`、种子 RNG |
| **可自验交付** | Playwright 冒烟、视觉回归基线、bot 试玩；画布像素与渲染预算检查脚本 |
| **核心不强制付费 Key** | 无 Tripo/Gemini/ElevenLabs Key 时降级为程序化/本地资产并报告限制；有 Key 再启用外部资产生成 |

## 九技能地图

| Skill | 职责 |
|-------|------|
| `threejs-game-director` | 完整构建入口：范围、品质、路由、委托、连续性、证据 |
| `threejs-gameplay-systems` | 可玩循环、架构、关卡/机制、实体、输入、相机、物理选型、手感（hitstop/震屏等） |
| `threejs-aaa-graphics-builder` | 视觉记分卡、技美预算、着色器/材质菜谱、模型/VFX/渲染打磨 |
| `threejs-game-ui-designer` | HUD、菜单、覆盖层、响应式、图标、安全区、UI 状态 |
| `threejs-debug-profiler` | 场景/运行时/渲染 Bug、移动端问题、性能与 renderer 指标 |
| `threejs-qa-release` | 浏览器 QA、截图、画布像素、视觉 harness、bot 试玩、生产构建与风险报告 |
| `threejs-3d-generator` | Tripo：文/图生 3D、贴图、绑骨、动画、转换下载与 Three.js 导入 |
| `threejs-image-generator` | Gemini：概念图、贴图/贴花/天空、图标与 GUI、图生 3D 输入 |
| `threejs-audio-generator` | ElevenLabs：SFX/音乐/氛围/UI 音、TTS、清理与 Three.js 音频集成 |

## 官方 Demo（可玩）

| 游戏 | 在线试玩 |
|------|----------|
| Neon Ridge Drift（赛车） | https://ridgedrift.netlify.app |
| Championship Snooker Arena | https://snookerarena.netlify.app |
| Starship Dogfight | https://starshipdogfight.netlify.app |
| Tide Singer | https://tidesinger.netlify.app |
| Ripcore | https://ripcore.netlify.app |

## 适用边界

### 更适合

- 想用 Codex / Claude Code **从零或迭代**做出可玩的 Three.js 网页游戏
- 需要「导演 + 专家」分工，而不是单条巨型提示词硬扛全流程
- 重视可测交付：冒烟、视觉基线、bot 试玩、移动端与性能检查
- 可选接入 Tripo / Gemini / ElevenLabs 做高端资产，但核心玩法不绑死付费 API

### 需要注意

- 定位是 **浏览器 Three.js**，不是 Unity/UE 原生管线（与 GameFactory-3A 多引擎框架互补而非替代）
- 外部 3D/图/音频生成需自备 API Key，且勿写入前端游戏代码
- 演示品质依赖宿主 Agent 能力、模型与委托是否可用；安装 Skills 本身不改模型设置
- Stars 数据以 GitHub 页面为准，沉淀时可再复核

## 快速开始

**Codex（全局安装全部技能）**

```bash
npx skills add majidmanzarpour/threejs-game-skills --skill '*' -a codex -g -y
```

**Claude Code**

```bash
npx skills add majidmanzarpour/threejs-game-skills --skill '*' -a claude-code -g -y
```

**本地 clone 安装**

```bash
./install.sh --codex
./install.sh --claude
./install.sh --all
# 覆盖同名技能：./install.sh --codex --force
```

**典型提示词**

```text
Use threejs-game-director to build a premium futuristic tower defense game from scratch.
Automatically use the relevant gameplay, graphics, UI, asset generation, audio, debug,
and QA skills. Build a playable loop first, then iterate until it passes browser,
mobile, visual, UI, performance, and release checks.
```

- Claude Code：可用 `/threejs-game-director` 或在对话中点名
- Codex：可用 `$threejs-game-director` 或自然语言点名

**可选环境变量**

| 变量 | Skill | 用途 |
|------|-------|------|
| `TRIPO_API_KEY` | `threejs-3d-generator` | 文/图生 3D、贴图、绑骨、动画 |
| `GEMINI_API_KEY` | `threejs-image-generator` | 概念图、贴图、GUI 图 |
| `ELEVENLABS_API_KEY` | `threejs-audio-generator` | SFX/音乐/语音 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/majidmanzarpour/threejs-game-skills |
| **Skills 目录** | https://github.com/majidmanzarpour/threejs-game-skills/tree/main/skills |
| **作者 X** | https://x.com/majidmanzarpour |
| **发现文章（头条）** | https://www.toutiao.com/article/7697053592645894690/ |
| **许可证** | MIT |

## My takeaways

1. **定位**：Three.js 浏览器游戏的 Agent Skills「制作班组」，入口永远是 `threejs-game-director`。
2. **与 GameFactory-3A**：后者偏多引擎资产 Pipeline + 引擎 Adapter；本库偏 **浏览器 Three.js 可玩交付 + 内置 QA**。可并列收藏。
3. **与 sprite-gen**：sprite-gen 专攻 2D 精灵图集；本库专攻 3D 网页游戏整包。
4. **与头条「小王子」流水线文**：那篇是人工编排 Fable/Image/Tripo/Blender/Unity；本库把同类分工固化进 Skills，并默认落在 Three.js。
5. **检索关键词**：threejs-game-skills、game-director、Codex Skill、Claude Code、Three.js 网页游戏、AAA graphics skill。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架]]
- [[50 来源资料/代码仓库/游戏开发/sprite-gen - 2D游戏精灵图与动画图集生成Skill|sprite-gen - 2D游戏精灵图与动画图集生成Skill]]
- [[30 知识资源/游戏开发/AI驱动的3D游戏制作流水线|AI驱动的3D游戏制作流水线]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]