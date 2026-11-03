---
id: source-20261103-opendots
title: OpenDots - 持久化 AI 同事工作区模板
type: source
status: active
created: 2026-11-03
updated: 2026-11-03
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/AI编程Agent
  - 主题/人机协作
  - 主题/MCP
author: CopilotKit
source_type: repo
source_url: https://github.com/CopilotKit/OpenDots
source_author: CopilotKit
source_date: 2026-11-03
summary: CopilotKit 开源的持久化 AI 同事工作区模板：每个 Dot 可有独立电脑、Spaces 文档库、文字/通话/Slack 多通道，内置人机审批、MCP/Composio 连接与 Automatic Learning；自托管部署，状态仍为 Alpha。
related:
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
---

# OpenDots - 持久化 AI 同事工作区模板

## Source summary

**OpenDots** 是 CopilotKit 提供的**开源、可自托管的持久化 AI 同事（always-on AI coworkers）工作区模板**：在 Web/移动端里管理 Spaces 文档、为每个 Specialist Dot 配置角色与工具，并让 Agent 在文字、实时通话与 Slack 之间连续工作；每个 Dot 还可拥有基于 OpenBot 的独立电脑（浏览器/文件/终端）。

- 开源项目，GitHub：[CopilotKit/OpenDots](https://github.com/CopilotKit/OpenDots)
- 语言：**TypeScript**
- 约 **6,350 stars / 924 forks**（2026-11-03）
- 许可证：**MIT**
- 状态：**Alpha**（模板，非托管 SaaS 成品）
- 官网：**https://www.copilotkit.ai/opendots**
- 技术底座：[CopilotKit](https://github.com/CopilotKit/CopilotKit) + [AG-UI](https://docs.ag-ui.com/introduction)；电脑能力参考 [OpenBot](https://github.com/CopilotKit/OpenBot)；Slack 参考 [OpenTag](https://github.com/CopilotKit/OpenTag) / [Channels SDK](https://github.com/CopilotKit/channels-sdk)

> 对话持久化依赖 **CopilotKit Intelligence**（托管云、本地 Docker 评估，或授权自托管）。页面/工作区元数据存在本地库，与会话历史分离。

## 解决什么问题

| 痛点 | OpenDots 方案 |
|------|----------------|
| 只有聊天，没有可持久文档与工作区 | Spaces + 可视化页面编辑器；对话可审批后落成 Space 页面 |
| Agent 没有独立执行环境 | 每 Dot 可挂独立电脑（浏览器配置/工作区文件跨启停持久） |
| 工具一调用就不可控 | MCP 写操作、保存草稿等用 HITL 卡片：**Approve & save / Approve & run** |
| 只想在 Slack / 通话里继续同一上下文 | Channels SDK 托管 Slack；Realtime 通话与计算 Agent 共享会话与权限 |
| 想自建而非买闭源 Cowork | Clone 模板自托管，按需接模型、Intelligence、浏览器服务、Composio |

## 核心能力

| 能力 | 说明 |
|------|------|
| **Spaces 与页面** | 可搜索文档库（网格/列表）、嵌套子页、可视化编辑（格式化/斜杠命令/撤销重做）、自动保存与修订冲突防护、Markdown 源码模式 |
| **Specialist Dots** | 名称、角色、指令、工具权限可配；研究/写作者等分工；可授权访问多个 Space |
| **Dot computers** | 经 OpenBot 容器监督与电脑服务：浏览器控制、人工接管、文件、终端输出与活动面板；凭据在服务端，每 Dot 派生独立电脑凭据 |
| **人机审批落稿** | 草稿先展示，Approve & save 写入授权 Space 并回链；同稿重试可恢复已存页 |
| **Connections（MCP）** | 任意 MCP（邮件/日历/GitHub 等）；只读可自动跑，其余需 Approve & run；可按 Dot 开关工具 |
| **Connected apps** | 经 [Composio](https://composio.dev) 接 Gmail/日历等；需 `COMPOSIO_API_KEY` + 全局与 Dot 双开关；`OWNER_ID` 须稳定 |
| **文字 + 通话** | 连续会话时间线；通话用 Realtime 语音 + 独立计算 Agent，可 mute/最小化回聊天 |
| **Slack** | 托管 Channels 连接；工作区/用户白名单；点名 Dot 后在线程继续 |
| **后台工作** | 在原会话中调度服务端回合，支持暂停/重试；中断需人工 Retry after review |
| **公开网页研究** | 默认 Parallel Search MCP；可切 browser 只读或 disabled |
| **Automatic Learning** | 每 Dot Learning 容器、会话证据路由与已发布 skill 投递（部分能力仍待联调验证） |
| **外观与记忆** | 系统/浅/深色；用户可管偏好供获权 Dot 使用 |

## 架构要点

- **AG-UI**：在后端与 CopilotKit 组件间流式传递消息、工具调用与 Agent 状态；电脑活动可内联展示。
- **栈**：TanStack AI（模型流式与服务端工具）+ CopilotKit React SDK/Runtime + Intelligence Threads + Channels SDK（Slack）。
- **参考实现**：[OpenMuse](https://github.com/CopilotKit/OpenMuse)、[OpenBot](https://github.com/CopilotKit/openbot)（持久工作、Agent 电脑与执行控制）。

## 安装与快速开始

要求：**Node.js 24** + **npm**。

```sh
git clone https://github.com/CopilotKit/OpenDots.git
cd OpenDots
npm ci
cp .env.example .env
npm run dev
```

浏览器打开 **http://127.0.0.1:5173**。可先建 Space、写页面、配置 Dot；要真正聊天需接 Intelligence 项目与模型：

```sh
npx copilotkit@latest login
npx copilotkit@latest project select
# 写入 CPK_INTELLIGENCE_API_KEY；保留 CPK_TELEMETRY_ID
# 再配置 OPENAI_API_KEY / OPENAI_MODEL 后重启
```

**注意**：不要在本目录跑 `copilotkit onboard`（会再塞一套通用集成，与模板冲突）。本地 Docker Intelligence、Slack、通话、浏览器服务等见官方 [Setup](https://github.com/CopilotKit/OpenDots/blob/main/docs/SETUP.md)。

## 数据与隐私（摘要）

- 会话消息/工具/运行事件落在所配置的 Intelligence；托管即上 CopilotKit 云，本地评估则在本机。
- 页面与工作区元数据在本地 SQLite；**没有**独立的 SQLite 会话库。
- 模型提供商会收到授权页面内容与工具结果；Composio/语音/消息集成启用时数据会到对应厂商。
- 可用 `COPILOTKIT_TELEMETRY_DISABLED=true` 或 `DO_NOT_TRACK=1` 关闭遥测。

## 适用场景

- 想自建「AI 同事工作区」：多角色 Dot + 文档 Space + 可选独立电脑
- 需要 **HITL 审批** 再落稿/跑写工具，而非全自动乱改
- 希望同一 Agent 在 **Web 聊天、语音通话、Slack** 间延续上下文
- 已有或计划用 CopilotKit / AG-UI / Intelligence 生态做产品原型

**不太适合**：要开箱即用托管 SaaS、不愿配 Intelligence/模型/可选电脑服务；或只要轻量单 Agent CLI、不需要 Spaces/多通道的场景（可对照 [[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi]] 的桌面 Cowork，或纯终端的 [[50 来源资料/代码仓库/AI/Agent运行时与编排/Luvus - AI Agent 任务控制中心|Luvus]]）。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/CopilotKit/OpenDots |
| **产品页** | https://www.copilotkit.ai/opendots |
| **Setup 文档** | https://github.com/CopilotKit/OpenDots/blob/main/docs/SETUP.md |
| **Computer 配置** | https://github.com/CopilotKit/OpenDots/blob/main/docs/COMPUTERS.md |
| **Connections** | https://github.com/CopilotKit/OpenDots/blob/main/docs/CONNECTIONS.md |
| **CopilotKit** | https://github.com/CopilotKit/CopilotKit |
| **AG-UI 文档** | https://docs.ag-ui.com/introduction |
| **Channels SDK** | https://github.com/CopilotKit/channels-sdk |
| **OpenBot** | https://github.com/CopilotKit/OpenBot |
| **OpenMuse** | https://github.com/CopilotKit/OpenMuse |
| **OpenTag** | https://github.com/CopilotKit/OpenTag |
| **与团队约谈** | https://www.copilotkit.ai/talk-to-an-engineer?ref=opendots_readme |
| **License** | MIT |

## My takeaways

1. **模板定位清晰**：不是竞品桌面 Cowork 成品，而是可 fork 的 Agent 工作区起点；价值在 Spaces + Dot 电脑 + 多通道 + HITL。
2. **与 AionUi 互补**：AionUi 偏「装好就能用的桌面 Multi-Agent Cowork」；OpenDots 偏「CopilotKit/AG-UI 栈上的自托管 Web 工作区 + 独立电脑」。
3. **安全默认偏审批**：MCP 写操作与落稿走 Approve 卡片，适合接真实邮箱/日历前的可控实验。
4. **依赖链要心里有数**：聊天靠 Intelligence，电脑靠 OpenBot 服务，Slack 靠 Channels，语音另配 Realtime——评估时按模块逐项接通。
5. **Alpha 边界**：README 明确多用户协作、文件上传、完整目标/事件触发系统等尚未包含；选型时按「单所有者起步模板」看待。

## Related

- [[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
