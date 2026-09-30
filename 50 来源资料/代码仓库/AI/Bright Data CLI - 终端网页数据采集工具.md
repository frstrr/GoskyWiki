---
id: source-20260901-brightdata-cli
title: Bright Data CLI - 终端网页数据采集工具
type: source
status: active
created: 2026-09-01
updated: 2026-09-01
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/CLI
  - 主题/数据采集
  - 主题/爬虫
  - 主题/Agent
  - 主题/MCP
author: Bright Data
source_type: repo
source_url: https://github.com/brightdata/cli
source_author: brightdata
source_date: 2026-09-01
summary: Bright Data 官方 CLI（brightdata / bdata），在终端直接抓取、搜索、抽取结构化公开网页数据；内置反爬/验证码/JS 渲染处理，可导出 Markdown/CSV/JSON，并支持 Skill / MCP 接入 WorkBuddy、Cursor 等 Agent。
related:
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
---

# Bright Data CLI - 终端网页数据采集工具

## Source summary

**Bright Data CLI**（npm：`@brightdata/cli`）是 Bright Data 官方终端工具，把网页抓取、搜索引擎检索、结构化抽取与远程浏览器操控封装成一行命令，无需自写复杂 Python/Playwright 反爬脚本。

- 开源项目，GitHub：[brightdata/cli](https://github.com/brightdata/cli)
- 约 **6,394 stars**（2026-09-01）；语言：**TypeScript**；许可证：**MIT**
- 命令入口：`brightdata`（别名 `bdata`）
- 官方文档：[docs.brightdata.com/cli/overview](https://docs.brightdata.com/cli/overview)
- 安装脚本：https://cli.brightdata.com/install.sh
- AI 额度注册（文中提及免费额度入口）：https://get.brightdata.com/dataforai
- 来源介绍文章（头条）：https://www.toutiao.com/article/7680142890932126248/

底层走 Bright Data 采集基础设施：自动处理人机验证、动态页面渲染、浏览器指纹与代理轮换，按 URL / 字段需求返回数据集，适合公开网页数据采集、电商监测、搜索结果批采，以及为大模型准备结构化语料。

> 注意：仅用于合法公开数据采集；需遵守目标站点条款与当地法规，并自行管理 API Key 与额度。

## 解决什么问题

| 痛点 | Bright Data CLI 方案 |
|------|----------------------|
| 自写 requests / Playwright 反爬成本高 | API + CLI 统一接管验证码、指纹、JS 渲染 |
| Agent 难稳定拿网页数据 | Skill / MCP 接入，口语即可触发采集 |
| 搜索/多平台抽取脚本碎片化 | `search` / `pipelines` 覆盖 Google/Bing/Yandex 与 40+ 平台 |
| 输出不便给 AI 消费 | 直接导出 Markdown / CSV / JSON / HTML 报告 |

## 核心能力

| 命令 / 能力 | 说明 |
|-------------|------|
| `brightdata scrape` | 抓取任意 URL，绕过验证码/反爬，支持 Markdown/HTML/JSON/截图 |
| `brightdata search` | Google / Bing / Yandex 搜索，结构化 JSON（含自然结果、广告等） |
| `brightdata discover` | AI 意图驱动的网页发现与相关性排序 |
| `brightdata scraper create/run/heal` | 自然语言生成爬虫、运行，以及 AI 自愈修复（含审批门） |
| `brightdata pipelines` | 从 Amazon、LinkedIn、TikTok 等 40+ 平台抽结构化数据 |
| `brightdata browser` | 远程真实浏览器：导航、点击、输入、截图、快照 |
| `brightdata skill` / `add mcp` | 安装 Agent Skill，或把 MCP 加到 Claude Code / Cursor / Codex |
| `brightdata zones` / `budget` | 查看代理 zone、余额与带宽成本 |
| `brightdata init` / `config` / `login` | 交互式初始化、配置与鉴权 |

## 安装与鉴权（速查）

要求：**Node.js ≥ 20**。

```bash
# macOS / Linux
curl -fsSL https://cli.brightdata.com/install.sh | sh

# Windows / 任意平台
npm install -g @brightdata/cli

# 初始化
brightdata init

# 登录（任选其一）
brightdata login --api-key <your-api-key>
export BRIGHTDATA_API_KEY=your-api-key
```

免费档（官方说明）：新账号每月约 **5,000 credits**（约 $7.50 等值），月初刷新，不结转；适合试用与小规模采集。

## 快速示例

```bash
brightdata scrape https://example.com
brightdata search "web scraping best practices"
brightdata pipelines linkedin_person_profile "https://linkedin.com/in/username"
brightdata budget
brightdata add mcp
```

## 与 Agent / WorkBuddy 搭配

文中推荐路径：

1. 本机安装并 `brightdata login` 配置 API Key
2. `brightdata skill add` 导出 SKILL 文件
3. 在 WorkBuddy（或 Codex / Trae 等）「技能 → 添加技能」上传 SKILL.md
4. 用自然语言让 Agent 调 CLI，例如：搜 Google「agent」相关新闻、采亚马逊销量前列手机的价格/名称/图片等

也可通过 `brightdata add mcp` 把 Bright Data MCP 接到 Cursor / Claude Code / Codex。

WorkBuddy 相关兼容说明可参考：[[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]。

## 适用场景

- 需要稳定抓取带反爬的公开网页，又不想维护 Playwright 反爬脚本
- 批量搜索引擎结果、电商商品字段、LinkedIn 等公开结构化数据
- 给 AI Agent（WorkBuddy / Cursor / Claude Code）补「能上网拿数据」的能力
- 为大模型训练/分析准备 Markdown、CSV、JSON 等多格式公开语料

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/brightdata/cli |
| **官方文档** | https://docs.brightdata.com/cli/overview |
| **npm 包** | https://www.npmjs.com/package/@brightdata/cli |
| **安装脚本** | https://cli.brightdata.com/install.sh |
| **API Key / 免费额度入口** | https://get.brightdata.com/dataforai |
| **产品博客** | https://brightdata.com/blog/ai/bright-data-cli |
| **组织主页** | https://github.com/brightdata |
| **来源文章（头条）** | https://www.toutiao.com/article/7680142890932126248/ |