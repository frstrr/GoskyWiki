---
id: source-20261012-rea
title: REA - Agent 逆向分析 MCP
type: source
status: active
created: 2026-10-12
updated: 2026-10-12
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/MCP
  - 主题/CLI
  - 主题/AI编程Agent
  - 主题/开源资源
  - 主题/静态分析
author: morluto
source_type: repo
source_url: https://github.com/morluto/rea
source_author: morluto
source_date: 2026-10-12
summary: "面向 AI Agent 的逆向分析 MCP/CLI（npm 包 rea-agents）：把 Ghidra/Hopper/IDA 等引擎与 JS/Electron、.NET、APK、浏览器等检查能力接到 Claude Code / Cursor / Codex 等 Agent，本机分析并返回带证据的结论。"
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill - AI Agent 登录态浏览器操控]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach - AI Agent 互联网接入能力层]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# REA - Agent 逆向分析 MCP

## Source summary

**REA**（Reverse Engineer Anything，仓库 [morluto/rea](https://github.com/morluto/rea)）是一个面向 AI Agent 的**逆向分析能力层**：通过 **MCP + CLI**（npm 包 `rea-agents`）把本机分析工具接到 Agent，在**没有源码**时也能检查应用行为，并返回带证据与限制说明的结论。

- 约 **107,714 stars** / **23,844 forks**（2026-10-12 GitHub API）；许可证 **MIT**；主语言 **TypeScript**；当前 npm 包版本约 **6.4.0**
- 官网：[rea.tools](https://rea.tools/)（中文站：[rea.tools/zh-CN](https://rea.tools/zh-CN/)）
- 一句话安装/注册：

```bash
npx rea-agents setup
```

- 支持的 Agent 设置流程：Claude Code、Codex、Cursor、Gemini CLI、Grok Build 等；其他客户端可手动注册 MCP
- 原生深度分析依赖 Hopper / Ghidra / IDA 之一；静态 JS / .NET 检查不需要这些引擎

来源线索：今日头条转载新智元文章 [比Jev更爆火！逆向灭霸 REA 一天登顶GitHub](https://www.toutiao.com/article/7695299637465121315/)（文章侧重热度与行业影响；仓库定位与能力以官方 README 为准）。

## 解决什么问题

| 痛点 | REA 方案 |
|------|----------|
| 想搞懂某功能实现，但只有二进制/安装包 | Agent 通过 MCP 调用本机分析工具，返回伪代码/汇编/模块关系与证据 |
| 传统逆向门槛高（IDA/Ghidra 上手重） | 用自然语言驱动调查；REA 做工具桥接与证据返回 |
| Agent「能写不能看」闭源实现 | 把分析引擎接到 MCP，让 Agent 能追调用、解释行为、再实现类似功能 |
| 多目标类型工具链零散 | 统一 CLI/MCP 覆盖原生、JS/Electron、.NET、APK、浏览器、固件等 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **MCP 给 Agent** | Agent 调 REA 检查目标、追踪相关代码；结果含发现 + 证据 + 未知项 |
| **CLI 同工作流** | `rea` / `npx rea-agents` 可在终端跑同类分析（含 `--json`） |
| **原生二进制** | 伪代码、汇编、字符串、符号、调用与引用（需 Hopper/Ghidra/IDA） |
| **JS / Electron** | 模块、导入、source map、路由、IPC、原生扩展关系；可分析目录或 ASAR |
| **网站 / 网络捕获** | 页面结构、脚本、网络观察、截图；HAR / mitmproxy 捕获支持 |
| **.NET / Android / 固件等** | 程序集元数据与 CIL；APK（JADX）/ ADB 设备；Binwalk/Unblob 固件提取等 |
| **本机分析** | 目标在本机处理；智能体会收到工具结果，其模型提供商另有数据政策 |
| **Skill / 工作流指引** | setup 会注册 MCP 并安装匹配调查指引（亦可经 skills.sh） |

## 可以分析什么（摘要）

| 目标 | 典型产出 | 额外要求（摘要） |
|------|----------|------------------|
| 原生二进制 | 伪代码、汇编、符号与引用 | Hopper / Ghidra / IDA |
| JavaScript / Electron | 模块图、IPC、Electron 边界 | Node.js + npm |
| 网站 | 结构、脚本、网络与截图 | Chrome 系浏览器 |
| .NET 程序集 | 元数据、CIL、原生依赖 | 静态检查 |
| Android APK / 设备 | 清单、反编译方法；ADB 进程/日志等 | JADX + JDK；adb |
| 固件 / 软件包资源 | 提取区域、bundle/plist 等 | Binwalk/Unblob 等（见文档） |

完整表与主机前提见仓库 README「可以分析什么」与官网 Guides。

## 快速开始

**环境**：Node.js 22.x（≥22.19）、24.x（≥24.11）或 26+，以及 npm。

```bash
# 注册到本机 Agent（会备份已有配置；确认变更后重启 Agent）
npx rea-agents setup

# 常用全局 CLI
npm install --global rea-agents
rea --help
rea update
```

对 Agent 的自然语言示例（把目标换成你有权分析的对象）：

```text
弄清某应用的搜索功能如何工作，展示证据，并为我的项目实现类似功能。
```

CLI 示例（已解包的 JS/Electron 目录或 ASAR）：

```bash
npx -y rea-agents@latest analyze-javascript-application /absolute/path/to/app --json
```

## 官方案例（了解能力边界）

| 案例 | 要点 | 链接 |
|------|------|------|
| DX-Ball 声像计算 | 从调用追到辅助函数，重建 C，并用大量测试对齐原指令 | [showcase](https://rea.tools/zh-CN/showcase/dx-ball/) |
| Notion 剪贴板桥 | Electron：渲染进程 API → preload/IPC → 主进程 | [showcase](https://rea.tools/zh-CN/showcase/notion/) |
| TH04 弹幕环 | 16 位 DOS/PC-98 指令恢复与编译器输出对比 | [showcase](https://rea.tools/zh-CN/showcase/th04/) |

## 合规与风险（必读）

- **官方免责声明**：REA 面向合法的逆向研究、分析与重建；使用者须自行取得授权并遵守适用法律，项目不支持非法或未经授权使用。
- **授权优先**：仅用于你拥有、已获授权，或法律明确允许的目标（如自有产品、安全研究授权范围、公开协议下的互操作分析等）。
- **别把营销文当操作手册**：媒体文章常夸大「扒商业软件」叙事；实际落地以授权、合同与当地法律为准。
- **隐私**：分析在本机，但 Agent/模型提供商会看到工具返回内容，注意敏感符号与密钥外泄。
- **运行时捕获**：部分模式会以你的用户权限运行或交互目标进程，按各运行时指南评估影响。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/morluto/rea |
| **官网** | https://rea.tools/ |
| **中文官网** | https://rea.tools/zh-CN/ |
| **指南** | https://rea.tools/zh-CN/guides/ |
| **案例** | https://rea.tools/zh-CN/showcase/ |
| **npm** | https://www.npmjs.com/package/rea-agents |
| **skills.sh** | https://skills.sh/morluto/rea/reverse-engineer-anything |
| **中文 README** | https://github.com/morluto/rea/blob/main/README_zh.md |
| **安装文档** | https://github.com/morluto/rea/blob/main/docs/installation.md |
| **CLI 文档** | https://github.com/morluto/rea/blob/main/docs/cli.md |
| **MCP 契约** | https://github.com/morluto/rea/blob/main/docs/mcp-contracts.md |
| **Discord** | https://discord.gg/GkcryMnJDM |
| **头条介绍文** | https://www.toutiao.com/article/7695299637465121315/ |
| **许可证** | MIT |

## My takeaways

1. **定位是 Agent 的逆向 MCP/CLI**，不是又一个独立 GUI 反编译器；深度原生分析仍要 Hopper/Ghidra/IDA。
2. **热度极高**（十余万星），和 BrowserSkill / Agent-Reach 一样属于「给 Agent 补专项能力」赛道，只是对象从网页/平台变成了二进制与制品。
3. **证据链是卖点**：结论带引用与限制，适合「搞懂行为 → 合法重建」而不是盲抄。
4. **合规是硬门槛**：能力越强，越要卡在授权与用途上；笔记只作资源索引，不展开未授权逆向步骤。
5. **与现有笔记互补**：[[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill]] 管登录态浏览器；[[50 来源资料/代码仓库/AI/Agent工具与中间件/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach]] 管多平台接入；REA 管本机二进制/制品分析。

## Related

- [[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill - AI Agent 登录态浏览器操控]]
- [[50 来源资料/代码仓库/AI/Agent工具与中间件/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach - AI Agent 互联网接入能力层]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]