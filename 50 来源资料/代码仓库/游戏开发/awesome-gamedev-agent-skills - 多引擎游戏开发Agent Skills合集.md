---
id: source-20261109-awesome-gamedev-agent-skills
title: awesome-gamedev-agent-skills - 多引擎游戏开发Agent Skills合集
type: source
status: active
created: 2026-11-09
updated: 2026-11-09
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/AI
  - 主题/Agent
  - 主题/Skills
  - 主题/Godot
  - 主题/Unity
  - 主题/Unreal
  - 主题/开源资源
author: Abhishek Barali
source_type: repo
source_url: https://github.com/gamedev-skills/awesome-gamedev-agent-skills
source_author: Abhishek Barali / gamedev-skills
source_date: 2026-11-09
summary: 77 个游戏开发 Agent Skills + 路由器：按引擎与任务自动加载匹配技能。覆盖 Godot/Unity/Unreal/Ebitengine/Phaser/PixiJS/three.js/Bevy/pygame/LÖVE/Defold/Roblox，兼容 Claude Code、Cursor、Codex、Copilot、Gemini CLI 等。
related:
  - "[[40 知识导航/游戏开发|游戏开发]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[50 来源资料/代码仓库/游戏开发/unity-agent-plugin - Unity官方Agent技能插件|unity-agent-plugin]]"
  - "[[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills]]"
  - "[[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]"
  - "[[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]"
---

# awesome-gamedev-agent-skills - 多引擎游戏开发Agent Skills合集

## Source summary

