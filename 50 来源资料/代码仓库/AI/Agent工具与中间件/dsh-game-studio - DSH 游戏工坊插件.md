---
id: source-20261106-dsh-game-studio
title: dsh-game-studio - DSH 游戏工坊插件
type: source
status: active
created: 2026-11-06
updated: 2026-11-06
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/游戏开发
  - 主题/DeepSeek-Harness
  - 主题/Three.js
  - 主题/开源资源
  - 主题/资产生成
author: ZSeven-W
source_type: repo
source_url: https://github.com/ZSeven-W/dsh-game-studio
source_author: ZSeven-W
source_date: 2026-11-05
summary: DeepSeek Harness 插件「DSH 游戏工坊」：一句话描述小游戏，侧栏即时可玩；支持 2D/3D 体素模板、OpenAI/Codex AI 换装（花钱前确认）、自动试玩、版本回退，并导出单文件离线 HTML。
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/dsh-blender - DSH Blender 3D 生产插件|dsh-blender]]"
  - "[[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills]]"
  - "[[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]"
  - "[[50 来源资料/代码仓库/游戏开发/sprite-gen - 2D游戏精灵图与动画图集生成Skill|sprite-gen]]"
  - "[[40 知识导航/游戏开发|游戏开发]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
aliases:
  - DSH 游戏工坊
  - dsh-game-studio
  - @zseven-w/dsh-game-studio
---

# dsh-game-studio - DSH 游戏工坊插件

## Source summary

