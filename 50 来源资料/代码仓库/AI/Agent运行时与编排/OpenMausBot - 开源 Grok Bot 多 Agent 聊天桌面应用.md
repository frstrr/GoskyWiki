---
id: source-20261103-openmausbot
title: OpenMausBot - 开源 Grok Bot 多 Agent 聊天桌面应用
type: source
status: active
created: 2026-11-03
updated: 2026-11-03
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/多智能体编排
  - 主题/AI编程Agent
  - 主题/桌面应用
  - 主题/ComputerUse
author: milind-soni
source_type: repo
source_url: https://github.com/milind-soni/OpenMausBot
source_author: milind-soni
source_date: 2026-11-03
summary: 开源 Grok Bot / Meta Muse / OpenAI dots / Manus Cue 替代品：以即时通讯形态管理多 Agent 团队，自带 Claude/Codex/Grok 本地 CLI、云端或本地计算机、Composio 应用连接与审批卡片；Electron 桌面端，Apache-2.0，local-first。
related:
  - '[[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]'
  - '[[50 来源资料/代码仓库/AI/Agent运行时与编排/Runner - 本地 AI Agent 编排器|Runner - 本地 AI Agent 编排器]]'
  - '[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenDots - 持久化 AI 同事工作区模板|OpenDots - 持久化 AI 同事工作区模板]]'
  - '[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]'
  - '[[40 知识导航/AI 工具使用|AI 工具使用]]'
  - '[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]'
---

# OpenMausBot - 开源 Grok Bot 多 Agent 聊天桌面应用

## Source summary

**OpenMausBot**（又名 **MausBot**，曾用名 **OpenGrokBot**）是独立的开源项目，定位为 **Grok Bot** 的开源替代，也是 **Meta Muse、OpenAI dots、Manus Cue** 一类「个人 Agent 团队 / 聊天式同事」产品的开源对应物。核心形态：侧边栏里每个 bot 都是真实 Agent（本地跑 Claude / Codex / Grok CLI），有独立人格、模型、云电脑/本地 VM 与已连接应用；像联系人一样对话，边看它们工作，边审批关键操作。

