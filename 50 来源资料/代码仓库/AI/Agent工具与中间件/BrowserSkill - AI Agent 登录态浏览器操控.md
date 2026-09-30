---
id: source-20260924-browserskill
title: BrowserSkill - AI Agent 登录态浏览器操控
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/CLI
  - 主题/AI编程Agent
  - 主题/浏览器自动化
  - 主题/开源资源
author: Tencent
source_type: repo
source_url: https://github.com/Tencent/BrowserSkill
source_author: Tencent
source_date: 2026-09-24
summary: 腾讯开源的浏览器自动化 Skill：CLI（bsk）+ Chrome/Edge 扩展，让 Cursor/Claude Code/Codex 等 Agent 在你已登录的真实浏览器里读写页面、填表、截图与调试网站，任务在独立 Agent Window 可见执行。
related:
  - [[50 来源资料/代码仓库/AI/Agent工具与中间件/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach - AI Agent 互联网接入能力层]]
  - [[50 来源资料/代码仓库/AI/Agent工具与中间件/Bright Data CLI - 终端网页数据采集工具|Bright Data CLI - 终端网页数据采集工具]]
  - [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
  - [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
  - [[40 知识导航/AI 工具使用|AI 工具使用]]
---

# BrowserSkill - AI Agent 登录态浏览器操控

## Source summary

**BrowserSkill**（仓库 [Tencent/BrowserSkill](https://github.com/Tencent/BrowserSkill)）让 AI Agent 连接你本机已登录的 **Chrome / Microsoft Edge**，在独立可见的 **Agent Window** 中读页面、点控件、填表、截长图、排查失败请求——不打断你正在用的标签页（也可显式借走现有标签，任务结束后归还）。

- 约 **7,036 stars** / **509 forks**（2026-09-24 GitHub API）；许可证 **MIT**；GitHub 主语言标注 TypeScript（仓库为 Rust + pnpm 工作区，含扩展与 CLI）
- 组成：**bsk CLI（含后台 daemon）+ 浏览器扩展 + Agent Skill**；DeepSeek Harness 另有专用 DSH 插件
- 兼容：Cursor、Claude Code、Codex、OpenClaw、CodeBuddy、WorkBuddy、Pi、Hermes Agent 等具备 shell 能力的 Agent
- 一句话让 Agent 自助安装：

```text
Set up browser-skill on this machine by following https://raw.githubusercontent.com/Tencent/BrowserSkill/main/AGENT_INSTALL.md
```

定位对比：[[50 来源资料/代码仓库/AI/Agent工具与中间件/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach]] 偏多平台读写路由/选型；[[50 来源资料/代码仓库/AI/Agent工具与中间件/Bright Data CLI - 终端网页数据采集工具|Bright Data CLI]] 偏云端采集与反爬；**BrowserSkill 偏本地登录态 + 可见操控 + 网站调试证据链**。

## 解决什么问题

| 痛点 | BrowserSkill 方案 |
|------|-------------------|
| Agent 无登录态，进不了内网/已登录站点 | 复用当前浏览器 profile 的 Cookie/会话 |
| 无头自动化难介入人机验证 | 独立 Agent Window；可请求人工处理登录/验证码 |
| 网页 Bug 缺请求/控制台证据 | Website debugging：操作时间线 + 请求体 + Console + 性能 |
| 多 Agent 宿主 CLI 不一 | 统一 bsk + bsk install-skill 写入各 harness |

## 核心能力

| 能力 | 说明 |
|------|------|
| **沿用已有账号** | 读文档、搜内网、填表、跑网页工作流，无需另登 |
| **任务可见可控** | 独立 Agent Window；可借标签并归还；可接管人机步骤 |
| **读/交互/截图** | 观察页面文本与控件、点击输入、管标签、视口/整页截图；本地模式支持上传下载 |
| **网站调试取证** | 关联操作与请求/响应体/Console/页面变化；性能与疑似重复请求；HTTP 规则或同域重放验证假设 |
| **多浏览器/多 profile** | 命名实例、绑定 profile；也可服务器 Agent + 本机浏览器经认证 WSS 配对（浏览器主动连出，本机无需入站端口） |
| **可回溯** | 扩展内调试历史；导出 JSON；可选操作审计（默认关） |

## 快速开始

**组件**：AI Agent + bsk CLI + Chrome/Edge 扩展（Chromium ≥ 125）。

**推荐**：把 Agent 安装指南甩给 Agent 自装（见上）。扩展仍需手工装：

- [Chrome Web Store](https://chromewebstore.google.com/detail/hhcmgoofomhgciiibhipgmgkgnoenaoi)
- [Edge Add-ons](https://microsoftedge.microsoft.com/addons/detail/browserskill/emacgiaaaiojkkpkddmmdfhmokgmnikg)

**手工 CLI（摘要）**

```powershell
# Windows
irm https://raw.githubusercontent.com/Tencent/BrowserSkill/main/install.ps1 | iex
bsk --version
bsk install-skill --harness cursor --json
bsk doctor
```

```sh
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/Tencent/BrowserSkill/main/install.sh | sh
export PATH='${BSK_INSTALL_DIR:-$HOME/.local/bin}:$PATH'
```

首任务示例：

```text
Use browser-skill to open https://example.com, summarize the page, and end the browser session when finished.
```

CLI 直调骨架：bsk session start → navigate / observe / screenshot → session stop（务必收尾，借走的标签才会归还）。

## 网站调试（亮点）

适合「授权调试的站点」：先开 capture，再复现问题，再让 Agent/扩展侧看证据。

- 跟一次操作：请求、Console、表单值、即时/延迟 DOM 变化
- 查请求：头、提交体、响应体、耗时与错误；可滤流量或抠 JSON 字段
- 性能：加载指标、API 耗时汇总、疑似重复请求
- 验证假设：任务内改写/拦截/mock/同域重放；导出 JSON 留存

重放会用当前页面会话发新请求，可能改服务端数据；证据可能含敏感信息，需自行脱敏与授权范围控制。

## 隐私与安全（必读）

- Agent Window **共用所选 profile 登录态**，不是隔离沙箱；Agent 拥有你已登录站点的同等权限 → 只交给可信 Agent/任务
- 扩展两项默认开启的自动化设置：「借标签前确认」「允许请求人工帮助」；旧 CLI 开关不能覆盖
- **不强制云服务、不采产品遥测**；结果到你选的 daemon/网关与所用 Agent，按其策略处理
- 调试历史存浏览器 profile（停用记录约 30 天 / 50 条 / 50MiB）；操作审计默认关，存 BSK_HOME/audit

## 更新

```sh
bsk update --yes
```

扩展走商店更新；DSH 插件单独更新。保持 CLI、daemon、扩展、可选插件版本一致。

## 技术栈（开发者）

Rust + pnpm workspace；构建需 Rust stable、Node.js 22、与 package.json 锁定的 pnpm。

| 目录 | 组件 |
|------|------|
| crates/bsk-cli | CLI、daemon、捆绑 Agent skill |
| crates/bsk-protocol | 协议类型与 JSON Schema |
| apps/extension | 扩展 UI、自动化与调试工作台 |
| packages/dsh-plugin-browserskill | DeepSeek Harness 插件 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/Tencent/BrowserSkill |
| **Agent 安装指南** | https://raw.githubusercontent.com/Tencent/BrowserSkill/main/AGENT_INSTALL.md |
| **中文 README** | https://github.com/Tencent/BrowserSkill/blob/main/README.zh-CN.md |
| **Changelog** | https://github.com/Tencent/BrowserSkill/blob/main/CHANGELOG.md |
| **Releases** | https://github.com/Tencent/BrowserSkill/releases |
| **网站调试文档** | https://github.com/Tencent/BrowserSkill/blob/main/docs/website-debugging.md |
| **远程扩展连接** | https://github.com/Tencent/BrowserSkill/blob/main/docs/remote-extension-connection.md |
| **沙箱 Agent** | https://github.com/Tencent/BrowserSkill/blob/main/docs/sandboxed-agents.md |
| **架构** | https://github.com/Tencent/BrowserSkill/blob/main/docs/architecture.md |
| **Chrome 扩展** | https://chromewebstore.google.com/detail/hhcmgoofomhgciiibhipgmgkgnoenaoi |
| **Edge 扩展** | https://microsoftedge.microsoft.com/addons/detail/browserskill/emacgiaaaiojkkpkddmmdfhmokgmnikg |
| **DSH 插件 npm** | https://www.npmjs.com/package/@wxg-prc-cpg/browser-skill-dsh-plugin |
| **隐私说明** | https://github.com/Tencent/BrowserSkill/blob/main/apps/extension/PRIVACY.md |
| **许可证** | MIT |

## My takeaways

1. **本地登录态是核心差异**：适合内网文档、已登录后台、需要人机协作验证的网页任务，而不是纯公开页批量采集。
2. **可见 Agent Window + 可借标签** 比纯无头 Playwright 更适合「边看边纠」的人类协作。
3. **Website debugging** 把「操作 → 网络 → Console → 页面变化」串成证据链，对前端/接口排障很实用。
4. **权限即风险**：共用 profile 等于把账号能力交给 Agent；应用小号/专用 profile，并保持借标签确认。
5. **与 Agent-Reach / Bright Data 互补**：Reach 管多平台渠道选型，Bright Data 管云采集，BrowserSkill 管本机真实浏览器操控与调试。

## Related

- [[50 来源资料/代码仓库/AI/Agent工具与中间件/Agent-Reach - AI Agent 互联网接入能力层|Agent-Reach - AI Agent 互联网接入能力层]]
- [[50 来源资料/代码仓库/AI/Agent工具与中间件/Bright Data CLI - 终端网页数据采集工具|Bright Data CLI - 终端网页数据采集工具]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]