---
id: source-20261103-archify
title: Archify - 交互式架构图 Agent Skill
type: source
status: active
created: 2026-11-03
updated: 2026-11-03
tags:
  - 来源/代码仓库
  - 主题/AI编程Agent
  - 主题/Agent Skills
  - 主题/架构可视化
  - 主题/系统图
author: 也无风雨也雾晴 (tt-a1i)
source_type: repo
source_url: https://github.com/tt-a1i/archify
source_author: tt-a1i
source_date: 2026-04-15
summary: 面向 Claude Code、Codex、Cursor、OpenCode 等编程 Agent 的 Skill：把想法、计划或代码仓库变成可交互 HTML 架构图/工作流图/时序图等，并做 Schema 校验与路径追踪；也可生成结论先行的一页式 explainer。
aliases:
  - Archify
  - tt-a1i/archify
related:
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]"
moc:
  - "[[40 知识导航/AI 编程 Agent]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/_分类说明|Skills与方法论]]"
---

# Archify - 交互式架构图 Agent Skill

## Source summary

[Archify](https://github.com/tt-a1i/archify)（`tt-a1i/archify`）是面向编程 Agent 的开源 **Agent Skill**：把一句话描述、系统计划或代码仓库，转成可搜索节点、追踪调用路径、切换主题的**独立 HTML** 交互图。

- **Stars / Forks**：约 **9.3 万 Stars / 6.3k Forks**（截至 2026-11-03，GitHub API）。
- **协议**：Apache-2.0。
- **主语言**：JavaScript。
- **官网**：[https://archify.si](https://archify.si)（中文：[https://archify.si/zh](https://archify.si/zh)）。
- **兼容 Agent**：Claude Code、Codex、Cursor、OpenCode 等；skills.sh 显示安装量 130K+。
- **定位差异**：不是单纯的 Mermaid/D2「文本→图」渲染器，而是把理解代码、生成结构化 JSON IR、规则校验、HTML 渲染嵌入 Agent 工作流；不满意可继续用自然语言改图。

## Key excerpts

### 能生成什么

常见输出类型包括：

- 架构图（architecture）
- 工作流图（workflow）
- 时序图（sequence）
- 数据流图
- 生命周期图
- **Explainer 一页答**：结论先行，再按问题分面板（表格/步骤/图/代码），适合排查与讲解

成品是**自包含 HTML**：下载后离线也能点节点、看路径；发文件即带走交互。可导出 PNG / SVG / WebM、路径分享卡片等。

### 工作流（简述）

1. 给 Agent 一段系统描述，或让它分析代码仓库。
2. Archify 生成结构化 **JSON IR**。
3. 做 Schema / 布局 / 连线冲突等校验；失败会指出问题与可修复方式，而不是硬交一张错图。
4. 渲染为交互 HTML（可选浅/深主题）。
5. 可用自然语言增量修改（例如「加入 Redis」「把认证移到左边」「突出缓存回退路径」）。
6. 支持比较两个架构快照（Before / Delta / After）。

### 快速安装（让 Agent 自己装）

```text
Install Archify for me by reading https://raw.githubusercontent.com/tt-a1i/archify/main/llms.txt
```

示例提示：

```text
用 Archify 画出一个网站的请求流程：浏览器请求 API，
API 优先读取 Redis，缓存未命中时查询 PostgreSQL 并回填缓存。
```

更多按 Agent 的安装入口：[https://archify.si/zh/start](https://archify.si/zh/start)

### 适用场景

- 用 AI 编程时快速理解陌生仓库与调用链
- 给团队讲清复杂系统 / 评审架构变更
- 把问题整理成可分享的一页 explainer（如 P99 抖动排查）
- 社区扩展：行程规划、协作讨论等非纯架构场景（输出仍是 HTML）

### 局限（官方与报道侧提示）

- 不是实时监控，也不能自动证明图与线上完全一致；准确性依赖代码信息、提示与 Agent 分析能力。
- 有用户反馈：节点详情偏小、弱视觉模型不够顺、Token 消耗偏大等（开发者在迭代）。

## 相关链接

| 类型 | 链接 |
|------|------|
| 仓库 | https://github.com/tt-a1i/archify |
| 官网 | https://archify.si |
| 交互演示 / Gallery | https://archify.si/zh/gallery |
| 场景指南 | https://archify.si/zh/guide |
| skills.sh | https://skills.sh/tt-a1i/archify/archify |
| Discord | https://discord.gg/6xWMjgCeUq |
| 作者 X | https://x.com/t20000622yy |
| Trendshift | https://trendshift.io/repositories/31352 |
| 量子位项目报道 | https://www.qbitai.com/2026/09/482469.html |
| 量子位开发者故事 | https://www.qbitai.com/2026/09/488519.html |
| 本次头条转载入口 | https://www.toutiao.com/article/7680480100529226286/ |

## My takeaways

1. **想找「架构图 / 系统地图」类开源 Agent Skill 时优先想到 Archify**：和 Mermaid 工具链互补——后者偏语法渲染，前者偏 Agent 理解 + 校验 + 交互交付。
2. **落地成本低**：一句 `llms.txt` 安装提示即可接到 Cursor / Claude Code / Codex / OpenCode。
3. **交付物可分享**：独立 HTML 适合丢进评审、文档或群聊，不必人人装 Skill。
4. **爆红背景**：曾冲上 GitHub Trending（报道称周榜第一量级），社区与量子位有较完整的项目与作者故事，便于二次检索。
5. **与本库 Skills 分类并列**：方法论类看 Superpowers / mattpocock；**可视化理解仓库**看 Archify。

## Related

- [[50 来源资料/代码仓库/AI/Skills与方法论/_分类说明|Skills与方法论]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
