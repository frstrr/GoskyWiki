---
id: source-20261103-windows-mcp
title: Windows-MCP - Windows 桌面 Computer Use MCP
type: source
status: active
created: 2026-11-03
updated: 2026-11-03
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/MCP
  - 主题/开源资源
  - 主题/Windows
  - 主题/桌面自动化
  - 主题/Computer Use
author: CursorTouch
source_type: repo
source_url: https://github.com/CursorTouch/Windows-MCP
source_author: CursorTouch / Jeomon
source_date: 2026-11-03
summary: 面向 Windows 的 Computer Use MCP 服务器：用原生 UI Automation（非 OCR 猜坐标）让任意 LLM/Agent 操控键鼠、窗口、文件与应用；PyPI 可 uvx 一键启动，已进 MCP Registry，另有 windows-use Agent。
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill - AI Agent 登录态浏览器操控]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/ego-lite - AI Agent 人机并行浏览器|ego-lite - AI Agent 人机并行浏览器]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/codex-with-chatgpt - ChatGPT 规划大脑与 Codex 执行桥|codex-with-chatgpt - ChatGPT 规划大脑与 Codex 执行桥]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# Windows-MCP - Windows 桌面 Computer Use MCP

## Source summary

**Windows-MCP**（仓库 [CursorTouch/Windows-MCP](https://github.com/CursorTouch/Windows-MCP)）是给 AI 补「手」的 **Windows MCP Server**：Agent / Cursor / Claude Desktop 等只发自然语言意图，中间经 MCP（JSON-RPC）翻译，底层用 **Windows 原生 UI Automation** 真去点鼠标、敲键盘、开窗口、管文件——**不靠 OCR 截图猜位置**，视觉可选，官方强调支持任意 LLM。

- 约 **10,983 stars** / **1,183 forks**（2026-11-03 GitHub API）；许可证 **MIT**；主语言 **Python**
- Topics：`ai` · `desktop` · `mcp` · `tools` · `windows` · `windows-automation`
- mcp-name：`io.github.CursorTouch/Windows-MCP`；已上 **PyPI**（`windows-mcp`）与 **MCP Registry**
- 衍生 Agent：[windows-use](https://pypi.org/project/windows-use/)（基于 Windows-MCP 的桌面 Agent）
- 支持系统：**Windows 10 / 11**；当前包要求 **Python 3.14+**
- 动作间典型延迟约 **0.2–0.5 秒**（受前台应用数、负载与 LLM 推理影响）

定位对比：[[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill]] / [[50 来源资料/代码仓库/AI/Agent工具与中间件/ego-lite - AI Agent 人机并行浏览器|ego-lite]] 偏**浏览器内**操控与登录态；**Windows-MCP 偏整个 OS 桌面**（文件、窗口、应用、Registry、PowerShell），浏览器只是工具之一（另有 `use_dom=True` 网页 DOM 模式）。

## 解决什么问题

| 痛点 | 本项目方案 |
|------|------------|
| LLM 会聊不会动手，缺系统级「Computer Use」 | 标准 MCP 暴露键鼠/窗口/文件/应用工具，任意 MCP 客户端可接 |
| 截图+OCR 自动化界面一变就崩 | 读系统 UI 元素树，比像素坐标稳 |
| Playwright/Selenium 只管浏览器 | 覆盖 OS 级操作；浏览器可用 Snapshot `use_dom=True` 过滤 chrome UI |
| 每个 Agent 宿主要单独写桌面胶水 | 一次装 MCP，Claude Desktop / Cursor / Claude Code 等共用 |
| 远程/高危工具敞着口 | `--exclude-tools`、`--auth-key`、IP 白名单、桌面控制权门控（人机可接管） |

## 核心能力

| 能力 | 说明 |
|------|------|
| **原生 UI 自动化** | 对接 Windows UI Automation；`App` 可解析本地化开始菜单显示名（中文「打开画图」可用） |
| **输入与导航** | `Click` / `Type` / `Scroll` / `Move`（含拖拽）/ `Shortcut` / `Wait` / `WaitFor` / `MultiSelect` / `MultiEdit` |
| **看见桌面** | `Screenshot`（偏视觉、快）；`Snapshot`（含可交互元素 id，可 `use_vision` / `use_dom`）；`DisplayInventory` |
| **应用与进程** | `App` 启动/切换/缩放窗口；`Process` 列举/结束；`Notification` Toast |
| **文件与系统** | `FileSystem` 读写拷删搜；`Clipboard`；`Registry`；`PowerShell`；`Scrape` 网页 |
| **浏览器 DOM 模式** | Snapshot `use_dom=True` 只盯页面内容，过滤浏览器工具栏/标签；Chrome / Edge / Firefox |
| **人机控制权** | `ControlStatus`；物理鼠标大幅移动或 `Ctrl+Alt+Shift+Backspace` 可把控制权交回用户；桌面 gate 策略 `relaxed` / `balanced` 等 |
| **传输与安全** | 默认 stdio；可选 SSE / streamable-HTTP；TLS、OAuth PKCE、CORS、IP allowlist；`~/.windows-mcp/config.toml` |

## 工作原理（简图）

```text
AI 层（Agent / Cursor / Claude Desktop）——只发意图
        │ MCP（Model Context Protocol / JSON-RPC）
        ▼
Windows-MCP —— 翻译成系统动作描述
        │
        ▼
Windows 层 —— 原生 UI Automation / 键鼠 / 文件 / 应用
```

## 快速开始

**前置**：Windows 10/11、Python 3.14+、[uv](https://github.com/astral-sh/uv)（`pip install uv`）。

```shell
# 推荐：PyPI 临时运行
uvx windows-mcp serve

# 可选网络传输
uvx windows-mcp serve --transport sse --host localhost --port 8000
uvx windows-mcp serve --transport streamable-http --host localhost --port 8000

# 登录时自动起（计划任务 windows-mcp-server）
windows-mcp install
windows-mcp uninstall   # 移除
```

首次启动会装依赖，可能超时——忽略后重启即可。

**Claude Desktop（`claude_desktop_config.json` 摘要）**

```json
{
  "mcpServers": {
    "windows-mcp": {
      "command": "uvx",
      "args": ["windows-mcp", "serve"]
    }
  }
}
```

微软商店版 Claude（MSIX）需改虚拟化路径下的配置，并用 `uvx.exe` / `windows-mcp.exe` 的**绝对路径**（沙箱不继承系统 PATH）。

**减险常用开关**

```shell
windows-mcp serve --exclude-tools "PowerShell,Registry"
windows-mcp serve --tools "Screenshot,Click,Snapshot"
```

## 适用场景与风险

**场景**：批量整理文件/填表等办公自动化；按自然语言做 UI/QA 点测；补传统 RPA「界面一变就崩」；给自研 Agent 接 OS 级动手能力。

**风险（选用前必读）**

- 跑在当前用户会话权限下，能删文件、改注册表、跑 PowerShell——误操作是真实损失。
- 不可逆操作建议先「只规划不执行」确认，敏感步骤人工把关。
- 仅 Windows 10/11 + Python 3.14+；非跨平台 Computer Use 方案。
- 人机接管是 best-effort，不是安全边界；远程暴露务必鉴权+白名单。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/CursorTouch/Windows-MCP |
| **PyPI（MCP）** | https://pypi.org/project/windows-mcp/ |
| **PyPI（Agent windows-use）** | https://pypi.org/project/windows-use/ |
| **MCP Registry** | https://github.com/modelcontextprotocol/registry |
| **Claude Desktop 扩展目录** | https://claude.ai/directory |
| **来源介绍文（头条）** | https://www.toutiao.com/article/7703356642330460698/ |

## 备注

- 建仓约 2025-05；介绍文称 Claude Desktop 扩展市场装机量破 200 万（以官方 README Updates 为准，会随时间变化）。
- 与「Computer Use」类云端/截图方案对比时：本仓库卖点是 **MCP 标准化 + Windows UIA + 任意 LLM**，不是单一厂商的视觉模型锁死。