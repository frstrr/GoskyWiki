---
id: source-20261015-ego-lite
title: ego-lite - AI Agent 人机并行浏览器
type: source
status: active
created: 2026-10-15
updated: 2026-10-15
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/浏览器自动化
  - 主题/开源资源
  - 主题/Chromium
author: CitroLabs
source_type: repo
source_url: https://github.com/citrolabs/ego-lite
source_author: citrolabs
source_date: 2026-10-15
summary: 面向 AI Agent 的 Chromium 专用浏览器（MIT）。人机共享同一内核、任务在独立 Space 并行；内核级语义快照 + JS 批量编排降 Token；一键迁移 Chrome 登录态。约 17.2k Stars；当前原生支持 macOS，Windows/Linux 在路线图。
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill - AI Agent 登录态浏览器操控]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach - AI Agent 互联网接入能力层]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/Bright Data CLI - 终端网页数据采集工具|Bright Data CLI - 终端网页数据采集工具]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
article_source: https://www.toutiao.com/article/7696003001995002422/
---

# ego-lite - AI Agent 人机并行浏览器

## Source summary

**ego (lite)**（仓库 [citrolabs/ego-lite](https://github.com/citrolabs/ego-lite)）是 **CitroLabs** 开源的 **面向 AI Agent 的 Chromium 专用浏览器**：人和 Agent 共用同一套浏览器内核，Agent 在独立 **Space** 跑自动化，你继续在前台浏览，互不抢焦点；通过 **ego-browser** Skill 对接 Claude Code / Codex / Cursor / OpenClaw 等任意能跑 shell 的本地 Agent。

- 约 **17.2k+ Stars**（头条文称 17.2K+；GitHub 检索约 **17391**，2026-10）
- **MIT** 协议，客户端免费、无功能阉割
- 官网下载：[lite.ego.app](https://lite.ego.app/)
- 平台：**macOS** 原生可用（Apple Silicon / Intel）；**Windows** 内测即将发布，**Linux** 在官方路线图
- 一句话定位：**不是挂在 Chrome 上的自动化桥，而是为人 + Agent 并行设计的浏览器本体**

与同类对比：[[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill]] 是扩展 + CLI 接管你已有 Chrome/Edge；**ego lite 自带改造过的 Chromium**，强调 Space 隔离、语义快照与批量 JS 降 Token。Browser-Use / Vercel agent-browser 多为「驱动外部浏览器的框架/CLI」，不自带人机共享内核。

## 解决什么问题

| 痛点 | ego lite 方案 |
|------|----------------|
| Agent 接管 Playwright/Browser-Use 后抢窗口焦点 | 人在前台 Space，Agent 在独立 Space，前台后台分离 |
| 每次新开浏览器实例，丢 Cookie、要重登 | 一键迁移 Chrome 书签/扩展/Cookie/登录态 |
| 完整 HTML 灌给 LLM，Token 爆炸 | 内核级语义 Snapshot（含 iframe / Shadow DOM） |
| 单步 tool call 往返慢 | Agent 写多步 JS，一次批量执行（官方称复杂任务最高约 2.5–3.45×） |
| Atlas / Comet 绑定自家模型 | ego-browser Skill 对接任意本地 Agent，无模型锁定 |
| 数据上云顾虑 | 浏览/账号数据本地存储；仅极简产品统计 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **单内核多 Space** | 共享 Chromium，多任务并行、逻辑隔离，避免多开完整浏览器进程 |
| **人机并行** | 右上角可看任务数，可查看/暂停/接管任意 Space |
| **Chrome 环境一键迁移** | 书签、扩展、密码、Cookie、Profile、标签组 |
| **ego-browser Skill** | 对 Agent 暴露 snapshot / click / fill / wait / navigate / capture 等 JS 能力 |
| **结构化快照省 Token** | 内核提取关键结构，稳定 `@N` 引用，减少 HTML 全量传输 |
| **强页面兼容** | 多层 iframe、Shadow DOM、第三方 SDK 组件（Stripe / Salesforce 等） |
| **全本地隐私** | Cookie/历史/密码不上云；Agent 复用真实登录，利于过验证码/SSO |

## 快速开始

**系统**：当前稳定支持 **macOS**；Windows/Linux 见路线图。

**方式 1：客户端直装（推荐）**

1. 打开 [lite.ego.app](https://lite.ego.app/) 下载对应架构 DMG 并安装
2. 首次启动选择「迁移 Chrome 数据」
3. 引导会把 **ego-browser** Skill 写入本机各 Agent 的 Skill 目录
4. 重启 Claude Code / Codex / Cursor 等后即可 `/ego-browser` 下发任务

**方式 2：只装 Skill（开发者）**

```bash
npx skills add citrolabs/ego-lite
```

Agent 首次跑浏览器任务时会引导下载客户端。

**方式 3：让 Agent 自装**

```text
帮我装好 ego lite：https://github.com/citrolabs/ego-lite
阅读 skills/ego-browser/references/install.md，按步骤安装 ego lite。
```

确认命令：

```bash
command -v ego-browser
ego-browser nodejs <<'EOF'
console.log('ego-browser ready')
EOF
```

## 文章实测与场景（来源：头条 7696003001995002422）

| 场景 | 做法要点 |
|------|----------|
| 抖音创作者后台 7 日数据 → Excel + 趋势图 | `/ego-browser` + 已登录后台，独立 Space 采集，人不被打断 |
| B 站 + 小红书多平台并行 | 多 Space 同时跑，数据隔离 |
| 竞品调研 / 求职筛选 / 参考站抓设计 / 云后台部署 | 复用真实登录态的网页操作闭环 |

## 与竞品速览（文中对比摘要）

| 维度 | ego lite | Browser-Use | agent-browser | Atlas / Comet |
|------|----------|-------------|---------------|---------------|
| 自带浏览器内核 | ✅ | — | — | ✅ |
| 多任务 Space | ✅ | — | — | — |
| 迁移 Chrome 登录态 | ✅ | — | — | ✅（偏内置） |
| 第三方 Agent | ✅ | ✅ | ✅ | —（绑定自家模型） |
| 数据本地 | ✅ | ✅ | ✅ | 偏云端 |
| 语义快照省 Token | ✅ | — | ✅（CLI 侧） | — |
| 人机不冲突 | ✅ | — | — | — |
| 免费 | ✅ MIT | ✅ | ✅ | 订阅 |

选型（文中建议）：本地多 Agent + 批量后台采集 + 隐私 → **ego lite**；简单单次抓取 → Browser-Use；只吃 ChatGPT/Perplexity 生态 → Atlas / Comet。若已深度用本机 Chrome/Edge 扩展调试，可同时看 [[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill]]。

## 常见问题（文中踩坑）

| 现象 | 处理 |
|------|------|
| Agent 不识别 ego-browser | 重启 Agent；确认已完成 Chrome 迁移；重跑 `npx skills add citrolabs/ego-lite` |
| iframe 读不到 | 内核支持多层 iframe；可关广告拦截扩展后重试 |
| 多任务内存升高 | 共享内核仍低于多开 Chrome；关闲置 Space |
| Windows/Linux 装不上 | 等官方版本；关注 GitHub roadmap |
| 被反爬 | 复用真实 Cookie + 自然人浏览行为，通常好于无头脚本 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/citrolabs/ego-lite |
| **官网 / 下载** | https://lite.ego.app/ |
| **ego-browser 文档** | https://lite.ego.app/document/en/docs/ego-browser |
| **安装说明（Skill）** | https://github.com/citrolabs/ego-lite/blob/main/skills/ego-browser/references/install.md |
| **头条介绍文** | https://www.toutiao.com/article/7696003001995002422/ |
| **许可证** | MIT |

## My takeaways

1. **差异点在「浏览器本体」**：不是再包一层 Playwright，而是改造 Chromium + Space，解决人机争用与登录态断裂。
2. **省 Token 靠两件事叠在一起**：内核语义快照 + 多步 JS 一次跑完，复杂任务差距更大。
3. **与 BrowserSkill 互补**：BrowserSkill 挂在现有 Chrome/Edge；ego lite 更适合愿意换成专用 Agent 浏览器、要并行 Space 的人。
4. **平台门槛**：现阶段以 macOS 为主；Windows 用户先关注内测。
5. **权限即风险**：复用真实登录态 = Agent 拥有同等账号能力，应用可信 Agent / 专用号。

## Related

- [[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill - AI Agent 登录态浏览器操控]]
- [[50 来源资料/代码仓库/AI/Agent工具与中间件/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach - AI Agent 互联网接入能力层]]
- [[50 来源资料/代码仓库/AI/Agent工具与中间件/Bright Data CLI - 终端网页数据采集工具|Bright Data CLI - 终端网页数据采集工具]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]