- 开源项目，GitHub：[milind-soni/OpenMausBot](https://github.com/milind-soni/OpenMausBot)
- 作者：**Milind Soni**（[@milind-soni](https://github.com/milind-soni)）
- 语言：**TypeScript**（React 19 + Electron；本地 harness server）
- 约 **11.5k stars / 1.6k forks**（截至 2026-11-03）
- 许可证：**Apache-2.0**（`enterprise/` 另有源码可用协议；删除该目录即为纯开源版）
- 最新发布：**v0.1.139**（2026-11-03）
- 平台：**macOS**（Apple Silicon 与 Intel，已签名公证）· **Windows x64** · **Ubuntu 24.04 x64**（.deb / AppImage，Beta）
- 官网：[openmausbot.com](https://www.openmausbot.com)
- 口号取向：bring-your-own-agent, local-first；与 xAI / Meta / OpenAI / Manus **无隶属关系**
- 免责声明：与任何加密货币无关；任何套用 OpenMausBot / Maus / SupaMaus 名称的代币均非官方

一句话概括：**把一队可操作电脑、可连应用的 AI Agent，收成一个本地优先的聊天 App。**

## 解决什么问题

封闭产品（Grok Bot、Muse、dots、Cue、Instinct）通常把模型、电脑与数据留在厂商侧。OpenMausBot 用同一「通讯录式多 bot」形状，但改成自托管、本地优先、自带已有 Agent 订阅。

| 痛点 / 对照 | OpenMausBot 做法 |
|-------------|------------------|
| Grok Bot 只绑 Grok + 共享云电脑 | 每 bot 可选 Claude / Codex / Grok；数据在 `~/.openmausbot` |
| Muse / dots 单 Agent、区域或套餐限制 | 多 Agent 团队；用你已有 CLI 登录，无需新账号代理 |
| Cue / Instinct 邀请制、密码托管云端 | 无邀请；OAuth；关键动作走审批卡片 |
| 多个 CLI Agent 窗口散落、难当「同事」 | 消息 App UX：置顶、未读、频道、部门/团队导入 |
| Agent 能改文件却无人把关 | Shell / 文件编辑 / 提问以内联 Allow·Deny 卡片呈现 |

对比文（官网）：[Grok Bot vs OpenMausBot](https://www.openmausbot.com/blog/grok-bot-vs-openmausbot)、[Muse vs Grok Bot vs Dots vs Cue](https://www.openmausbot.com/blog/muse-vs-grok-bot-vs-dots-vs-cue)

## 核心能力

| 能力 | 说明 |
|------|------|
| **每 bot 独立大脑** | 模型选择器带 provider 轨道；对话中途可换模型；不可用 provider 会标明原因 |
| **每 bot 一台电脑** | Boat 云端 Linux 桌面、隔离 Local VM，或（经明确 opt-in）本机 computer use；可预览屏幕 / 浏览器接管桌面 |
| **动作前询问** | Permission broker：shell、改文件、提问都变成聊天内决策卡 |
| **连接应用** | 经 Composio Sessions 一键接 Gmail、Slack、GitHub、Notion、Linear 等 500+ 应用 |
| **像聊天一样管 bot** | 右键置顶、未读、改资料、复制、隐藏、删除 |
| **密钥本地一次配置** | App Settings 粘贴密钥，本地持久化；UI 只见「已配置」标志 |
| **频道 / 上下文** | Work、Personal、项目分频道：独立 transcript、共享指令、工作目录、响应规则与 bot 名册 |
| **Jev 快速路由** | 决策模型（TypeSafe Jev 或 OpenRouter）在 Auto 房间毫秒级选应答 bot |
| **团队 Markdown 一键安装** | [BotMRR](https://botmrr.io) / 本地 `.md` / GitHub URL 导入整队 bot、频道、playbook、例程（不含凭证与对话） |
| **部门分享** | 导出部门包（指令、技能、群聊、例程槽位等）；导入后技能默认关、例程暂停 |
| **语音与通话** | ElevenLabs / Fish Audio / Grok TTS / Mac 内置声 / 本地 Chatterbox；通话目前偏 macOS |
| **触发器** | 日程例程 + Webhook + 已连接应用事件（如邮件到达）驱动 bot |
| **MCP 控制面** | stdio MCP：查 bot/频道、发任务、等完成、换模型、打断；不暴露审批授权、删改、凭证、电脑生命周期 |
| **无头 CLI / CI** | `openmausbot team apply` / `openmausbot run ... --export` 等脚本化编排 |

## 架构要点

两进程：**App（React，约 5199）** 只发类型化 HTTP 命令并折叠一条 SSE；**Harness server（127.0.0.1:8799）** 拥有全部 Agent 进程，把各厂商协议归一成运行时事件流（按线程 NDJSON）。

| 层 | 位置 | 作用 |
|----|------|------|
| Drivers | `server/drivers/` | Claude / Codex / Grok 本地 CLI + 云电脑 agent；未知 driver 降级为 unavailable |
| Harness | `server/harness/` | 实例注册表 + 事件总线 |
| API | `server/index.ts` | bots、turns、审批、模型目录、电脑生命周期、连接器 |
| Voice | `server/tts/` | 多 TTS 后端；密钥留在 harness |
| App | `src/` | 聊天壳；服务端状态，客户端无自建传输 |
| Desktop | `electron/` | macOS / Windows / Ubuntu 壳 + 嵌入式 harness |

依赖生态（节选）：Boat（云电脑赞助方）、Composio、Cua Driver（本机 computer use）、ElevenLabs / Fish Audio。

## 快速开始

**发行版（推荐）**：harness 已嵌入，无需单独起服务。见 [Releases](https://github.com/milind-soni/OpenMausBot/releases/latest)。

| 平台 | 安装包 |
|------|--------|
| macOS Apple Silicon | `OpenMausBot.dmg`（已签名公证） |
| macOS Intel | `OpenMausBot-intel.dmg` |
| Windows x64 | `OpenMausBot-setup.exe`（尚未代码签名，SmartScreen 可能提示） |
| Ubuntu 24.04 x64 | `.deb`（推荐）或 `AppImage`（Beta） |

也可：`npm i -g openmausbot` 或 `npx openmausbot`；支持 `serve`、Tailscale / 托管隧道与手机配对（见仓库 `docs/deploy-vps.md`、`docs/self-hosting.md`）。

**从源码：**

```bash
git clone https://github.com/milind-soni/OpenMausBot && cd OpenMausBot
pnpm install
pnpm dev:server    # 127.0.0.1:8799
pnpm dev           # http://127.0.0.1:5199
pnpm dev:desktop   # Electron（需上面两个仍在跑）
```

**环境要求**：macOS / Windows / Ubuntu 24.04 x64；**Node 24+**；**pnpm**；至少一个已登录 Agent CLI：[`claude`](https://claude.com/claude-code)、[`codex`](https://github.com/openai/codex)、[`grok`](https://x.ai/cli)。

可选密钥（App Settings）：Composio project key、Boat API key、ElevenLabs / Fish Audio（语音）。

## 桌面能力矩阵（摘要）

| 能力 | macOS | Ubuntu Xorg | Ubuntu Wayland |
|------|:-----:|:-----------:|:--------------:|
| 打包应用 + 本地 CLI | 支持 | Beta | Beta |
| Composio / Boat 云电脑 | 支持 | Beta | Beta |
| 本机 bot 控制 | 支持 | Beta（显式 opt-in） | 关闭（安全门控） |
| 本机听写 | 支持 | 规划中 | 规划中 |

## 适用场景

### 更适合

- 想要 **Grok Bot / Muse / dots / Cue 开源自托管** 形态，且已有 Claude / Codex / Grok 订阅
- 需要 **多 bot 分工 + 频道上下文 + 审批闸门** 的个人或小团队
- 希望 Agent **能用云电脑或本机 + 连 Gmail/Slack/GitHub**，数据默认留在本机目录
- 要用 **MCP / headless CLI** 从 Cursor、脚本或 CI 编排同一套团队

### 不太适合

- 只想要单一聊天框、不需要电脑操作与应用连接
- 无法安装任何 Agent CLI、也不愿接第三方云电脑/连接器
- 强依赖 Ubuntu Wayland 本机 computer use（当前禁用）
- 需要厂商级托管 SLA（项目仍标 Early but real，Webhook 默认本地接收端）

## 同类对照（本库相关）

| 项目 | 差异要点 |
|------|----------|
| [[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用\|AionUi]] | 同为 Electron Cowork 桌面；AionUi 更强调内置 Agent 与 20+ CLI 统一编排；OpenMausBot 更强调「通讯录式多 bot + 每 bot 电脑 + Grok Bot 替代叙事」 |
| [[50 来源资料/代码仓库/AI/Agent运行时与编排/Runner - 本地 AI Agent 编排器\|Runner]] | Runner 偏 coding crew / mission / 真实 PTY；OpenMausBot 偏消息产品 UX + 电脑/应用/审批 |
| [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenDots - 持久化 AI 同事工作区模板\|OpenDots]] | OpenDots 偏工作区模板；OpenMausBot 是完整桌面产品 |
| [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手\|OpenClaw]] | OpenClaw 偏 IM 通道接入；OpenMausBot 可暴露 MCP，但主交互是自有桌面聊天壳 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/milind-soni/OpenMausBot |
| **官网** | https://www.openmausbot.com |
| **下载** | https://www.openmausbot.com/download |
| **最新 Release** | https://github.com/milind-soni/OpenMausBot/releases/latest |
| **团队 playbook 仓库** | https://github.com/milind-soni/openmausbot-teams |
| **BotMRR 团队市场** | https://botmrr.io |
| **Grok Bot 对比文** | https://www.openmausbot.com/blog/grok-bot-vs-openmausbot |
| **GitHub Sponsors** | https://github.com/sponsors/milind-soni |
| **Topics** | `ai-agents` · `claude-code` · `codex` · `computer-use` · `electron` · `grok-bot-alternative` · `self-hosted` |

## My takeaways

1. **产品形状对标封闭「Agent 同事」**：不是又一个 coding terminal multiplexer，而是 messaging-first 的多 bot 团队 + 电脑 + 应用。
2. **BYO Agent 是关键差异**：复用本机 `claude` / `codex` / `grok` CLI 与已有订阅，中间不插代理账号。
3. **安全模型清晰**：local-first（`~/.openmausbot`）+ 审批卡片 + Wayland 本机控制默认关闭，适合作为「能动手的 Agent」实验床。
4. **可编排性超出桌面 UI**：MCP 有界控制面 + `openmausbot` headless CLI，便于接到现有 Cursor / CI 流程。
5. **生态打包路径短**：BotMRR / Markdown 团队包 / 部门分享，降低「从零捏人设与频道」成本。
6. **选型时与 AionUi、Runner 分工**：要 Grok Bot 式聊天花名册与每 bot 电脑 → OpenMausBot；要更广 CLI Cowork 或 coding mission 舰队 → 对照 AionUi / Runner。

## Related

- [[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/Runner - 本地 AI Agent 编排器|Runner - 本地 AI Agent 编排器]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenDots - 持久化 AI 同事工作区模板|OpenDots - 持久化 AI 同事工作区模板]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]