**awesome-gamedev-agent-skills**（[gamedev-skills/awesome-gamedev-agent-skills](https://github.com/gamedev-skills/awesome-gamedev-agent-skills)）是一套面向 AI Coding Agent 的**多引擎游戏开发 Skills 合集**：提供 **77 个**可移植 `SKILL.md`（Anthropic 发起的 Agent Skills 开放格式）以及一个**路由器（router）**。你用自然语言描述要做的事；路由器识别引擎与任务后，只加载匹配技能再写代码。

核心一句话：**装一次 → 路由器按引擎/任务自动点名技能 → 跨引擎写游戏。**

- 许可证：**Apache-2.0**
- 组织 / 维护者：[gamedev-skills](https://github.com/gamedev-skills) / [Abhishek Barali](https://github.com/AbhishekBarali)
- 默认分支：`main`
- 格式：[Agent Skills](https://agentskills.io) 开放标准（`SKILL.md`）
- 文档站：[gamedev-skills.github.io/awesome-gamedev-agent-skills](https://gamedev-skills.github.io/awesome-gamedev-agent-skills/)
- 引擎基线见仓库 [version support](https://github.com/gamedev-skills/awesome-gamedev-agent-skills/blob/main/docs/VERSION-SUPPORT.md)（如 Godot 4.7、Unity 6.3 LTS、UE 5.8 等，以文档为准）

解决的痛点：单引擎 Skill 包（如 Unity 官方插件、Three.js 合集）很好用，但换引擎就要换整套；本库把多引擎、跨引擎学科、类型模板与发布工作流统一进一套路由器，避免手工挑选几十个技能名。

## 核心特性

| 特性 | 说明 |
|------|------|
| **跨引擎** | Godot、Unity、Unreal、Ebitengine、Phaser、PixiJS、three.js、Bevy、pygame、LÖVE、Defold、Roblox |
| **路由器自动选技能** | 检测工程指纹（如 `project.godot`、`*.uproject`）+ 解析任务语义，只加载相关技能 |
| **美术方向管线** | `create-game-assets` 锁定视觉目标、族系生成、确定性归一化，并在游戏内校验 |
| **可信任基线** | 技能基于官方文档编写、版本钉死，并由 `scripts/validate-skills.py` 校验 |
| **一键安装** | `npx skills add ...` 自动识别本机 Agent；也可作 Claude Code plugin marketplace |
| **原生多端** | 同一套 `SKILL.md` 可在 Claude Code、Cursor、Windsurf、Cline、Codex、Gemini CLI、GitHub Copilot、Kiro 等加载 |

## 路由器怎么选技能

1. **检测引擎**（互斥，一次只选一个）：读项目文件指纹；不确定就追问。
2. **解析任务**：概念（AI、着色器、存档）、类型（平台跳跃、Roguelike）、发布步骤（Game Jam、Steam）等。
3. **按序加载**：引擎基础 → 学科概念 → 类型胶水；切换主题会重新路由。

示例：「给 Godot 玩家加二段跳」→ `godot-2d-movement` + `platformer`，其余 75 个不加载。完整路由表见仓库 [`router/SKILL.md`](https://github.com/gamedev-skills/awesome-gamedev-agent-skills/blob/main/router/SKILL.md)。

## 能力目录（77 Skills / 9 类）

### 引擎

| 分组 | 数量 | 代表能力 |
|------|------|----------|
| **Godot** | 16 | GDScript/C#、节点场景、2D 移动、TileMap、物理、UI、动画、着色器、3D、音频、多人、导出、无头测试 |
| **Unity** | 8 | C# 脚本、新 Input System、物理、动画、ScriptableObject、2D Tilemap、NavMesh、构建管线 |
| **Unreal** | 6 | Blueprint、C++ Gameplay、Enhanced Input、行为树、Niagara、打包 |
| **Ebitengine** | 1 | Go 2D 可玩游戏：输入/渲染/音频/测试与平台构建 |
| **Web 引擎** | 6 | Phaser 核心与 Arcade 物理、PixiJS 渲染、three.js 场景/glTF/材质灯光 |
| **其他引擎** | 12 | Bevy ECS、pygame、LÖVE、Defold、Roblox（Luau/DataStore/UI/网络/角色/物理/Studio/模拟器经济） |

### 跨引擎学科（15）

资产生成、游戏 AI / 行为树与 Utility AI、程序生成、对话、存档、音频设计、着色器、物理手感、关卡设计、输入架构、game feel、UI/UX、相机、性能优化等。

### 类型模板（9）

`platformer`、`roguelike`、`rpg`、`fps-shooter`、`tower-defense`、`card-game`、`visual-novel`、`survival-crafting`、`puzzle` —— 组合引擎 + 学科，不重复教基础。

### 工作流（4）

`game-jam`、`prototype-fast`、`steam-publish`、`itch-publish`。

## 适用边界

### 更适合

- 用 Cursor / Claude Code / Codex 等做**多引擎**游戏原型与功能迭代
- 不想记技能名，希望「描述任务 → 自动加载」
- 需要跨引擎共通知识（存档、手感、程序生成）与类型模板
- 希望技能版本钉死、有校验器，减少「模型瞎编 API」

### 需要注意

- 定位是 **Agent Skills 知识包**，不是完整游戏工厂；资产生成深度弱于 GameFactory-3A 一类 Pipeline
- Unity 深度官方规范与编辑器控制仍看 [[50 来源资料/代码仓库/游戏开发/unity-agent-plugin - Unity官方Agent技能插件|unity-agent-plugin]]
- 浏览器 Three.js「导演 + QA 整包交付」仍看 [[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills]]
- 安装 CLI 需要 **Node.js 22.20+**；引擎基线会随仓库升级，以 VERSION-SUPPORT 为准
- Stars / 活跃度以 GitHub 页面为准（沉淀时 API 可能限流）

## 快速开始

**通用一键安装（推荐）**

```bash
npx skills add gamedev-skills/awesome-gamedev-agent-skills
```

可选：`--list` 预览、`-g` 全局、`-a cursor` / `-a claude-code` / `-a gemini-cli` 指定 Agent。

**Claude Code 插件方式**

```bash
claude plugin marketplace add gamedev-skills/awesome-gamedev-agent-skills
claude plugin install gamedev@awesome-gamedev-agent-skills
```

只装某一引擎时可：`router` + `godot` / `unity` / `unreal` / `ebitengine` / `web-engines` / `other-engines`。

**各 Agent 技能目录（手拷时）**

| Agent | 目录 |
|-------|------|
| Claude Code | `.claude/skills/` |
| Codex / Gemini CLI / Copilot 等 | `.agents/skills/` |
| Cursor | `.cursor/skills/`（也读 `.agents/skills/`） |
| Windsurf / Cline / Kiro / omp | 见仓库 `docs/COMPATIBILITY.md` |

装好后直接自然语言描述任务即可，不必手写技能名。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/gamedev-skills/awesome-gamedev-agent-skills |
| **文档站（技能目录）** | https://gamedev-skills.github.io/awesome-gamedev-agent-skills/ |
| **安装说明** | https://github.com/gamedev-skills/awesome-gamedev-agent-skills/blob/main/docs/INSTALLATION.md |
| **兼容性** | https://github.com/gamedev-skills/awesome-gamedev-agent-skills/blob/main/docs/COMPATIBILITY.md |
| **版本支持** | https://github.com/gamedev-skills/awesome-gamedev-agent-skills/blob/main/docs/VERSION-SUPPORT.md |
| **技能格式** | https://github.com/gamedev-skills/awesome-gamedev-agent-skills/blob/main/docs/SKILL-FORMAT.md |
| **路由器** | https://github.com/gamedev-skills/awesome-gamedev-agent-skills/tree/main/router |
| **npm skills CLI** | https://www.npmjs.com/package/skills |
| **Agent Skills 标准** | https://agentskills.io |
| **许可证** | Apache-2.0 |

## My takeaways

1. **定位**：多引擎「游戏开发技能路由器」——比单引擎官方/社区 Skill 包更广，比资产生成工厂更轻。
2. **与 unity-agent-plugin**：官方插件强在 Unity 编辑器现场施工；本库强在**跨引擎统一路由**与共通学科。
3. **与 threejs-game-skills**：后者是 Three.js 可玩交付班组 + QA；本库 three.js 技能是目录中的一块，外加 Godot/Unity/UE 等。
4. **与 GameFactory-3A**：GameFactory 偏资产 Pipeline；本库偏**引擎 API / 玩法 / 类型 / 发布**的 Skills 知识。
5. **检索关键词**：awesome-gamedev-agent-skills、gamedev-skills、游戏 Agent Skills、Godot Unity Unreal Skill 路由、Claude Code Cursor Codex 游戏开发。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[50 来源资料/代码仓库/游戏开发/unity-agent-plugin - Unity官方Agent技能插件|unity-agent-plugin - Unity官方Agent技能插件]]
- [[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills - Three.js浏览器游戏Agent Skills合集]]
- [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架]]
- [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
