---
id: source-20260909-runner
title: Runner - 本地 AI Agent 编排器
type: source
status: active
created: 2026-09-09
updated: 2026-09-09
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/多智能体编排
  - 主题/AI编程Agent
  - 主题/桌面应用
author: yicheng47
source_type: repo
source_url: https://github.com/yicheng47/runner
source_author: yicheng47
source_date: 2026-09-09
summary: 本地桌面 AI 编排器（Rust + GPUI）：用 crew / mission 组织多个 CLI coding agent（Claude Code、Codex 等）并行协作；真实 PTY、事件日志、MCP 自驱、跨厂商 peer coding；GPL-3.0。
related:
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/Edict - 三省六部制 Multi-Agent 编排|Edict - 三省六部制 Multi-Agent 编排]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# Runner - 本地 AI Agent 编排器

## Source summary

**Runner** 是本地桌面 **AI Orchestrator / Agent Multiplexer**：把 Claude Code、Codex 等 CLI coding agent 收成一支可编排的舰队（runner → crew → mission），在同一应用里管终端、协作与事件流，而不是散落在多个终端窗口。

- 开源项目，GitHub：[yicheng47/runner](https://github.com/yicheng47/runner)
- 作者：Yicheng Wang（Jason Wang / [@yicheng47](https://github.com/yicheng47)）
- 语言：**Rust**（UI：gpui-ce；终端：`alacritty_terminal`；状态：SQLite）
- 约 **86 stars / 4 forks**（截至 2026-09-09）
- 许可证：**GPL-3.0-only**（2026-08-22 前发布版本曾为 MIT，仍按当时许可）
- 状态：**alpha**，积极开发；原生 **macOS（Apple Silicon）** 与 **Windows x64**（自 0.8.0）；不支持 Intel Mac / Linux
- 口号：*Spawn a runner. Create your crew. Ship the feature.*
- 经验分享帖（V2EX）：[Agent 时代的独立开发经验分享](https://www.v2ex.com/t/1240659)

## 解决什么问题

多 Agent 并行时代，常见痛点是会话散落、协作靠人肉复制粘贴、被单一 AI 厂商绑架。

| 痛点 | Runner 方案 |
|------|-------------|
| 多 Agent 窗口难管、长任务易忘 | Arc 式 **Multi-Agent Tab**：聊天/任务分 tab、文件夹分组；spinner / 完成点提示 |
| 跨厂商模型 peer coding 难 | **Crew**：多 runner 编队，一人 lead；经内置 CLI 写本地 **NDJSON** 事件日志跨 session 沟通 |
| 人肉改队员/组队麻烦 | **MCP Self Drive**：app 暴露 MCP，主 agent 可创建 crew、发起 mission，Runner 变可视化 sandbox |
| 想 Agent-agnostic | 同一工作流切换 Claude Code / Codex 等 runtime，不被单一 provider 锁死 |
| TUI subagent 可视化弱 | 原生 GPU UI + 每 slot 真实 PTY，可看每个 agent 在干什么 |

同类对照：cmux / herdr / [Orca](https://github.com/stablyai/orca) 等同属 Agent Orchestrator 赛道；Runner 强调 **可定制 workflow + 跨 Agent 协作 + MCP 自驱**。

## 核心概念

| 概念 | 说明 |
|------|------|
| **Runner** | 可复用的 agent 配置：runtime、角色、system prompt、工作目录 |
| **Crew** | 由若干 runner 槽位组成，**恰好一个 lead**；可挂团队约定与 Definition of Done |
| **Mission** | 一支 crew 执行一个目标：每槽位 spawn 一个真实 PTY；协作走 append-only 事件日志；`ask_human` 冒泡给人 |
| **Chat** | 不需要 mission 的 1:1 PTY；tab 最多三栏并排（如 Claude + Codex 同屏） |
| **MCP** | `runner-mcp` stdio sidecar；Settings 一键注册到 Claude Code / Codex（macOS 另有 TRAE CLI） |

## 核心能力

| 能力 | 说明 |
|------|------|
| **Crews** | 角色 / prompt / 单一 lead；团队约定与完成定义继承到 mission |
| **Missions** | 实时事件 feed；日志落盘可回放；退出/崩溃可恢复；人工决策入口 |
| **Chats** | Tab + 分栏 + sidebar 文件夹；并行 agent 可扫描状态 |
| **真实终端** | `alacritty_terminal` 网格 + GPUI GPU 绘制；ANSI / 鼠标 / IME / 万行 scrollback |
| **多窗口** | macOS `⇧⌘N` / Windows `Ctrl+Shift+N`；共享 session 有所有权交接，避免双写损坏终端 |
| **MCP 驱动** | `mission_start` / `mission_feed` / `mission_post_human_signal` / `session_start_direct` 等；agent 可派发 crew 再继续自己干活 |
| **Projects** | 绑定工作目录；该项目下启动的 chat/mission 继承 cwd 并分组 |
| **内置 `runner` CLI** | 被 spawn 的 agent 用其读写 NDJSON、查 roster、发信号 |
| **更新** | macOS Sparkle；Windows 后台下载 + minisign 验签，Authenticode 签名 |

## 支持的 Agent

| Agent | macOS (Apple Silicon) | Windows (x64) |
|-------|:---------------------:|:-------------:|
| Claude Code | 支持 | 支持 |
| Codex | 支持 | 支持 |
| TRAE CLI | 实验性 | 未验证（默认关） |

需自行安装各 Agent CLI；PATH 检测，可在 Settings → Agents 覆盖可执行文件。Windows 上 Claude Code 需 Git for Windows（Git Bash）；npm 系 CLI 需 Node.js。Agent **原生跑 Windows，不依赖 WSL**。

## 示例 Crew

默认形态是 **peer-coding** 双人环：一人实现、一人审 diff，循环到 review 干净。

| Runner | Runtime | 角色 |
|--------|---------|------|
| **@coder**（lead） | 默认 `codex` | 开分支、实现、跑检查、交 diff、修 findings |
| **@reviewer** | 默认 `codex` | 只读 working-tree diff，报 must-fix（file:line），不改代码 |

把 `@coder` 换成 `claude-code` 即跨厂商 pair。更多示例在 `examples/`：`dev-crew`、`docs-crew`、`tic-tac-toe`、`werewolf`、`tomb-raid` 等。

## 技术栈与仓库结构（摘要）

| 层 | 技术 |
|----|------|
| UI | [gpui-ce](https://github.com/gpui-ce/gpui-ce)（Zed GPUI 社区 fork） |
| 终端 | alacritty_terminal；Windows 捆绑 ConPTY |
| 后端 | `runner-backend`：SQLite、session manager、event bus、router、MCP（**不依赖 UI**） |
| 协议 | `cli/` + `runner-core`：app 与 agent 之间的事件/CLI 协议 |
| 设计 | `design/runner.pen`（[pen.dev](https://www.pen.dev/) 画布） |

原则（作者在 V2EX 强调）：**`AGENTS.md` 是唯一 agent 指南**，`CLAUDE.md` 仅 symlink，避免双份规则漂移；后端与 UI 解耦，便于前端重写。

## Agent 友好的 docs 结构（可复用经验）

作者用类似 wiki 的目录给 Agent **减 token**：

```text
docs/
├── README.md                 # 目录约定
├── product/vision.md
├── arch/                     # 仍生效的架构决策
├── features/                 # 进行中的 feature spec（文件名前缀 = GitHub issue 号）
│   ├── README.md             # 索引：一句话 + issue + Dropped
│   └── archive/              # 已上线，文件名不变
├── impls/                    # how + 如何验证；含 Current state / standing rules
│   └── archive/
└── tests/                    # 人工 smoke 清单
```

省 token 要点：

1. **目录即状态**：活文档在外，上线/取代进 `archive/`；Agent `ls` 即可知现状。
2. **每目录 README 当索引**：先读索引再决定是否打开正文。
3. **编号 = issue 号**：spec / issue / PR 三边可互找。
4. **impl 置顶 Current state + standing rules**：新 mission brief 只引用这一段，不喂整本 log。

## 作者推荐的周边工具链（来自 V2EX）

| 用途 | 工具 |
|------|------|
| UI / UX | [pen.dev](https://www.pen.dev/)（原 Pencil，免费；强调开放 JSON 数据结构 + MCP，适合 agent 改设计） |
| 需求 & Roadmap | GitHub Issues & Project |
| IDE | [Zed](https://zed.dev/) + Vim（看重 worktree、轻量、GPUI 性能、配置可被 agent 改） |
| Terminal | [Ghostty](https://ghostty.org/)（配置文件优于纯 UI，便于 agent 编辑） |
| 同类 Orchestrator | Runner（自研 dogfooding）；[Orca](https://github.com/stablyai/orca) |

工作流分层：小需求可 Agent 闭环（UI 改动人工确认 `.pen` + smoke）；大型重构（如 Tauri → GPUI）强依赖分 phase + 人工 checkpoint，可并行 mission 交给 peer-coding crew，另派 agent 处理 merge conflict。

## 安装与获取

从 [Releases](https://github.com/yicheng47/runner/releases/latest) 下载：

- macOS：Apple Silicon `.dmg`（签名 + 公证，Sparkle 更新）
- Windows：`Runner-Setup-…-x64.exe`（需 Win10 1809+；可能触发 SmartScreen，发布者名为 *Open Source Developer Yicheng Wang*）

## 适用场景

- 需要 **本地桌面** 同时管多个 coding agent，且要看真实终端输出
- 想做 **跨厂商 peer coding / crew mission**，并希望协作日志可回放
- 希望日常 driver agent 通过 **MCP** 派发 coder/reviewer crew
- 评估 Agent Orchestrator 赛道（对照 Luvus / AionUi / Orca）
- 学习 **Agent 友好的 docs 归档与 AGENTS.md 单一真相源** 实践

**不太适合**：只要单 Agent CLI；需要 Linux / Intel Mac；不能接受 **GPL-3.0** 衍生义务；要的是 IM「下旨」式制度化编排（看 [[50 来源资料/代码仓库/AI/Agent运行时与编排/Edict - 三省六部制 Multi-Agent 编排|Edict]]）或纯终端 Mission Control（看 [[50 来源资料/代码仓库/AI/Agent运行时与编排/Luvus - AI Agent 任务控制中心|Luvus]]）。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/yicheng47/runner |
| **Releases** | https://github.com/yicheng47/runner/releases |
| **架构文档** | https://github.com/yicheng47/runner/blob/main/docs/arch/arch.md |
| **产品愿景** | https://github.com/yicheng47/runner/blob/main/docs/product/vision.md |
| **贡献指南 AGENTS.md** | https://github.com/yicheng47/runner/blob/main/AGENTS.md |
| **设计稿 runner.pen** | https://github.com/yicheng47/runner/blob/main/design/runner.pen |
| **GPUI 重写记录** | https://github.com/yicheng47/runner/blob/main/docs/impls/gpui-rewrite/README.md |
| **Demo（YouTube）** | https://www.youtube.com/watch?v=eKXcfxC4m1U |
| **V2EX 经验分享** | https://www.v2ex.com/t/1240659 |
| **同类：Orca** | https://github.com/stablyai/orca |
| **设计工具 pen.dev** | https://www.pen.dev/ |
| **作者** | https://github.com/yicheng47 |

## My takeaways

1. **定位**：本地「agent 工作台」——组织的是 agent 的 PTY / crew / mission，不是又一个 IDE。
2. **差异化**：跨 session NDJSON 协作 + MCP 让 app 变 sandbox；强调 Agent-agnostic，避免单厂商锁定。
3. **工程可学点**：`AGENTS.md` 单一真相源、backend/UI 解耦、docs 目录即状态 + issue 号命名——直接可搬到自有仓库降 token。
4. **与 Luvus / AionUi 对照**：Luvus 偏终端多路复用 + worktree Mission Control；AionUi 偏 Multi-AI Cowork 桌面；Runner 偏 crew/mission 编排 + MCP 自驱 + GPUI 原生终端。
5. **许可与平台**：GPL-3.0；目前仅 Apple Silicon Mac + Windows x64——选型前先确认。

## Related

- [[50 来源资料/代码仓库/AI/Agent运行时与编排/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/Edict - 三省六部制 Multi-Agent 编排|Edict - 三省六部制 Multi-Agent 编排]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/FastClaw - Go 轻量多 Agent 运行时|FastClaw - Go 轻量多 Agent 运行时]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]