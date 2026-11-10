---
id: source-20261110-genex-desktop
title: Genex - AI游戏开发桌面Harness
type: source
status: active
created: 2026-11-10
updated: 2026-11-10
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/AI
  - 主题/Agent
  - 主题/Three.js
  - 主题/开源资源
  - 主题/资产生成
author: genex.games
source_type: repo
source_url: https://github.com/genex-games/genex-desktop
source_author: genex-games
source_date: 2026-10-02
summary: MIT 开源的 AI 游戏开发桌面 Harness：用 Claude Code / ChatGPT / 本地模型等，让 Agent 搭引擎、构建并自测；Three.js 内置，可接 Unreal / Unity / Godot / Roblox；配套 Genex Tools（Skill / MCP / CLI / API）做资产生成与发布。
related:
  - "[[40 知识导航/游戏开发|游戏开发]]"
  - "[[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]"
  - "[[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills]]"
  - "[[50 来源资料/代码仓库/游戏开发/awesome-gamedev-agent-skills - 多引擎游戏开发Agent Skills合集|awesome-gamedev-agent-skills]]"
  - "[[50 来源资料/代码仓库/游戏开发/unity-agent-plugin - Unity官方Agent技能插件|unity-agent-plugin]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
---

# Genex - AI游戏开发桌面Harness

## Source summary

