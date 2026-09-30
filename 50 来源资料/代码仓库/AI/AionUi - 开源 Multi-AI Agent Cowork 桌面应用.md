---
id: source-20260831-aionui
title: AionUi - 开源 Multi-AI Agent Cowork 桌面应用
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/AI编程Agent
  - 主题/办公自动化
author: iOfficeAI
source_type: repo
source_url: https://github.com/iOfficeAI/AionUi
source_author: iOfficeAI
source_date: 2026-08-31
summary: 免费开源的 24/7 Cowork 桌面应用，内置 Agent 零配置开箱即用，统一编排 Claude Code、Codex、OpenClaw、Hermes 等 20+ CLI Agent，支持 Team Mode、Cron 定时任务、WebUI/IM 远程访问与 Office 文档生成。
related:
  - "[[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[50 来源资料/代码仓库/AI/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
---

# AionUi - 开源 Multi-AI Agent Cowork 桌面应用

## Source summary

**AionUi** 是一个**免费开源的 Cowork 桌面应用**，让 AI Agent 像同事一样在你的电脑上协作——读文件、写代码、搜网页、自动化任务，全程透明可控。相比 Claude Cowork（仅 macOS、仅 Claude、$100/月），AionUi 是全模型、跨平台、开源的增强替代。

- 开源项目，GitHub：[iOfficeAI/AionUi](https://github.com/iOfficeAI/AionUi)
- 语言：**TypeScript**（Electron 前端 + [AionCore](https://github.com/iOfficeAI/AionCore) 本地后端）
- 约 **32,400+ stars**（2026-08-31）
- 平台：**macOS · Windows · Linux**
- 许可证：**Apache-2.0**
- 最新版本：**v2.1.61**
- 官网下载：**https://www.aionui.com**（安装包不再发布在 GitHub Releases）

## 解决什么问题

| 痛点 | AionUi 方案 |
|------|-------------|
| 传统 AI 聊天客户端不能操作本地文件 | 内置 Agent，完整文件读写权限，Cowork 而非纯聊天 |
| 需要分别安装/管理多个 CLI Agent | 自动检测已装 CLI，统一界面编排 20+ Agent |
| Claude Cowork 平台/模型/成本受限 | 跨平台 + 30+ 模型 + 免费开源 |
| 无法 24/7 无人值守 | Cron 定时任务，防休眠，支持自然语言创建任务 |
| 移动端/IM 无法触发 Agent | WebUI + Telegram / 飞书 / 钉钉 / 微信 |
| 文档产出需切换多个 App | 内置预览面板（10+ 格式）+ OfficeCLI 生成 PPT/Word/Excel |

## 核心能力

| 能力 | 说明 |
|------|------|
| **内置 Agent（零配置）** | 安装即用，无需 CLI；粘贴任意 API Key 即可；21 个专业助手开箱即用 |
| **多 Agent 模式** | 自动检测 Claude Code、Codex、Qwen Code、Gemini CLI、OpenClaw、Hermes、Cursor Agent、OpenCode 等 |
| **Team Mode** | Leader 分解任务 → Teammate 并行执行；共享工作区；ACP 协议连接外部 Agent |
| **任意 API Key** | Gemini / OpenAI / Anthropic / AWS Bedrock / Ollama / NewAPI 等 30+ 平台 |
| **Cron 定时任务** | Cron 表达式 / 固定间隔 / 一次性；绑定会话；24/7 无人值守 |
| **远程访问** | WebUI（LAN/跨网/无头服务器）；Telegram / 飞书 / 钉钉 / 微信 Bot |
| **预览面板** | PDF、Word、Excel、PPT、代码、Markdown、图像、HTML、Diff 等 10+ 格式即时预览与编辑 |
| **办公文档生成** | 通过 [OfficeCLI](https://github.com/iOfficeAI/OfficeCLI) 生成 `.pptx` / `.docx` / `.xlsx` |
| **技能生态** | 内置 + 自定义 + Extension SDK 三层技能；对话级启用/禁用 |
| **数据本地化** | 全部数据存本地 SQLite，不上传服务器 |

## 内置专业助手（21 个）

Cowork、PPT 生成器、Morph PPT / Morph PPT 3D、Pitch Deck 生成器、仪表板生成器、Word 生成器、Word 表单生成器、Excel 生成器、学术论文写作、财务模型生成器、3D 游戏、UI/UX Pro Max、文件规划助手、HUMAN 3.0 教练、社交招聘发布、moltbook、Beautiful Mermaid、OpenClaw 设置、故事角色扮演、AionUi Butler 等。

内置定义权威来源：[AionCore assistants.json](https://github.com/iOfficeAI/AionCore/blob/main/crates/aionui-app/assets/builtin-assistants/assistants.json)

## 支持的 CLI Agent（多 Agent 模式）

内置 Agent（[aionrs](https://github.com/iOfficeAI/aionrs) 引擎）• Claude Code • Codex • Qwen Code • Gemini CLI • Goose • OpenClaw • Augment Code • CodeBuddy • Kimi CLI • OpenCode • Factory Droid • GitHub Copilot • Qoder • Mistral Vibe • Nanobot • Snow • Hermes • Cursor Agent • Pi • MiMo Code • omp • Antigravity 等

## 与 Claude Cowork 对比

| 维度 | Claude Cowork | AionUi |
|------|---------------|--------|
| OS | 仅 macOS | macOS / Windows / Linux |
| 模型 | 仅 Claude | Gemini、Claude、DeepSeek、OpenAI、Ollama 等 |
| 交互 | 桌面 GUI | 桌面 GUI + WebUI + IM 集成 |
| 自动化 | 手动 | Cron 24/7 |
| 成本 | $100/月 | 免费开源（仅 API 用量付费） |

## 安装与快速开始

**系统要求**：macOS 10.15+ / Windows 10+ / Ubuntu 18.04+；建议 4GB+ 内存、500MB+ 磁盘。

```bash
# 官网下载（推荐）
# https://www.aionui.com

# macOS Homebrew
brew install aionui
```

三步上手：① 安装 → ② 输入任意 API Key → ③ 开始 Cowork。

## 适用场景

- **不想装 CLI 但要 Cowork 能力**：内置 Agent 零配置，粘贴 Key 即用
- **已有 Claude Code / Codex / OpenClaw 等 CLI**：统一界面并行编排，Team Mode 协作
- **办公自动化**：定时汇总数据、生成报告、整理文件、PPT/Word/Excel 文档产出
- **远程/移动端触发 Agent**：WebUI 或飞书/钉钉/微信 Bot 远程监管
- **替代 Claude Cowork**：跨平台、多模型、免费开源

**不太适合**：只需极简单模型聊天、不需要文件操作/自动化的场景；或偏好纯终端工作流且不需要 GUI 的用户（可考虑 [[50 来源资料/代码仓库/AI/Luvus - AI Agent 任务控制中心|Luvus]]）。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/iOfficeAI/AionUi |
| **官网 / 下载** | https://www.aionui.com |
| **中文 README** | https://github.com/iOfficeAI/AionUi/blob/main/docs/readme/readme_ch.md |
| **Wiki（安装/配置）** | https://github.com/iOfficeAI/AionUi/wiki |
| **AionCore 后端** | https://github.com/iOfficeAI/AionCore |
| **aionrs Agent 引擎** | https://github.com/iOfficeAI/aionrs |
| **OfficeCLI** | https://github.com/iOfficeAI/OfficeCLI |
| **Discord 社区** | https://discord.gg/2QAwJn7Egx |
| **GitHub Discussions** | https://github.com/iOfficeAI/AionUi/discussions |
| **远程访问教程（中文）** | https://github.com/iOfficeAI/AionUi/wiki/Remote-Internet-Access-Guide-Chinese |

## My takeaways

1. **Cowork 而非 Chat**：定位是本地 AI 同事，文件操作、多步任务、定时自动化是一等公民，不是聊天套壳。
2. **双层 Agent 架构**：内置 aionrs 引擎降低门槛；多 Agent 模式兼容已有 CLI 生态，与 Luvus（终端编排）形成互补——AionUi 偏 GUI Cowork，Luvus 偏 tmux 式终端 Mission Control。
3. **OpenClaw 协同**：内置 OpenClaw 设置助手，且 README 明确支持 OpenClaw 作为外部 Agent；与 [[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw]] 的 IM 入口 + Skill 生态可组合使用。
4. **Office 产出链路**：OfficeCLI 驱动 PPT/Word/Excel 生成，配合预览面板，适合非开发者的 AI 办公场景。
5. **下载渠道变更**：v2.1.61 起安装包改从官网分发，查资源时优先记官网而非 GitHub Releases。

## Related

- [[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[50 来源资料/代码仓库/AI/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
