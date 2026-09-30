---
id: source-20260831-luvus
title: Luvus - AI Agent 任务控制中心
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/AI编程Agent
  - 主题/终端工具
author: RizRiyz
source_type: repo
source_url: https://github.com/RizRiyz/luvus
source_author: RizRiyz
source_date: 2026-08-31
summary: 跨平台 Rust 终端多路复用器，专为 AI 编码 Agent 设计——持久化工作区、多窗格编排、Agent 状态感知、Git/GitHub 集成、worktree 多 Agent 协作与 UHP 1.0 统一 Harness 协议。
related:
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[50 来源资料/代码仓库/AI/JIT-Agent - 即时生成 Agent Harness|JIT-Agent - 即时生成 Agent Harness]]"
---

# Luvus - AI Agent 任务控制中心

## Source summary

**Luvus** 是一个 **AI 编码 Agent 的任务控制中心（Mission Control）**，用 Rust 编写的跨平台终端多路复用器（类似 tmux，但面向 AI Agent 工作流深度优化）。

- 开源项目，GitHub：[RizRiyz/luvus](https://github.com/RizRiyz/luvus)
- 语言：**Rust**；约 **607 stars**（2026-08-31）
- 平台：**macOS · Linux · Windows**
- 许可证：**Apache-2.0**
- 定位：持久化工作区 + 多窗格终端 + Agent 状态监控 + Git/worktree 编排

## 解决什么问题

日常用多个 AI 编码 Agent（Claude Code、Codex、Cursor 等）时，常见痛点：

| 痛点 | Luvus 方案 |
|------|-----------|
| 终端会话关闭后 Agent 状态丢失 | 后台 server 持久化 tabs、panes、布局、终端状态与命名 session |
| 多 Agent 并行难以管理 | 自动检测 Agent，展示 blocked/working/done/idle 状态、token、cost、context |
| 切换项目成本高 | 持久 workspace：打开、重命名、pin、切换项目 |
| 多 Agent 改同一仓库冲突 | worktree 编排：创建 worktree、分配 Agent、预留文件路径、质量门禁、合并 |
| 需在终端与 GitHub 间来回切 | 内置 Git-aware 文件树、status/branch/PR/issue 视图 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **持久工作区** | 后台 server 保持 tabs、panes、布局、终端状态、命名 session |
| **完整窗格控制** | 分割、缩放、聚焦、命名、重排；支持鼠标、TUI、CLI |
| **Agent 感知** | 自动检测支持的 Agent，显示状态、session 标题、token、cost、context，可选声音提醒 |
| **Agent 工作流** | 启动、命名、发消息、inspect、wait、resume、send keys；可 fork Claude/Grok/Codex/Pi/OMP session 并保留 context |
| **文件与代码** | Git-aware 文件树、diff 查看、在外部编辑器或 pane 中打开 |
| **Git 与 GitHub** | status、branch、commit、contributor、PR、issue、仓库活动 |
| **Worktree 编排** | 创建 worktree、协调依赖任务、预留路径、分配 Agent、质量门禁、合并 |
| **远程与多客户端** | SSH attach、多客户端独立 viewport、窄屏 compact switcher |
| **终端工具** | 可配置 Scrollback Memory、跨 pane 搜索、copy mode、链接点击 |
| **可扩展模块** | actions、events、settings、startup hooks、sidebar docks、Luvus Bar widgets |
| **UHP 1.0** | Universal Harness Protocol：统一 method registry、本地 IPC、snapshot、event stream、semantic waits |
| **自定义界面** | 双 sidebar、键位 remap、8 语言、本地/社区主题 |

## 支持的 Agent

| Agent | 实时状态 | Session 恢复 | 精确事件 (hook) |
|-------|:--------:|:------------:|:---------------:|
| Claude Code | ✓ | ✓ | ✓ |
| GitHub Copilot CLI | ✓ | ✓ | ✓ |
| Codex | ✓ | ✓ | ✓ |
| opencode | ✓ | ✓ | ✓ |
| Kimi | ✓ | ✓ | ✓ |
| Grok | ✓ | ✓ | ✓ |
| Hermes CLI | ✓ | ✓ | — |
| Pi | ✓ | ✓ | — |
| Oh My Pi (omp) | ✓ | ✓ | ✓ |
| Muse Code | ✓ | ✓ | — |
| Fx | ✓ | ✓ | — |
| Cursor | ✓ | resume command | — |
| Gemini · Aider · Amp · Droid · Qwen · Kiro | ✓ | — | — |

实时状态无需 Agent 额外集成；Cursor 支持 resume command 但无 hook 级精确事件。

## 安装

```sh
# macOS / Linux
curl -fsSL https://luvus.dev/install.sh | sh

# Homebrew
brew install RizRiyz/luvus/luvus
```

```powershell
# Windows PowerShell
irm https://luvus.dev/install.ps1 | iex
```

也可通过 [crates.io](https://crates.io/crates/luvus) 安装。

## 快速开始

```bash
luvus          # 启动或重新 attach 到 session
luvus doctor   # 检查 git、gh、ssh 等环境
luvus update   # 检查并安装新版本
```

在项目目录运行 Luvus，分割 pane，启动 Agent 即可——Luvus 会自动检测支持的 Agent。

**macOS 提示**：在 **系统设置 → 键盘 → 键盘快捷键 → 输入源** 中关闭「选择上一个输入源」，以释放 `Ctrl+Space`。

## 适用场景

- **多 Agent 并行开发**：同一仓库开多个 pane/worktree，各跑不同 Agent，统一监控状态与 cost
- **长时间 Agent 任务**：server 持久化 session，断开终端后仍可 reattach
- **需要 Git/GitHub 上下文**：不想在终端、浏览器、IDE 间反复切换
- **Agent 编排实验**：UHP 1.0 协议适合构建 harness 与 orchestrator
- **Windows 原生体验**：跨平台安装脚本，不必依赖 WSL + tmux

**不太适合**：只需单一 Agent CLI、不需要多窗格/持久 session 的简单场景；或已深度绑定 tmux/screen 且不愿迁移工作流的用户。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/RizRiyz/luvus |
| **官网** | https://luvus.dev |
| **文档** | https://luvus.dev/docs/ |
| **Releases** | https://github.com/RizRiyz/luvus/releases |
| **crates.io** | https://crates.io/crates/luvus |
| **添加 Agent 支持** | https://luvus.dev/docs/extend/adding-agent-support/ |
| **CONTRIBUTING** | https://github.com/RizRiyz/luvus/blob/main/CONTRIBUTING.md |

## My takeaways

1. **Agent-first 终端**：不是通用 tmux 替代品，而是把「多 Agent 并行 + 状态可见 + Git 上下文」作为一等公民。
2. **持久 server 架构**：tabs/panes/layout/session 由后台 server 维护，适合长时间 Agent 任务与 reattach。
3. **Worktree 编排**：多 Agent 改同一 repo 时，worktree + 路径预留 + 质量门禁是实用差异化能力。
4. **UHP 1.0**：若做 Agent harness/orchestrator，可复用其 versioned method registry 与 semantic waits，与 [[50 来源资料/代码仓库/AI/JIT-Agent - 即时生成 Agent Harness|JIT-Agent]] 的 harness 思路可对照。
5. **Cursor 已支持**：实时状态 + resume command，适合作为 Cursor Agent 的多 session 管理前端。

## Related

- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
- [[50 来源资料/代码仓库/AI/JIT-Agent - 即时生成 Agent Harness|JIT-Agent - 即时生成 Agent Harness]]
- [[50 来源资料/代码仓库/AI/Agent Substrate - AI Agent 大规模运行时|Agent Substrate - AI Agent 大规模运行时]]
