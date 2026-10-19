---
id: source-20261019-unity-agent-plugin
title: unity-agent-plugin - Unity官方Agent技能插件
type: source
status: active
created: 2026-10-19
updated: 2026-10-19
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/Unity
  - 主题/AI
  - 主题/Agent
  - 主题/Skills
  - 主题/开源资源
author: Unity Technologies
source_type: repo
source_url: https://github.com/Unity-Technologies/unity-agent-plugin
source_author: Unity Technologies
source_date: 2026-08-06
summary: Unity 官方面向 Claude Code / Codex / Grok 的 Agent 插件。约 29+ 原生 Skill，配合 Unity CLI 与编辑器控制能力，让 Agent 按引擎团队规范创建项目、改场景、写脚本、跑构建与回修；要求 Unity 6.0+。
related:
  - '[[40 知识导航/游戏开发|游戏开发]]'
  - '[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]'
  - '[[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills]]'
  - '[[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]'
  - '[[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]'
discovery_source: https://mp.weixin.qq.com/s/sg37D9UvgF80T_Hq6a-wRQ
---

# unity-agent-plugin - Unity官方Agent技能插件

## Source summary

**unity-agent-plugin**（[Unity-Technologies/unity-agent-plugin](https://github.com/Unity-Technologies/unity-agent-plugin)）是 Unity 官方为第三方 Coding Agent 提供的游戏开发插件：把引擎团队维护的 Skills、Unity CLI、以及对运行中编辑器的控制能力打包给 Agent，让其不再只靠零散第三方资料「猜 Unity」，而是按第一方规范施工。

核心一句话：**官方 Skill 包 + CLI + 编辑器控制 = Agent 进 Unity 上班。**

- 约 **433 stars / 46 forks**（截至 2026-10-19 GitHub API）
- 许可证：**Unity Companion License**（Unity 附属许可，非 MIT/Apache）
- 组织：[Unity Technologies](https://github.com/Unity-Technologies)
- 默认分支：`main`；仓库创建约 2026-08-06
- 插件包名：`unity@unity-agent-plugin`
- 支持 Agent：**Claude Code**、**Codex**、**Grok**（官方文档称将扩展到更多 Coding Agent）
- 引擎要求：**Unity 6.0+**
- 发现来源：微信文 [Unity官方 29 个Skill，让Agent 接管游戏引擎…](https://mp.weixin.qq.com/s/sg37D9UvgF80T_Hq6a-wRQ)（VibeCoding 系列）

注意：这不是 Asset Store / UPM 包，不能通过 Package Manager 安装；安装入口是各 Agent 的 plugin marketplace。

## 核心特性

| 特性 | 说明 |
|------|------|
| **第一方维护** | Skills、CLI 与编辑器控制规范由 Unity 团队提供，区别于依赖截图/模拟点击的社区 Unity MCP |
| **一条命令安装** | marketplace add + plugin install，无需为每个项目重复配置 |
| **三件套闭环** | Skills 教规范 → Unity CLI 跑命令/构建/许可证 → 与打开中的 Editor 交互施工 |
| **任务可显式调用** | 自然语言触发，或在 Claude Code / Codex / Grok 的 slash 菜单里直接点名 Skill |
| **UI 先分流** | `/ui` 先识别 UI Toolkit / uGUI / IMGUI，再进入对应工作流，避免「技术正确、工程错误」的框架替换 |
| **覆盖完整链路** | 工程基座、UI/文本、2D、渲染、玩法系统、变现与 Live Ops 等 |

## Skill 能力地图（仓库 `skills/`）

以下为仓库当前技能目录名（公开检索口径，列表会随版本变化）：

### 工程基座

| Skill | 用途 |
|-------|------|
| `new-unity-project` | 引导创建 Unity 项目 |
| `unity-cli` | 终端与实时编辑器操作入口 |
| `unity-package-management` | UPM 包管理 |
| `generate-editor-search-query` | 生成编辑器搜索查询 |
| `asset-transformer-toolkit` | 资产转换工具相关 |

### UI 与文本

| Skill | 用途 |
|-------|------|
| `ui` | UI 总入口：先识别框架再分流 |
| `ui-uitk` / `ui-ugui` / `ui-imgui` | UI Toolkit / uGUI / IMGUI 专用工作流 |
| `optimize-text-mesh-pro` | TextMeshPro（含中文方框等字体问题） |
| `localization` | 本地化 |

### 2D 与精灵

| Skill | 用途 |
|-------|------|
| `2d-pixel-perfect` | 像素完美与抖动模糊类问题 |
| `sprite-editor` / `manage-sprite-atlas` | Sprite 编辑、图集 |
| `sprite-segment-3x3grid` | 九宫格切片等 |
| `create-tile-palette-or-rule-tiles` | Tilemap 调色板 / Rule Tiles |
| `create-2d-physics` | 2D 物理 |

### 图形与渲染

| Skill | 用途 |
|-------|------|
| `shader-graph-create-custom-node` | Shader Graph 自定义节点 |
| `urp-postprocessing` | URP 后处理 |
| `validate-urp-render-graph-renderer-feature` | URP Render Graph / Renderer Feature 校验 |
| `migrate-birp-to-urp` | Built-in → URP 迁移 |

### 玩法与系统

| Skill | 用途 |
|-------|------|
| `physics-3d-collision` | 3D 物理碰撞 |
| `initialize-ai-navigation` | AI 寻路 / NavMesh |
| `audio-setup-mixers` / `optimize-audio` | 音频混音与优化 |
| `setup-multiplayer-services` | 多人联机服务 |
| `setup-vivox-voice-chat` | Vivox 语音聊天 |

### 变现、运营与发布

| Skill | 用途 |
|-------|------|
| `implement-in-app-purchases` | IAP 内购 |
| `levelplay-unity-integration` | LevelPlay 广告聚合 |
| `build-live-game` / `build-gtk` | Live Ops / 相关构建工作流 |
| `optimize-web` | Web 性能优化 |

Agent 在装好后可做的典型动作（配合 CLI / 打开中的 Editor）：创建与修改 GameObject、编辑 Scene/Asset、读 Hierarchy、在 Editor 中执行 C#、取 Editor/Build Log、安装编辑器与处理 License、跑 Build/测试并据报错回修。

## 适用边界

### 更适合

- 已用或准备用 **Unity 6.0+**，希望 Claude Code / Codex / Grok 直接参与 Unity 工程
- 需要第一方规范，而不是社区「截图点鼠标」式 MCP
- 高频标准任务：建项目、搭 UI、配物理/寻路、IAP/广告、URP/Render Graph 校验等
- 小团队「人指挥、Agent 施工」的原型与迭代

### 需要注意

- **不是自动游戏工厂**：长链路重构、创意判断、大规模资产与最终体验评测仍靠人
- 改项目可能不可轻易撤销；大改前务必 Git 提交（官方 README 明确提醒）
- 流水线/非交互场景要把输入、输出、验收与禁止事项写清，减少 Agent 反问阻塞
- 关闭当前项目不需要的 Skill，降低上下文干扰
- 许可为 **Unity Companion License**，商用/分发前需自行核对条款
- 微信文写「29 个 Skill」是整理口径；以仓库 `skills/` 与官方文档为准

## 快速开始

### Claude Code（会话内 slash）

```text
/plugin marketplace add Unity-Technologies/unity-agent-plugin
/plugin install unity@unity-agent-plugin
```

或终端：

```bash
claude plugin marketplace add Unity-Technologies/unity-agent-plugin
claude plugin install unity@unity-agent-plugin
```

验证：输入 `/unity:` 应能看到技能列表；`/plugin` 中 `unity` 为 installed/enabled。

### Codex

```bash
codex plugin marketplace add Unity-Technologies/unity-agent-plugin
codex plugin add unity@unity-agent-plugin
```

验证：`codex plugin list` 可见 `unity@unity-agent-plugin`。

### Grok

在 Grok Build 打开 `/marketplace`，搜索 Unity 后安装；或：

```bash
grok plugin install Unity-Technologies/unity-agent-plugin --trust
```

### 手动安装（无 marketplace 时）

```bash
git clone https://github.com/Unity-Technologies/unity-agent-plugin.git
cd unity-agent-plugin
# Claude Code: 将 skills/* 软链到 ~/.claude/skills/
# Codex: 将 skills/* 软链到 ~/.agents/skills/
```

手动链接的技能通常**没有** `unity:` 前缀（因未作为 plugin 安装）。

### 典型自然语言任务

```text
Add in-app purchases so players can buy a coin pack
I want to build a settings screen
My pixel art looks blurry and jitters when the camera moves
Show rewarded video ads so players can earn coins
Create a hexagonal tile palette for my level
Chinese characters show up as empty boxes in my TextMeshPro labels
Review my ScriptableRendererFeature for Render Graph problems
```

### 官方建议的稳妥用法

1. UI 相关请求**主动写明**框架（如「使用 UI Toolkit」或「使用 uGUI」）
2. 结果不对时，先让 Agent 读该 Skill 的 `references/` 再改
3. 流水线场景把验收标准写进提示词，避免反复追问
4. 不自动触发时，在提示词里**显式点名** Skill

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/Unity-Technologies/unity-agent-plugin |
| **Skills 目录** | https://github.com/Unity-Technologies/unity-agent-plugin/tree/main/skills |
| **官方说明（About plugin）** | https://docs.unity.com/en-us/ai/unity-plugin/about-unity-plugin |
| **Unity CLI 文档** | https://docs.unity.com/en-us/unity-cli |
| **反馈论坛** | https://discussions.unity.com/ |
| **发现文章（微信）** | https://mp.weixin.qq.com/s/sg37D9UvgF80T_Hq6a-wRQ |
| **许可证** | Unity Companion License（见仓库 `LICENSE.md`） |

## My takeaways

1. **定位**：Unity 官方把「正确做法」编成 Skill 包交给通用 Coding Agent，不是又一个社区 MCP Demo。
2. **与 threejs-game-skills**：后者是浏览器 Three.js 的 Agent 班组；本库是 **Unity 编辑器内**第一方技能。目标平台不同，可并列收藏。
3. **与 GameFactory-3A**：GameFactory 偏多引擎资产生成 Pipeline；本插件偏 **Unity 工程现场施工与引擎规范**。可互补。
4. **行业信号**：引擎开始成为 Agent 的执行环境；比拼重心从「谁敲 C# 更快」转向需求、规范、审美与 review 兜底。
5. **检索关键词**：unity-agent-plugin、Unity Skill、Claude Code、Codex、Grok、Unity CLI、Unity 6 Agent、官方 MCP/插件。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills - Three.js浏览器游戏Agent Skills合集]]
- [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架]]
- [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