**Genex**（[genex-games/genex-desktop](https://github.com/genex-games/genex-desktop)）是 **genex.games** 推出的 **MIT 开源桌面应用**：你在聊天里描述游戏，Agent 负责搭引擎、写代码、构建，并自行试玩验收。官方定位是「游戏开发专用 harness」，不是新引擎。

核心一句话：**把已有的 coding agent（Claude Code / Codex / 本地模型等）编排成可试玩的游戏开发闭环。**

- 约 **800+ stars / ~100 forks**（公开检索口径，约 2026-11）
- 许可证：**MIT**；组织：[genex-games](https://github.com/genex-games)
- 默认分支：`dev`；主语言：**TypeScript**（Electron 桌面）
- 产品页：https://genex.games/desktop
- 配套工具仓：[genex-games/genex-tools](https://github.com/genex-games/genex-tools)（Skill / Claude Code 插件镜像）
- 文档索引（适合 Agent 拉取）：https://genex.games/docs/llms.txt

注意：同名「Genex」还有 Elixir 遗传算法库、正则展开库、以及具身 AI 研究项目 GenEx-world；本笔记只沉淀 **genex.games / genex-desktop**。

## 核心特性

| 特性 | 说明 |
|------|------|
| **多模型角色分工** | 主 Agent 规划、workers 构建、reviewers 验收；可混用 Claude / ChatGPT / Grok / 本地模型 |
| **引擎闭环** | Three.js 内置；聊天中可开启 Unreal / Unity / Godot / Roblox，由 Agent 安装并试玩 |
| **构建时间预算** | 大任务可设 30 分钟到不限时；小改动即时落地 |
| **资产与插件** | 集中管理模型/音效/图片；Blender、Genex Tools、任意 MCP 可作插件 |
| **Harness 自改进** | 从每次构建学习并建议更好工作方式，需你确认后才应用 |
| **开源可改** | 桌面端 MIT；可自建、可改 harness；插件可提交 Marketplace |

## 产品两层结构

| 层 | 是什么 | 入口 |
|----|--------|------|
| **Desktop** | 开源桌面 harness：编排 agent、引擎、试玩 | 仓库 `genex-desktop`；文档 `docs/desktop.md` |
| **Tools** | 游戏资产生成 + 多人 + 发布；Skill / MCP / CLI / API | `genex-tools`；`docs/tools.md`；`https://genex.games/SKILL.md` |

Tools 的钱怎么算：一个账户一个余额，按次计费、失败可退，工具本身无订阅。桌面软件开源免费，不等于云端资产生成与模型用量免费。

## 适用边界

### 更适合

- 想用现有 Claude Code / Codex / Cursor / 本地模型做 **可玩原型**，而不是只生成静态资产
- 需要「聊天 → 引擎工程 → 构建 → 试玩验收」一条桌面闭环
- 浏览器 Three.js 起步，后续再接 Unity / Godot / Unreal / Roblox
- 想给 Agent 挂上游戏向 Skill / MCP / 资产生成能力

### 需要注意

- 仍偏早期：Windows 为预览（签名路线图中）；macOS 需 Apple Silicon；Linux 有 `.deb` / `.rpm` / `.zip`
- 开源的是 harness；Meshy / Tripo / ElevenLabs 等第三方资产与模型订阅另计费
- 与「全自动 3A 量产」不是同一回事；更像可定制的游戏开发编排层
- 贡献桌面端：官方文档要求 macOS Apple Silicon + Git + Node 24

## 快速开始

### 使用桌面应用

```text
1. 从 https://genex.games/desktop 或 GitHub Releases 下载
2. macOS：Genex.dmg；Linux：.deb / .rpm / .zip；Windows：Genex-Setup.exe（预览，可能未签名）
3. 接入 Claude / ChatGPT 订阅，或本地模型 / OpenRouter 等
4. 在聊天中描述游戏；需要时让 Agent 开启目标引擎并自行试玩
```

### 只用 Tools（不装桌面）

```text
把下面这句话贴给 Claude Code / Codex / Cursor 等：
set up https://genex.games/SKILL.md and let it take it from there
```

或：

```text
/plugin marketplace add genex-games/genex-tools
/plugin install genex@genex
```

或：`npx skills add genex-games/genex-tools`

### 本地开发桌面端

```bash
git clone https://github.com/genex-games/genex-desktop.git
cd genex-desktop
nvm install && nvm use   # Node 24
npm ci
npm run studio:dev -- start --profile first-run --fixture app-basics
```

fixture 用脚本化模型与示例游戏，无需真实账号即可跑通界面。

## 路线图要点（官方）

- 签名 Windows 应用
- Self-improving harness 2.0、Computer use
- 更多模型提供方（如 Hermes）
- Rust 原生游戏（Wasm / WebGPU）

无固定日期；落地以 changelog 为准。

## 与知识库内相近资源对比

| 资源 | 侧重点 | 与 Genex 关系 |
|------|--------|----------------|
| [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架\|GameFactory-3A]] | Skills + 多模态资产 Pipeline + 多引擎 Adapter | Genex 偏桌面编排与试玩；GameFactory 偏资产/代码生成管线 |
| [[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集\|threejs-game-skills]] | Three.js 浏览器游戏九技能包 | Genex 内置 Three.js，可互补；前者是 Skill 包，后者是桌面 harness |
| [[50 来源资料/代码仓库/游戏开发/awesome-gamedev-agent-skills - 多引擎游戏开发Agent Skills合集\|awesome-gamedev-agent-skills]] | 跨引擎 Skills 清单与路由 | Genex Tools 的 Skill 可视为另一条「即装即用」产品化路径 |
| [[50 来源资料/代码仓库/游戏开发/unity-agent-plugin - Unity官方Agent技能插件\|unity-agent-plugin]] | Unity 官方编辑器内 Agent 闭环 | Genex 可接 Unity，但是跨引擎桌面 harness，不是 Unity 官方插件 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 桌面仓** | https://github.com/genex-games/genex-desktop |
| **GitHub Tools 仓** | https://github.com/genex-games/genex-tools |
| **官网** | https://genex.games/ |
| **Desktop 产品页** | https://genex.games/desktop |
| **Desktop 说明** | https://genex.games/docs/desktop.md |
| **Tools 说明** | https://genex.games/docs/tools.md |
| **Agent 文档索引** | https://genex.games/docs/llms.txt |
| **Skill 一句话安装** | https://genex.games/SKILL.md |
| **Roadmap** | https://genex.games/docs/desktop/roadmap.md |
| **组织** | https://github.com/genex-games |
| **许可证** | MIT |

## My takeaways

1. **定位**：游戏向 AI harness（编排 agent + 引擎 + 试玩），不是新引擎，也不是纯资产生成库。
2. **写入价值**：与知识库「AI 游戏开发 / Agent Skills」主线一致，且补上「桌面闭环」这一缺口。
3. **落地路径**：先试 Three.js 浏览器游戏；需要引擎时再开 Unity/Godot/Unreal/Roblox。
4. **成本边界**：桌面 MIT 免费；模型订阅与 Tools 按次资产生成分开算。
5. **检索关键词**：Genex、genex-desktop、AI game harness、Three.js、Claude Code 游戏、游戏 Agent 桌面。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架]]
- [[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills - Three.js浏览器游戏Agent Skills合集]]
- [[50 来源资料/代码仓库/游戏开发/awesome-gamedev-agent-skills - 多引擎游戏开发Agent Skills合集|awesome-gamedev-agent-skills - 多引擎游戏开发Agent Skills合集]]
- [[50 来源资料/代码仓库/游戏开发/unity-agent-plugin - Unity官方Agent技能插件|unity-agent-plugin - Unity官方Agent技能插件]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]