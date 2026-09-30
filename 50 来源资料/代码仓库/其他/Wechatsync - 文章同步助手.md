---
id: source-20260618-wechatsync
title: Wechatsync - 文章同步助手
type: source
status: active
created: 2026-06-18
updated: 2026-06-18
tags:
  - 来源/代码仓库
  - 主题/多平台发布
  - 主题/自媒体工具
author: fun (lljxx1)
source_type: repo
source_url: https://github.com/wechatsync/Wechatsync
source_author: fun (lljxx1)
source_date: 2026-06-18
summary: 开源免费的 Chrome 浏览器扩展，一键同步文章到知乎、头条、掘金、CSDN、小红书等 29+ 平台，支持 MCP 协议与 AI 工作流集成。
related:
  - "[[30 知识资源/其他/多平台文章同步发布方案|多平台文章同步发布方案]]"
  - "[[40 知识导航/工作流|工作流]]"
---

# Wechatsync - 文章同步助手

## Source summary

开源免费的跨平台文章同步工具，Chrome 浏览器扩展形态。核心定位：**自媒体内容分发**，将写好的文章一键同步到 29+ 主流平台，告别重复复制粘贴。

- 5.8k Stars / 953 Forks
- GPL-3.0 协议开源
- Monorepo (TypeScript 59.6% + JavaScript 35.1%)
- 当前版本 v2.0.9 (2026-03-24)

## 工作原理

- **不是爬虫，不模拟登录，不经过第三方服务器**
- 使用浏览器已有的登录态（Cookie）
- 调用各平台 Web 编辑器的官方 API
- 数据不离开用户设备，所有请求从浏览器直达各平台
- 默认同步为草稿，发布前需人工确认

## 支持平台（29+）

### 主流自媒体

微信公众号、知乎、微博、小红书、头条号、抖音图文、B站专栏、百家号

### 技术社区

掘金、CSDN、语雀、51CTO、慕课网、开源中国、SegmentFault、博客园

### 通用平台

简书、搜狐号、大鱼号、一点号、什么值得买、网易号、豆瓣、人人都是产品经理

### 财经

雪球、东方财富

### 海外

X (Twitter)

### 建站/CMS

WordPress、Typecho、Hexo（Markdown 下载）、Hugo（Markdown 下载）

## 使用方式

### 1. Chrome 扩展（主入口）

- 推荐从 Chrome 应用商店安装（自动更新）
- 也支持手动加载 Release 包
- 兼容 Chrome / Edge / 360 / QQ 等 Chromium 内核浏览器

### 2. CLI 命令行

```bash
npm install -g @wechatsync/cli
export WECHATSYNC_TOKEN="你的token"

# 同步文章到多个平台
wechatsync sync article.md -p zhihu,juejin,csdn

# 查看平台登录状态
wechatsync platforms --auth

# 从浏览器当前页面提取文章
wechatsync extract -o article.md
```

### 3. MCP Server（AI 集成）

配置 `~/.claude/claude_desktop_config.json`：

```json
{
  "mcpServers": {
    "sync-assistant": {
      "command": "node",
      "args": ["/path/to/Wechatsync/packages/mcp-server/dist/index.js"],
      "env": {
        "MCP_TOKEN": "your-secret-token-here"
      }
    }
  }
}
```

MCP 可用工具：

| 工具 | 说明 |
|------|------|
| list_platforms | 列出所有平台及登录状态 |
| check_auth | 检查指定平台登录状态 |
| sync_article | 同步文章到指定平台（草稿） |
| extract_article | 从当前浏览器页面提取文章 |
| upload_image_file | 上传本地图片到平台 |

### 4. Claude Code Skill

```
/plugin marketplace add wechatsync
/plugin install wechatsync
```

### 5. JS SDK（网页调用）

通过 article-syncjs 库在网页端直接调用同步。

## 核心功能

- **一键批量发布**：同步到多个自媒体平台
- **网页转 Markdown**：智能提取正文，过滤广告，图片本地化，打包 ZIP
- **智能提取**：自动提取标题、内容、封面图（基于 Safari 阅读模式）
- **图片自动上传**：自动转存文章图片到目标平台
- **草稿模式**：同步后保存为草稿
- **AI 集成**：MCP / Claude Code Skill / OpenClaw

## 项目结构

```
Wechatsync/
├── packages/
│   ├── extension/     # Chrome 扩展 (MV3)
│   ├── mcp-server/    # MCP Server (stdio/SSE)
│   ├── cli/           # 命令行工具
│   └── core/          # 核心逻辑 (共享)
```

## 开发

```bash
pnpm install
pnpm dev    # 开发模式
pnpm build  # 构建
```

## My takeaways

1. **安全性设计优秀**：不经过第三方服务器，直接用浏览器 Cookie，开源可审计
2. **AI 集成前瞻**：MCP + CLI + Claude Code Skill 三种接入方式，覆盖不同工作流
3. **草稿优先策略**：不会自动发布，降低误操作风险
4. **平台覆盖面广**：29+ 平台，覆盖自媒体、技术社区、建站 CMS
5. **CLI 适合自动化**：可以集成到 CI/CD 或自定义工作流中

## Related

- [[30 知识资源/其他/多平台文章同步发布方案|多平台文章同步发布方案]]
- [[40 知识导航/工作流|工作流]]
