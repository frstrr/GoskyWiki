---
id: source-20261019-reasonix
title: Reasonix - DeepSeek 缓存优先编码 Agent
type: source
status: active
created: 2026-10-19
updated: 2026-10-19
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/AI编程Agent
  - 主题/CLI
  - 主题/PromptCaching
  - 主题/DeepSeek
  - 主题/MCP
author: Huahui Yu / esengine
source_type: repo
source_url: https://github.com/esengine/DeepSeek-Reasonix
source_author: esengine
source_date: 2026-10-19
summary: 开源 AI 编码 Agent（MIT），围绕 DeepSeek 前缀缓存做三层上下文隔离，实测缓存命中率可达 99.82%；v1.x 用 Go 重写，支持 CLI/桌面/VS Code，并兼容 20+ 模型与 MCP。
related:
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[30 知识资源/AI/Claude API Prompt Caching 失效根因与优化策略|Claude API Prompt Caching 失效根因与优化策略]]"
  - "[[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]"
aliases:
  - DeepSeek-Reasonix
  - Reasonix Studio
---

# Reasonix - DeepSeek 缓存优先编码 Agent

## Source summary

**Reasonix**（仓库名 `DeepSeek-Reasonix`）是面向终端 / 桌面 / 浏览器 / 编辑器的开源 **AI 编码 Agent**：本地运行、自选模型、可读写项目文件并执行命令与测试；架构从第一天就围绕 **DeepSeek 前缀缓存（prefix-cache）稳定性** 设计，把长会话输入成本压到极低。

- GitHub：[esengine/DeepSeek-Reasonix](https://github.com/esengine/DeepSeek-Reasonix)
- 官网：[reasonix.io](http://reasonix.io/) · 文档/下载页：[esengine.github.io/DeepSeek-Reasonix](https://esengine.github.io/DeepSeek-Reasonix/)
- 约 **35,847 stars / 2,463 forks**（2026-10-19 GitHub API）；许可证 **MIT**；主语言 **Go**（v1.x 全量重写；历史线为 TypeScript）
- 默认分支：`studio`（2.x / Studio 活跃线）；稳定 1.x 维护线见仓库版本说明（文章口径为 `main-v2`）
- Topics：`coding-agent`、`deepseek`、`prompt-caching`、`cli`、`tui`、`ai-agent` 等
- 维护者：Huahui Yu / esengine

来源线索：今日头条 [GitHub 35.8K+ Star！缓存命中率 99.82%，这款开源 AI 编码工具火了](https://www.toutiao.com/article/7697447213282247209/)（文中对比：同等约 4.35 亿输入 Token，无缓存优化约 61 美元 vs Reasonix 约 12 美元）。

## 解决什么问题

DeepSeek API 自带前缀缓存，命中后输入价可低至约 1/10；但 Aider、Claude Code、通用客户端长会话里，动态系统提示、工具定义乱序、历史截断重写等会破坏固定前缀，命中率常年很低，账单爆炸。

| 痛点 | Reasonix 方案 |
|------|----------------|
| 前缀一变缓存清零 | 三层上下文隔离：不可变前缀 / 仅追加日志 / 易失暂存区不进 API |
| 只用贵模型浪费 | Flash 优先 + 失败/复杂任务自动升 Pro；`/effort`、`/pro` 精细控费 |
| 上下文膨胀 | 大体量工具返回自动压缩摘要，按需再读文件 |
| 多端分裂 | CLI、桌面 Studio、VS Code 扩展共用同一本地引擎与配置 |
| 旧版只认 DeepSeek | v1.x 起支持 20+ 国产/海外模型及 OpenAI/Anthropic 兼容端点（DeepSeek 仍为一等公民，缓存优化最深） |

## 核心能力

| 能力 | 说明 |
|------|------|
| **读改跑** | 读代码、改文件、跑命令与测试；Plan / Review / Auto / YOLO 四级权限 |
| **缓存优先循环** | 系统提示 + 工具定义 + `REASONIX.md` 固定；历史只追加；推理草稿不污染前缀 |
| **模型调度** | 默认 deepseek-v4-flash；连续失败或复杂任务可升 v4-pro；`/effort low|high|max` |
| **记忆分层** | 项目 `REASONIX.md`、用户 `~/.reasonix/memories/`、全局规则、会话临时记忆 |
| **MCP** | stdio / Streamable HTTP；可读项目 `.mcp.json`；内置市场与斜杠命令 |
| **进阶（v1.x）** | Auto Research、心跳定时任务、QQ/飞书/Lark/微信机器人、规划器+执行器双模型、沙盒黑白名单 |
| **可回退** | 编辑工具改动的文件可按对话轮次回退（不覆盖 shell 副作用） |

## 架构要点（缓存三层）

```text
┌─ 不可变前缀（全会话固定）─┐  系统提示 + 工具定义 + REASONIX.md
├─ 仅追加日志 ─────────────┤  历史只往后加，不删不重排旧消息
└─ 易失暂存区（不传 API）──┘  推理过程、临时计划，杜绝污染前缀
```

配套：工具调用修复流水线、历史只从尾部截断、推理内容按 DeepSeek 规范处理。

## 版本线速览

| 维度 | 旧 0.53.x（TS） | v1.x（Go，文章口径） |
|------|-----------------|----------------------|
| 运行时 | Node ≥ 22 | 静态单二进制 |
| 模型 | 几乎仅 DeepSeek | 20+ 模型 + 兼容协议 |
| 配置 | config.json | reasonix.toml + `.env` |
| 安装 | npm / 旧桌面包 | npm、brew、桌面包、VS Code 扩展 |

注意：仓库当前还有 **2.x Studio**（`studio` 分支）与 **1.x 维护线** 的产品划分，落地前以官方 README / Releases 为准；0.x 桌面「检查更新」可能失效，需手动装新包并用 `/migrate`。

## 安装与快速体验

```bash
# macOS
brew install esengine/reasonix/reasonix

# 通用
npm i -g reasonix
reasonix --version
reasonix setup
# 在项目目录
reasonix chat
# 会话内 /init 生成 REASONIX.md（可兼容 CLAUDE.md 思路）
```

- 桌面端：https://esengine.github.io/DeepSeek-Reasonix/
- VS Code 扩展（文中）：搜索 `SivanLiu.reasonix-agent`（经 ACP 连本地后端）

## 适用 / 不适用

**更适合**：API 用量大、预算敏感；DeepSeek 重度用户；需要 CLI+桌面+扩展统一入口；要 MCP、定时任务、IM 远程审批。

**不太适合当主力**：极致数学/博士级推理（更强闭源模型上限更高）；只想要 Cursor 级 IDE 内联补全；完全不碰国产模型生态。

## 链接

| 资源 | 链接 |
|------|------|
| GitHub | https://github.com/esengine/DeepSeek-Reasonix |
| 官网 | http://reasonix.io/ |
| 文档/下载 | https://esengine.github.io/DeepSeek-Reasonix/ |
| DeepSeek API Key | https://platform.deepseek.com/api_keys |
| 来源文章（头条） | https://www.toutiao.com/article/7697447213282247209/ |
