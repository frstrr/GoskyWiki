---
id: source-20260831-memos
title: Memos - 自托管轻量笔记工具
type: source
status: active
created: 2026-08-31
updated: 2026-09-01
tags:
  - 来源/代码仓库
  - 主题/个人知识管理
  - 主题/笔记
author: usememos
source_type: repo
source_url: https://github.com/usememos/memos
source_author: usememos
source_date: 2026-09-01
summary: 开源自托管短笔记工具（Go+React），Markdown 原生、时间线式记录；支持标签/置顶/搜索、可见性控制、附件；零遥测、MIT 许可；Docker 一键部署，提供 Web Clipper、REST/gRPC API 与 Webhooks。最新发布 v0.30.0，约 6.27 万 Stars。
related:
  - "[[30 知识资源/其他/Obsidian 个人知识库方法论|Obsidian 个人知识库方法论]]"
  - "[[30 知识资源/其他/多平台文章同步发布方案|多平台文章同步发布方案]]"
---

# Memos - 自托管轻量笔记工具

## Source summary

**Memos** 是一款**开源、自托管**的短笔记工具，专为快速捕捉想法而设计。日常笔记、链接、工作日志和代码片段以**时间线式 Markdown 流**呈现，部署在你自己的基础设施上，无需全功能工作区的复杂开销。

- GitHub：`usememos/memos`（MIT 许可，约 **6.27 万** Stars / 4.7k Forks）
- 官网：https://usememos.com
- 定位 slogan：*Fast enough for every thought. Private enough for all of them.*
- 技术栈：后端 Go、前端 React；默认 SQLite，亦支持 MySQL / PostgreSQL
- Docker 镜像：`neosmemo/memos:stable`，默认端口 **5230**
- 最新发布：`v0.30.0`（2026-07）

## 核心能力

| 能力 | 说明 |
|------|------|
| **快速捕捉** | Markdown 写作、附件媒体，无需选标题/文件夹/模板 |
| **轻量组织** | 时间线浏览、全文搜索、`#标签` 自动提取、置顶 |
| **可见性控制** | 默认私有；可设 Private / Protected / Public |
| **数据自主** | 自托管、**零遥测**、无广告、MIT 开源 |
| **关联与附件** | 笔记互链、本地或 S3 附件存储 |
| **Web Clipper** | 浏览器扩展，将网页/选区/图片保存为带来源链接的 Markdown |
| **API 集成** | REST、gRPC、Webhooks，可接脚本、Bot 与自定义采集流程 |

## 快速部署

```bash
docker run -d \
  --name memos \
  --restart unless-stopped \
  -p 127.0.0.1:5230:5230 \
  -v ~/.memos:/var/opt/memos \
  neosmemo/memos:stable
```

启动后访问 http://localhost:5230 即可使用。生产环境推荐 Docker Compose；另有 Kubernetes（Helm/清单）与源码构建选项，详见官方部署文档。

## 扩展生态

| 扩展 | 说明 |
|------|------|
| **Web Clipper** | Chrome / Firefox 扩展，一键剪藏网页内容 |
| **REST API** | 标准 REST 接口，便于脚本与自动化 |
| **gRPC** | 高性能 RPC 接口 |
| **Webhooks** | 事件推送，对接外部工作流 |

## 与 Obsidian 的对比定位

| 维度 | Memos | Obsidian |
|------|-------|----------|
| 形态 | 自托管 Web 服务 | 本地桌面/移动客户端 |
| 内容模型 | 时间线短笔记流 | 双向链接知识图谱 |
| 部署 | Docker 一键自托管 | 本地 vault 文件夹 |
| 适用场景 | 快速随手记、工作日志、公开分享 | 深度知识管理与长期沉淀 |

两者可互补：Memos 做**轻量快速捕捉与对外分享**，Obsidian 做**结构化长期知识库**。

## 适用场景

- 需要**自托管**、数据完全掌控的轻量笔记服务
- 日常碎片想法、链接收藏、工作日志的快速记录
- 希望有 Web Clipper 和 API，方便与自动化工具集成
- 不想用 Notion/飞书等 SaaS，但又需要 Web 端随时访问

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/usememos/memos |
| **官网** | https://usememos.com |
| **在线 Demo** | https://demo.usememos.com/ |
| **官方文档** | https://usememos.com/docs |
| **功能概览** | https://usememos.com/features |
| **数据所有权说明** | https://usememos.com/features/data-ownership |
| **部署指南** | https://usememos.com/docs/deploy |
| **API 文档** | https://usememos.com/docs/api |
| **Web Clipper** | https://usememos.com/web-clipper |
| **Chrome 扩展** | https://chromewebstore.google.com/detail/memos-web-clipper/nebaoebnljalfegiidibihhkebeiklbl |
| **Firefox 扩展** | https://addons.mozilla.org/en-US/firefox/addon/memos-web-clipper/ |
| **Webhooks 集成** | https://usememos.com/docs/integrations/webhooks |
| **Discord 社区** | https://discord.gg/tfPJa4UmAv |
| **GitHub Discussions** | https://github.com/usememos/memos/discussions |
| **Docker Hub** | https://hub.docker.com/r/neosmemo/memos |
| **贡献指南** | https://usememos.com/docs/development/contributing |
| **GitHub Sponsors** | https://github.com/sponsors/usememos |

## My takeaways

1. **轻量自托管笔记**的成熟方案：比 Obsidian 更偏「微博客/随手记」形态，部署简单（单 Docker 命令），适合个人或小团队快速搭建私有笔记站。
2. **零遥测 + MIT** 是核心卖点，对隐私敏感用户友好；与 MemPalace（AI 记忆）等工具定位不同——Memos 是通用短笔记，不是 Agent 专用记忆层。
3. **Web Clipper + API/Webhooks** 使其可嵌入现有工作流，可作为 Obsidian 之外的「快速入口」或对外公开分享层。
4. 社区活跃（约 6.27 万 Stars，最新 `v0.30.0`），文档与 Demo 完善，自托管笔记选型时可优先考虑。

## Related

- [[30 知识资源/其他/Obsidian 个人知识库方法论|Obsidian 个人知识库方法论]]
- [[30 知识资源/其他/多平台文章同步发布方案|多平台文章同步发布方案]]
- [[40 知识导航/工作流|工作流]]