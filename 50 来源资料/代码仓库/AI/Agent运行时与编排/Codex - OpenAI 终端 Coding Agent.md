---
id: source-20261019-openai-codex
title: Codex - OpenAI 终端 Coding Agent
type: source
status: active
created: 2026-10-19
updated: 2026-10-19
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/Agent运行时
  - 主题/CLI
  - 主题/AI编程Agent
  - 主题/Codex
  - 主题/开源资源
author: OpenAI
source_type: repo
source_url: https://github.com/openai/codex
source_author: openai
source_date: 2026-10-19
summary: OpenAI 开源的本地终端 Coding Agent（Codex CLI）。可在仓库内探查、改代码、跑命令；支持交互会话、非交互 exec/CI、代码审查、Skills/Plugins、MCP、子 Agent、沙箱权限与云端交接。主语言 Rust，Apache-2.0。
related:
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenAgentCore - OpenAI Agents API 自托管运行时|OpenAgentCore - OpenAI Agents API 自托管运行时]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/codex-with-chatgpt - ChatGPT 规划大脑与 Codex 执行桥|codex-with-chatgpt - ChatGPT 规划大脑与 Codex 执行桥]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/Reasonix - DeepSeek 缓存优先编码 Agent|Reasonix - DeepSeek 缓存优先编码 Agent]]"
---

# Codex - OpenAI 终端 Coding Agent

## Source summary

**Codex**（仓库 [openai/codex](https://github.com/openai/codex)）是 OpenAI 开源的 **本地终端 Coding Agent**：在本机仓库里探查代码、编辑文件、运行开发工具，并把可重复工作流自动化。GitHub 描述为 *Lightweight coding agent that runs in your terminal*。

- 开源项目，GitHub：[openai/codex](https://github.com/openai/codex)
- 约 **129,863 stars** / **20,451 forks**（截至 2026-10-19 GitHub API）
- 主语言：**Rust**（另有 Python / TypeScript / Shell 等）；许可证：**Apache-2.0**
- 官方文档：[developers.openai.com/codex](https://developers.openai.com/codex) · CLI 专页：[Codex CLI](https://developers.openai.com/codex/cli)
- npm 包：`@openai/codex` · 发行页：[GitHub Releases](https://github.com/openai/codex/releases) / `releases.openai.com/codex`
- 形态区分：本仓库主打 **Codex CLI**；另有 IDE 扩展、桌面 App（`codex app`）、以及云端 [Codex Web](https://chatgpt.com/codex)（非本仓库本体）

## 解决什么问题

| 痛点 | Codex 方案 |
|------|------------|
| 想在终端里完成「读代码 → 改 → 跑测试」闭环，不愿反复切 IDE 聊天窗 | 交互式 TUI：同一会话内探查、编辑、执行、跟进 |
| CI / 脚本需要可重复的 Agent 调用 | `codex exec` 非交互模式，可输出文本或 JSONL |
| 提交/开 PR 前缺少独立审查 | `codex review`：审查未提交改动、某次 commit 或相对 base branch 的 diff，不改工作树 |
| 需要权限边界与本机安全执行 | `/permissions` + OS 沙箱；可检查可写根目录 |
| 要把团队工具接到 Agent | Skills / Plugins 市场 + `codex mcp` 管理 MCP 服务器 |
| 本机与云端协作 | `codex cloud` 提交/浏览云端任务，`codex apply` 把云端 diff 拉回本地 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **终端编码闭环** | 探查仓库、改文件、跑本机工具；可中途跟进/打断当前 turn |
| **会话管理** | `codex resume` / `fork` / `archive`；按仓库恢复近期聊天 |
| **非交互执行** | `codex exec`（别名 `e`），适合脚本与 CI |
| **代码审查** | `codex review`：PR 风格 / 未提交 / 指定 commit / 自定义指令 |
| **Skills 与 Plugins** | 可复用指令打包为 skills；插件连接团队工具与数据源 |
| **MCP** | `codex mcp`：增删、列表、登录 MCP；会话内可见可用工具 |
| **子 Agent** | 把大调查拆给 specialized agents，结果回收主会话 |
| **多模态与检索** | `--image` 带图；`--search` 开启实时网页搜索 |
| **云端交接** | `codex cloud` / `codex apply` 本机 ↔ 云端环境 |
| **诊断与更新** | `codex doctor`、`codex update`、shell completion |

## 安装与上手

**macOS / Linux（官方安装脚本）：**

```shell
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

**Windows（PowerShell）：**

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

**包管理器：**

```shell
npm install -g @openai/codex
brew install --cask codex
```

安装后进入项目目录运行 `codex`，首次选择 **Sign in with ChatGPT**（Plus / Pro / Business / Edu / Enterprise 计划可用），也可按文档配置 API Key。

建议首轮：`/init` 生成 `AGENTS.md`，再用 `/permissions`、`/model` 设定权限与模型。

## 关键命令速查

| 命令 | 用途 |
|------|------|
| `codex` | 启动交互 TUI |
| `codex exec` | 非交互执行 |
| `codex review` | 代码审查 |
| `codex resume` | 恢复会话 |
| `codex mcp` | 管理 MCP |
| `codex plugin` | 安装/管理插件 |
| `codex cloud` / `codex apply` | 云端任务与应用 diff |
| `codex doctor` | 本地诊断报告 |
| `codex sandbox` | 在 Codex 沙箱中跑任意命令 |

完整参考：[CLI Reference](https://developers.openai.com/codex/cli/reference)

## 有用链接

| 类型 | 链接 |
|------|------|
| 仓库 | https://github.com/openai/codex |
| 文档总入口 | https://developers.openai.com/codex |
| CLI 文档 | https://developers.openai.com/codex/cli |
| ChatGPT 计划说明 | https://help.openai.com/en/articles/11369540-codex-in-chatgpt |
| Codex Web | https://chatgpt.com/codex |
| Releases | https://github.com/openai/codex/releases |
| 贡献 / 构建 | 仓库内 `docs/contributing.md`、`docs/install.md` |

## 何时选用

- 需要 **OpenAI 官方**、本地优先的终端 Coding Agent，并与 ChatGPT 订阅打通
- 要同时覆盖 **交互开发 + CI 非交互 + 本地 Review**
- 生态上要接 **MCP / Skills / Plugins**，或与 OpenAgentCore 等自托管 Agents API 侧的 Codex Harness 对照

相关笔记：导航见 [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]；生态桥接见 [[50 来源资料/代码仓库/AI/Agent工具与中间件/codex-with-chatgpt - ChatGPT 规划大脑与 Codex 执行桥|codex-with-chatgpt]]；自托管挂载见 [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenAgentCore - OpenAI Agents API 自托管运行时|OpenAgentCore]]。