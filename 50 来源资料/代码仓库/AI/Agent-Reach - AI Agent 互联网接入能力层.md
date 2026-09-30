---
id: source-20260924-agent-reach
title: Agent-Reach - AI Agent 互联网接入能力层
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/CLI
  - 主题/MCP
  - 主题/AI编程Agent
author: Panniantong（Pnant）
source_type: repo
source_url: https://github.com/Panniantong/Agent-Reach
source_author: Panniantong
source_date: 2026-09-24
summary: 给 AI Agent 一键装上互联网读写能力的能力层 CLI：选型/安装/体检/多后端路由覆盖网页、YouTube、GitHub、B站、Twitter、小红书等，零 API 费用，兼容 Claude Code / Cursor / OpenClaw。
related:
  - "[[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# Agent-Reach - AI Agent 互联网接入能力层

## Source summary

**Agent Reach**（仓库名 `Agent-Reach`）是一个面向 AI Agent 的**互联网接入能力层**：不自己做抓取内核，而是把各平台「当下最稳」的开源工具选好、装好、体检好，并用「首选 + 备选」后端列表做路由与自动修复。

- GitHub：[Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach)
- 约 **85,000+ stars**（2026-09-24 API）；MIT；语言 **Python**
- 一句话安装（扔给 Claude Code / Cursor / OpenClaw 等即可）：

```
帮我安装 Agent Reach：https://raw.githubusercontent.com/Panniantong/agent-reach/main/docs/install.md
```

- 更新同样一句话：`docs/update.md`
- AtomGit 镜像：[atomgit.com/.../Agent-Reach](https://atomgit.com/qq_51337814/Agent-Reach)

来源线索：今日头条文章 [GitHub 7.7 万星项目；让你的 AI Agent 轻松访问各大平台](https://www.toutiao.com/article/7681142810611106347/)（文章写作时约 7.7 万星，仓库持续增长）。

## 解决什么问题

Agent 写代码很强，但上网查资料常卡死：Reddit 403、YouTube 无字幕、Twitter API 付费、小红书要登录、B 站下载被风控等。各平台门槛不同，逐个踩坑且换机器要重来。

Agent Reach 的定位：**能力层，不是又一个抓取工具**——负责选型、安装、体检、路由；真正读写由 Agent 直接调上游工具（无中间包装层）。

## 核心能力

| 能力 | 说明 |
|------|------|
| **一句话安装** | Agent 自读 `install.md`，装 CLI、查基建、按需解锁登录态平台 |
| **零 API 费用** | 工具与接口尽量免费开源；服务器代理约 1 美元/月，本地通常不需要 |
| **多后端路由** | 每平台「首选 + 备选」；坏了自动换路并给修复处方（如 B 站 yt-dlp 412 → bili-cli） |
| **doctor 体检** | `agent-reach doctor` 列出渠道通断与当前后端 |
| **SKILL.md 注册** | Agent 读 skill 后自知调什么命令，用户无需背 CLI |
| **默认安全** | 默认只检查不改系统；`--system` 才装依赖/写配置；`--dry-run` 可预览 |
| **凭据本地** | Cookie/Token 仅存 `~/.agent-reach/config.yaml`（权限 600），不上传 |

## 支持平台（摘要）

| 平台 | 装好即用 | 配置后解锁 |
|------|---------|-----------|
| 网页 / YouTube / RSS / V2EX | 读/搜/订阅等 | — |
| GitHub | 公开仓读+搜 | 私有仓、Issue/PR、Fork（`gh auth`） |
| 全网语义搜索 | — | Exa via MCP（免 Key） |
| B站 | 搜索+详情（bili-cli） | 字幕等（OpenCLI） |
| Twitter/X | 读单条 | 搜索/时间线（Cookie 等） |
| Reddit / Facebook / Instagram / 小红书 | — | 需登录态（OpenCLI / Cookie，建议小号） |
| LinkedIn / Boss直聘 / 雪球 / 小宇宙 | 部分公开读或体检 | 详情/搜索/转录等 |

完整表与「当前选型」见仓库 README。

## 快速开始

```
帮我安装 Agent Reach：https://raw.githubusercontent.com/Panniantong/agent-reach/main/docs/install.md
```

装完示例用法（对 Agent 自然语言即可）：

- 「帮我看看这个链接」→ Jina Reader
- 「这个 GitHub 仓库是干嘛的」→ `gh`
- 「这个 YouTube 讲了什么」→ yt-dlp 字幕
- 「B 站搜一下 AI 教程」→ bili-cli
- 「帮我配 Twitter / 小红书」→ 引导登录态配置

> OpenClaw 需先开 exec：`openclaw config set tools.profile "coding"`，否则装不了依赖。

> 勿从 PyPI 误装同名无关包；应按仓库文档安装本项目的 `agent-reach` CLI。

## 设计要点

`channels/` 下每平台一个文件：有序探测候选后端，首个可用即用；`doctor` 报告当前路径。作者自述本人每天在用，会持续跟平台反爬与渠道换代——「接入方式会换代，你不用操心」。

## 安全注意

- 用 Cookie/脚本的平台（Twitter、小红书等）有封号风险 → **务必小号**
- Twitter Cookie 等需用户手工导出（如 Cookie-Editor），项目不代登、不偷读浏览器 Cookie（OpenCLI 仅复用用户已有会话）
- 卸载：`agent-reach uninstall`（可 `--dry-run` / `--keep-config`）

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/Panniantong/Agent-Reach |
| **安装文档** | https://raw.githubusercontent.com/Panniantong/agent-reach/main/docs/install.md |
| **更新文档** | https://raw.githubusercontent.com/Panniantong/agent-reach/main/docs/update.md |
| **英文 README** | https://github.com/Panniantong/Agent-Reach/blob/main/docs/README_en.md |
| **AtomGit 镜像** | https://atomgit.com/qq_51337814/Agent-Reach |
| **头条介绍文** | https://www.toutiao.com/article/7681142810611106347/ |
| **作者 X** | https://x.com/Neo_Reidlab |
| **联系邮箱** | pnt01@foxmail.com |

## My takeaways

1. **缺口清晰**：Agent 缺的是稳定「上网眼」，不是又一个 LLM 框架；用能力层统一选型比手搓各平台 CLI 更可维护。
2. **运维比创意重要**：多后端 + doctor + 作者自用承诺，比炫酷 demo 更适合当基建。
3. **与 OpenClaw/Cursor 互补**：OpenClaw 管 IM 入口与 skill 生态；Agent Reach 管外网读写；二者可叠加。
4. **合规使用**：登录态渠道务必小号；生产环境注意平台 ToS 与风控。

## Related

- [[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]