---
id: source-20261110-gpt6-astra-coldcoffee
title: 冷咖啡 gpt6-Astra - 多 Agent 破甲工作台
type: source
status: active
created: 2026-11-10
updated: 2026-11-10
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/AI编程Agent
  - 主题/桌面应用
  - 主题/开源资源
  - 主题/Skills
aliases:
  - 冷咖啡
  - coldcoffee
  - BREAK//OPEN
  - gpt6-Astra 破甲工作台
summary: 冷咖啡（仓库名 gpt6-Astra）是跨平台桌面「破甲」工作台：一键向约十五个 AI 编程 Agent 写入开源技能包（写前备份、可卸载），并提供付费 API 中转可视化、原创工具箱与社群入口。与 Tripo 的 GPT-6 Astra 3D 模型无关。
author: 3641397194-wq
source_type: repo
source_url: https://github.com/3641397194-wq/gpt6-Astra
source_author: 3641397194-wq
source_date: 2026-11-10
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA - Agent 逆向分析 MCP]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/dsh-game-studio - DSH 游戏工坊插件|dsh-game-studio - DSH 游戏工坊插件]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/Ponytail - AI Agent 少写代码规则插件|Ponytail - AI Agent 少写代码规则插件]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/awesome-astra-prompts - GPT-6 Astra 提示词精选|awesome-astra-prompts - GPT-6 Astra 提示词精选]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# 冷咖啡 gpt6-Astra - 多 Agent 破甲工作台

## Source summary

