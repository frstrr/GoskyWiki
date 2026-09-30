---
id: source-20260928-tidings-rss
title: tidings-rss - 精选可验证 OPML 订阅合集
type: source
status: active
created: 2026-09-28
updated: 2026-09-28
tags:
  - 来源/代码仓库
  - 主题/RSS
  - 主题/OPML
  - 主题/开源资源
  - 主题/信息源
  - 主题/AI
author: fuxiaoai
source_type: repo
source_url: https://github.com/fuxiaoai/tidings-rss
source_author: fuxiaoai
source_date: 2026-09-28
summary: 经人工精选、生产环境多次校验的 OPML 订阅合集；覆盖 AI、工程、安全、科研、博客、视频、播客等，推荐从 Top 200 起步，完整目录约 718 源；许可 CC0-1.0，可配合任意 RSS 阅读器或 Tidings 客户端导入。
related:
  - "[[50 来源资料/代码仓库/其他/awesome-selfhosted - 自托管软件精选列表|awesome-selfhosted - 自托管软件精选列表]]"
  - "[[50 来源资料/代码仓库/AI/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach - AI Agent 互联网接入能力层]]"
---

# tidings-rss - 精选可验证 OPML 订阅合集

## Source summary

**tidings-rss** 是一份**可直接导入阅读器的高质量 RSS/Atom/JSON Feed OPML 合集**：按主题打包，并经 Tidings 生产解析器多轮实网校验，减少「订阅了但拉不到文」的情况。

- GitHub：`fuxiaoai/tidings-rss`（约 **489** Stars / **34** Forks；语言以 **Python** 为主；许可证 **CC0-1.0**）
- 主页：https://tidings.info/
- 定位：精选信息源目录 + OPML 分发包，不是阅读器本体
- 目录规模：完整集合约 **718** 源；推荐入口 **Top 200**（14 类均有代表）
- 目录最近校验（README）：**2026-09-14**

任意兼容 OPML 的 RSS 阅读器都可导入；官方推荐客户端为 **Tidings**（macOS 12+，AI 原生阅读器，可保留 OPML 分组）。

## 核心能力

| 能力 | 说明 |
|------|------|
| **按场景分包下载** | Top 200 / 完整包 / AI / 工程 / 中文源 / 博客 / 社区 / 安全 / 科技媒体 / 周刊 / 微信 / 公司技术 / 新闻 / 科研 / 视频 / 播客等 |
| **质量门槛** | 看重作者与编辑声誉、近期更新、正文可用、端点稳定；同类栏目去重；发布日志类不因更新频繁而压过原创 |
| **生产环境校验** | 候选源经 Tidings 解析器三轮检查；Top 200 须每轮都能返回文章与真实发布时间 |
| **中英平衡** | Top 200 兼顾中英文；另有中文独立博客、中文源、微信公众号等专题包 |
| **机器可读目录** | `data/feeds.json`、目录摘要与 SHA-256 校验文件，便于程序消费与完整性核对 |
| **开放共建** | 源失效、分类错误或希望增补可按贡献说明提交建议 |

## OPML 分包速览

| 合集 | 约源数 | 适合 |
|------|--------|------|
| **Top 200（推荐）** | 200 | 广覆盖、低清理成本的第一导入 |
| 完整集合 | 718 | 自建归档或自行精简 |
| 人工智能 | 99 | 模型、研究、工具与观点 |
| 工程与技术 | 419 | 软件、架构、开发者工具与实践 |
| 中文源 | 469 | 中文文章、社区、音视频 |
| 中文独立博客 | 349 | 活跃的中文个人写作 |
| 公司技术 | 40 | 一线机构工程/AI/安全/研究写作 |
| 视频 / 播客 | 93 / 73 | 频道订阅与音频节目 |
| 微信公众号 | 30 | 在 App 外阅读精选公号文章 |
| 社区 / 安全 / 科技媒体 / 周刊 / 新闻 / 科研 | 各数十 | 按主题加深，而非一次全量 |

专题包之间有重叠；完整集合中每个 feed URL 只出现一次。不确定时优先 **Top 200**。

## 与配套客户端 Tidings

仓库与 [Tidings](https://tidings.info/) 配套宣传，但 OPML **不绑定**该客户端。Tidings 侧亮点（便于选型，非本仓库代码能力）：

- 文章 / 图片 / 视频 / 播客同一库管理
- 划线、笔记、Ask AI；未读列表 AI Radar 聚类
- 段落级对照翻译；可导出 Markdown / 写入 Obsidian
- 支持 FreshRSS 双向同步；AI 能力使用用户自选模型提供商（Pro 不含模型用量）

## 适用场景

- 想快速搭一套**高质量中英技术/AI 资讯订阅**，不想自己从零搜源
- 需要按主题导入（只关心 AI、安全、播客等），而不是一次塞满上千订阅
- 给自建 FreshRSS / Miniflux / 任意阅读器提供**可校验的 OPML 起点**
- 为 Agent / 信息工作流准备「可信公开信息源」清单（可与 [[50 来源资料/代码仓库/AI/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach]] 等上网能力层互补）

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/fuxiaoai/tidings-rss |
| **Tidings 官网** | https://tidings.info/ |
| **Top 200 OPML** | https://github.com/fuxiaoai/tidings-rss/releases/latest/download/tidings-top200.opml |
| **完整 OPML** | https://github.com/fuxiaoai/tidings-rss/releases/latest/download/tidings-all.opml |
| **AI 专题 OPML** | https://github.com/fuxiaoai/tidings-rss/releases/latest/download/tidings-ai.opml |
| **RSS 使用指南** | https://github.com/fuxiaoai/tidings-rss/blob/main/RSS-GUIDE.md |
| **机器可读目录** | https://github.com/fuxiaoai/tidings-rss/blob/main/data/feeds.json |
| **目录摘要** | https://github.com/fuxiaoai/tidings-rss/blob/main/reports/catalog-summary.md |
| **校验和** | https://github.com/fuxiaoai/tidings-rss/releases/latest/download/SHA256SUMS.txt |
| **贡献 / 推荐源** | https://github.com/fuxiaoai/tidings-rss/blob/main/CONTRIBUTING.md |

## My takeaways

1. **价值在「精选 + 可导入」**：不是阅读器框架，而是经过校验的信息源地图；选型时先 Top 200，再按主题加深。
2. **与 awesome 类列表同类但更可操作**：输出是 OPML，可直接进 FreshRSS / Tidings 等，而不是只给人扫 README。
3. **注意可用性边界**：校验基于当时网络与官方可达端点，不保证所有地区/运营商长期可用；源会迁移或消失，需定期更新分包。
4. 与 wiki 中 [[50 来源资料/代码仓库/其他/awesome-selfhosted - 自托管软件精选列表|awesome-selfhosted]] 互补：那边偏自托管软件选型，这边偏**持续信息摄入**的订阅起点。

## Related

- [[50 来源资料/代码仓库/其他/awesome-selfhosted - 自托管软件精选列表|awesome-selfhosted - 自托管软件精选列表]]
- [[50 来源资料/代码仓库/AI/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach - AI Agent 互联网接入能力层]]