**dsh-game-studio / DSH 游戏工坊**（[ZSeven-W/dsh-game-studio](https://github.com/ZSeven-W/dsh-game-studio)）是面向 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)（DSH）的 **小游戏制作插件**（npm：`@zseven-w/dsh-game-studio`）。用一句话描述玩法，Agent 从 2D 或 3D 体素模板生成可玩版本，在对话**右侧侧栏**实时预览；可换 AI 美术、自动试玩、版本回退，最终导出**单个离线 HTML**。

核心一句话：**一句话 → 侧栏可玩小游戏 → AI 换装 → 单文件导出。**

- 约 **53 stars / 5 forks**（截至 2026-11-06 GitHub API）
- 许可证：**MIT**；主语言：**TypeScript**
- 组织：[ZSeven-W](https://github.com/ZSeven-W)；默认分支：`main`；仓库创建约 2026-11-05
- 当前插件版本：**0.1.0-rc.2**；官方称已在 **DSH 0.2.1-rc.5** 测试
- Topics：`ai-game-generator` / `deepseek-harness` / `dsh` / `dsh-plugin` / `game` / `gamedev`
- npm：[https://www.npmjs.com/package/@zseven-w/dsh-game-studio](https://www.npmjs.com/package/@zseven-w/dsh-game-studio)

## 核心特性

| 特性 | 说明 |
|------|------|
| **首轮即可玩** | 两个模板：`collector`（2D 俯视）与 `voxel`（同一玩法的 3D 体素 / three.js）；Agent 按描述改角色、规则、关卡与界面 |
| **侧栏实时预览** | 沙箱面板内运行；改动后重建；下方列出美术槽位、候选图与版本；横屏用宽舞台 |
| **AI 美术 + 花费确认** | `game_studio_generate_asset` 经 OpenAI 图像 API 或本机 Codex CLI 生成 1–4 张编号候选；审批卡先显示服务、张数与预估费用；聊天附图可作参考 |
| **自动试玩** | `game_studio_playtest` 用无头 Chrome/Edge 报告阶段、得分、帧率、错误与截图；非方向键玩法可定义 `window.__autoplay` |
| **版本历史** | 每次改动游戏的构建可存档；`game_studio_restore_version` 恢复代码、规则与美术 |
| **单文件导出** | `game_studio_export` 产出自包含、禁止联网的 HTML，可本地打开或静态托管 |
| **局域网分享到手机** | `game_studio_share` 开临时只读链接，侧栏出二维码（默认约 30 分钟） |
| **我的游戏** | 侧栏「游戏工坊 · 我的游戏」卡片浏览工作区历史作品，直接玩、不经 Agent |

## 实测效果（官方口径）

在真实 DSH + DeepSeek-V4.1-Flash、新会话一句话开局、独立试玩打分（2026-11-04/05）：

- **收集 + 躲避类**（模板主场）：10/10 首轮可玩，中位约 **14 分钟**/局
- **其他类型**（消除、推箱、塔防、投篮、横版、节奏、赛车、卡牌、种菜、咖啡店等）：10/10 可玩，中位约 **26 分钟**；补 `__autoplay`、自定义槽位与横屏后复跑 4 个，约 **8–18 分钟**（样本较小）

## 工具地图（10 个）

| 工具 | 作用 |
|------|------|
| `game_studio_create` | 从 2D/3D 模板创建、构建并显示；含标题、美术风格、横竖屏、`slots` |
| `game_studio_build` | 改 `game.json` / `game.js` 后重建、刷新并记版本 |
| `game_studio_playtest` | 无头试玩与截图；`characters: true` 只要角色特写 |
| `game_studio_generate_asset` | OpenAI / Codex 生候选图；花钱前审批；Codex 可后台跑 |
| `game_studio_apply_asset` | 按编号应用候选图并重建 |
| `game_studio_revert_asset` | 槽位美术回退 |
| `game_studio_versions` | 列出版本（新→旧） |
| `game_studio_restore_version` | 整局恢复到某版本 |
| `game_studio_export` | 导出离线 HTML |
| `game_studio_share` | 开/关局域网只读分享 |

侧栏按钮经 `/game-studio` 走同一套操作。

## 生图服务

| 服务 | 需要 | 费用要点 |
|------|------|----------|
| **OpenAI**（GPT Image） | DSH 凭据或环境变量（默认 `OPENAI_API_KEY`） | 按张计费；审批卡估费；`openaiBaseUrl` 可指兼容网关 |
| **Codex CLI** | 本机已 `codex login` | 计入 Codex 订阅，无需单独 API key；单张更慢、后台完成 |

默认 `imageProvider: auto`（有 OpenAI key 用 OpenAI，否则 Codex）；`confirmSpending: true` 时未批准不生成。

## 适用边界

### 更适合

- 已用或准备用 **DeepSeek Harness**，想快速做**可玩网页小游戏**原型
- 需要侧栏即时预览 + 自动试玩闭环，而不是只吐一堆代码
- 想用 OpenAI / Codex 换皮，且希望**花钱前先确认**
- 最终交付要**单文件离线 HTML** 或局域网手机试玩

### 需要注意

- 强依赖 DSH 生态（非 Claude Code / Cursor 通用 Skill 包）
- 环境：DSH `0.2.1+`、Node.js `24.11+`；试玩需要本机 Chrome/Edge
- 无生图凭据时仍可用代码绘制角色，但美术上限受限
- 官方效果以收集躲避类最稳；异玩法可玩但更耗时
- 定位是**小游戏工坊**，不是 Unity/UE 级 3A 管线（对比 GameFactory-3A / unity-agent-plugin）

## 环境与安装

```sh
dsh plugin --profile web add @zseven-w/dsh-game-studio@latest
dsh web
```

### 示例提示词

```text
做一个小游戏：小猫在厨房里偷鱼干，别被扫地机器人撞到。
做一个 3D 小游戏：小骑士在城堡里收集金币，躲开骷髅兵，60 秒内收集 20 枚金币就算胜利。
做一个消除小游戏：在 6×6 的格子里交换相邻的宝石，60 秒内拿到 1000 分就赢。
用 AI 给主角生成 2 张新美术，我在右栏挑。
导出成一个 HTML 文件。
```

## 游戏项目格式（工作区文件夹）

- `game.json`：标题、风格、横竖屏、槽位与自由规则（→ `window.GAME.rules`）
- `game.js`：游戏逻辑；暴露 `__gameState` / 可选 `__autoplay` / `__renderSlots`
- `assets/` + `ASSETS.json`：槽位图与来源记录
- `dist/index.html`：当前构建；`.studio/`：版本、候选、试玩记录

## 常用配置（`cordis.patch.yml` → `config`）

`imageProvider`、`openaiModel`、`openaiBaseUrl`、`openaiCredential`、`codexPath`、`codexModel`、`codexReasoningEffort`、`codexTimeoutMinutes`、`confirmSpending`。

## 安全与隐私（官方说明摘要）

- 游戏在沙箱 iframe，CSP 禁联网；导出 HTML 不发网络请求
- 出机内容主要是提示词与可选参考图，发往所选生图服务
- 分享链接只读、仅本地网、限时关闭
- API key 不写进项目、日志或工具结果

## 同作者 / 生态链接

| 项目 | 说明 |
|------|------|
| [DSH Crew](https://github.com/ZSeven-W/dsh-crew) | 从 Claude Code / Codex 派活给 DSH Agent |
| [DSH iOS](https://github.com/ZSeven-W/dsh-ios) | 对话内控 iOS 模拟器 / USB 真机 |
| [DSH Android](https://github.com/ZSeven-W/dsh-android) | 对话内控 Android 模拟器 / USB 设备 |
| [DSH Noema](https://github.com/ZSeven-W/dsh-noema) | DSH 长期记忆 |
| [DSH OpenPencil](https://github.com/ZSeven-W/dsh-openpencil) | 对话内查看编辑 `.op` 设计文档 |

依赖致谢：three.js、qrcode-generator、playwright-core（见仓库 `THIRD_PARTY_NOTICES.md`）。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/ZSeven-W/dsh-game-studio |
| **Issues** | https://github.com/ZSeven-W/dsh-game-studio/issues |
| **npm** | https://www.npmjs.com/package/@zseven-w/dsh-game-studio |
| **组织主页** | https://github.com/ZSeven-W |
| **DeepSeek Harness** | https://github.com/deepseek-ai/deepseek-harness |
| **许可证** | MIT |

## My takeaways

1. **定位**：DSH 侧栏内的「一句话小游戏工坊」，强项是即时可玩 + 试玩验收 + 单文件交付，不是通用跨引擎资产生成框架。
2. **与 dsh-blender**：同属 DSH 插件生态；blender 管 3D 资产生产，本库管可玩网页小游戏闭环，可并列收藏。
3. **与 threejs-game-skills**：后者是 Codex/Claude Code 的 Three.js Skills 班组；本库绑死 DSH，但侧栏预览与自动试玩更产品化。
4. **与 GameFactory-3A**：GameFactory 偏多引擎资产 Pipeline；本库偏浏览器小游戏原型与导出。
5. **检索关键词**：dsh-game-studio、DSH 游戏工坊、DeepSeek Harness 插件、一句话做游戏、体素小游戏、单文件 HTML 导出、game_studio_playtest。

## Related

- [[50 来源资料/代码仓库/AI/Agent工具与中间件/dsh-blender - DSH Blender 3D 生产插件|dsh-blender - DSH Blender 3D 生产插件]]
- [[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills]]
- [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]
- [[50 来源资料/代码仓库/游戏开发/sprite-gen - 2D游戏精灵图与动画图集生成Skill|sprite-gen]]
- [[40 知识导航/游戏开发|游戏开发]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]