**冷咖啡**（GitHub 仓库名 [`3641397194-wq/gpt6-Astra`](https://github.com/3641397194-wq/gpt6-Astra)，亦称 **BREAK//OPEN**）是一个跨平台桌面工作台：勾选目标 AI 客户端后，一键写入开源技能包与配置（写前备份、写后校验、可一键卸载），并附带「冷咖啡中转」可视化路由、原创工具箱与社群入口。

- 约 **2,097 stars** / **448 forks**（2026-11-10 GitHub API）
- 主语言 **JavaScript**（另有 CSS / Python / HTML）；许可证 **MIT**
- 当前宣传版本：**3.2.0-preview.9**（Windows / macOS / Linux 安装包）
- 官网 / 中转：**[coldcoffeeai.com](https://coldcoffeeai.com)**
- Topics：`jailbreak`、`prompt-engineering`、`security-research`、`claude`、`deepseek`、`grok` 等

**命名易混**：本仓库席位里的「GPT-6 Astra」指写入 `~/.codex` 一类 **Codex 系编程 Agent** 的席位，**不是** Tripo 的 3D 生成模型 GPT-6 Astra。后者见 [[50 来源资料/代码仓库/AI/Skills与方法论/awesome-astra-prompts - GPT-6 Astra 提示词精选|awesome-astra-prompts]]。

## 解决什么问题

| 痛点 | 冷咖啡方案 |
|------|------------|
| 多个 AI 客户端各自装 Skill / 规则，操作分散 | 十五个席位集中勾选，一次备份写入 |
| 改坏配置难回退 | 写前备份；卸载按备份恢复 |
| 多中转 / 主备路由不透明 | 「冷咖啡中转」页展示真实请求时间线、主备切换与上下文 |
| 需要本机小工具 + 逆向联动 | 内置工具箱；可写 IDA MCP 连接说明（`127.0.0.1:13337`） |

## 核心能力

| 能力 | 说明 |
|------|------|
| **一键破甲（开源）** | 只处理勾选项；备份 → 安装 → 核验；附带完整技能扩展（README 称约 94 项） |
| **一键卸载** | 勾选席位退回写入前备份 |
| **冷咖啡中转** | 基址 `https://coldcoffeeai.com/v1`；本机粘贴 Key；可视化融合、多分支路由、嵌套组 |
| **十五个 AI 席位** | Codex/Astra、Claude Code、Grok、DeepSeek Harness、GLM、Gemini、豆包、WorkBuddy、Cursor、MiniMax、Kimi、Grok Bot、Claude Duet、ChatGPT Dot、Meta Muse 等 |
| **DeepSeek Harness 插件** | 「冰蓝」皮肤与助手；复用 REA / reverse-skill；六个研究入口（安全检查、审计、REA、源码审计等） |
| **Grok Bot 记忆管理** | 读取/搜索/预览确认后写入或忘掉 Bot 云端记忆 |
| **工具箱** | 网络/网站/文件/文本工具；IDA Pro 中文界面插件指引；MCP/Skill 联动 |
| **社群** | 四个 QQ 群 + Telegram；闭源/定制破甲走群管理（不在本仓库开通） |

## 四页结构

| 页 | 做什么 |
|----|--------|
| 01 一键破甲 | 勾选席位 → 备份写入 / 卸载恢复 |
| 02 冷咖啡中转 | 填 Key、选模型、预览 Codex 配置、看路由 |
| 03 冷咖啡社群 | QQ / Telegram、售后与定制入口 |
| 04 工具箱 | 原创工具与 IDA 相关能力 |

## 支持席位（摘要）

写入目标均为各客户端**用户数据目录**下的 Skill / 规则文件（如 `prompts/`、`CLAUDE.md`、`skills/`），不改安装目录本身（README 对 Cursor / MiniMax / Kimi 等有「不写」清单）。

| # | 席位 | 典型数据根（示意） |
|---|------|-------------------|
| 01 | GPT-6 Astra（Codex 系） | `~/.codex` |
| 02 | Claude Code | `~/.claude` |
| 03 | Grok 4.7 | `~/.grok` |
| 04 | DeepSeek v4.1 Flash / Harness | `~/.dsh` |
| 05 | GLM 5.3 | `~/.glm` 或 `~/.zcode` |
| 06 | Gemini | `~/.gemini` |
| 07–15 | 豆包、WorkBuddy、Cursor、MiniMax、Kimi、Grok Bot、Claude Duet、ChatGPT Dot、Meta Muse | 见仓库 README 表 |

完整路径与文件名以仓库 README「十五个 AI 模型」为准。

## 上手路径（高层）

1. 从 [最新版下载页](https://github.com/3641397194-wq/gpt6-Astra/releases) 取对应系统安装包（或按 README 自行 `npm ci` / `pack:*`）。
2. 打开工作台 → 只勾需要的席位 → 一键写入（先备份）。
3. 重启目标客户端；按官方说明用口令触发已装技能（具体口令见仓库 README）。
4. 不需要时回到工作台一键卸载，恢复备份。

开发者本地跑桌面端（README）：

```bash
cd desktop
npm ci
npm start
```

## 合规与风险（必读）

- **官方免责声明**：严禁用于违法违纪；仅供合法合规学习、研究及本人有权操作的环境。禁止未授权入侵/渗透/窃取、诈骗、传播恶意程序、违法内容、侵犯隐私与知识产权，以及违反当地法律或第三方平台/模型服务条款。
- **本笔记定位**：开源资源索引与能力摘要，**不收录**破甲词包原文、绕过安全策略的逐步攻击教程或未授权用途指引。
- **服务条款**：改写各商业 AI 客户端行为与使用第三方中转，可能违反对应产品 ToS；自行评估账号与合规风险。
- **备份仍可能丢数据**：卸载依赖写前备份；异常中断或手动改目录后，恢复不一定完整。
- **闭源/定制与付费中转**：不在本开源仓开通；Key 与商业服务见官网与社群。

## 与库内相关条目

| 条目 | 关系 |
|------|------|
| [[50 来源资料/代码仓库/AI/Skills与方法论/awesome-astra-prompts - GPT-6 Astra 提示词精选\|awesome-astra-prompts]] | **不同项目**：Tripo 3D 提示词策展；勿与本工作台混淆 |
| [[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP\|REA]] | DeepSeek Harness 冰蓝插件 README 写明复用 REA / reverse-skill |
| [[50 来源资料/代码仓库/AI/Agent工具与中间件/dsh-game-studio - DSH 游戏工坊插件\|dsh-game-studio]] / [[50 来源资料/代码仓库/AI/Agent工具与中间件/dsh-blender - DSH Blender 3D 生产插件\|dsh-blender]] | 同属 DeepSeek Harness（DSH）生态插件线 |
| [[50 来源资料/代码仓库/AI/Agent工具与中间件/Ponytail - AI Agent 少写代码规则插件\|Ponytail]] | 同为「给多客户端写规则/技能」思路，但定位与交付形态不同 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/3641397194-wq/gpt6-Astra |
| **官网 / 购 Key** | https://coldcoffeeai.com |
| **API 基址** | https://coldcoffeeai.com/v1 |
| **Releases / 下载** | https://github.com/3641397194-wq/gpt6-Astra/releases |
| **作者** | https://github.com/3641397194-wq |
| **Telegram 群** | https://t.me/coolcoffeeai |
| **Telegram 频道** | https://t.me/chachachalkf |
| **QQ 群（README）** | 1057540028 · 1077074552 · 618179023 · 1131239659 |
| **许可证** | MIT |

## My takeaways

1. **本质是多客户端 Skill/配置安装器 + 中转可视化桌面壳**，不是又一个独立 Coding Agent 运行时。
2. **仓库名极易与 Tripo GPT-6 Astra 撞车**；检索时用「冷咖啡 / coldcoffee / BREAK//OPEN」更准。
3. **开源部分侧重可逆写入**（备份/卸载）；闭源破甲与定制走社群商业路径。
4. **和 REA / DSH 插件生态有交叉**，做 DeepSeek Harness 或逆向联动调研时可并列对照。
5. **合规优先**：当资源索引即可；是否安装、是否使用中转，按本机授权与各平台条款自行判断。

## Related

- [[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA - Agent 逆向分析 MCP]]
- [[50 来源资料/代码仓库/AI/Agent工具与中间件/dsh-game-studio - DSH 游戏工坊插件|dsh-game-studio - DSH 游戏工坊插件]]
- [[50 来源资料/代码仓库/AI/Agent工具与中间件/dsh-blender - DSH Blender 3D 生产插件|dsh-blender - DSH Blender 3D 生产插件]]
- [[50 来源资料/代码仓库/AI/Agent工具与中间件/Ponytail - AI Agent 少写代码规则插件|Ponytail - AI Agent 少写代码规则插件]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/awesome-astra-prompts - GPT-6 Astra 提示词精选|awesome-astra-prompts - GPT-6 Astra 提示词精选]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
