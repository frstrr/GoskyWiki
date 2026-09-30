---
id: source-20260831-openmaic
title: OpenMAIC - 开源多智能体交互课堂
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI教育
  - 主题/多智能体
  - 主题/Agent
author: 清华 THU-MAIC 团队
source_type: repo
source_url: https://github.com/THU-MAIC/OpenMAIC
source_author: THU-MAIC
source_date: 2026-08-31
summary: 清华大学 THU-MAIC 团队开源的 AI 教育平台，将书籍、论文、PDF 等材料自动转化为可播放、可交互、带 AI 配音的多智能体互动课堂。v1.0 升级为 Agent 形态，内置 20 个 skill，GitHub 21k+ Stars，MIT 协议。
related:
  - "[[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[50 来源资料/代码仓库/AI/VoxCPM - 语音克隆 TTS|VoxCPM - 语音克隆 TTS]]"
article_source: https://www.toutiao.com/article/7679437901129613864/
---

# OpenMAIC - 开源多智能体交互课堂

## Source summary

**OpenMAIC**（Open Multi-Agent Interactive Classroom）是清华大学 THU-MAIC 团队开发的**开源 AI 教育平台**。核心理念：把读不下去的书、啃不动的论文，自动做成适合你的互动课程。

- **~21k Stars**（2026-08 头条文章数据）
- **MIT 协议**开源（v0.3.0 起由 AGPL-3.0 改为 MIT）
- **2026-03** 首次开源，**2026-08-27** 发布 **v1.0.0** 正式版
- 已在清华 **700+ 真实学生**中验证
- 技术栈：Next.js + LangGraph 多智能体编排 + 可插拔 LLM Provider

一句话概括：**你扔一份材料进去，它自己读完、查资料、设计课程结构，输出一套能播放、能交互、带 AI 配音的完整课件。**

## 核心能力

| 能力 | 说明 |
|------|------|
| **Agent 工作台（v1.0）** | 对话式课程构建：规划大纲、逐页生成与修订、会话可恢复、支持中途 steer |
| **一键课件生成** | 描述主题或上传 PDF/Office/Markdown/音视频，AI 自动编排 AI 教师、助教、同学 |
| **多场景类型** | 幻灯片、测验、交互 HTML 仿真、项目式学习（PBL）、白板公式、TTS 讲解 |
| **深度交互模式** | 3D 可视化、物理仿真（滑块调参、动画联动）、在线编程等 |
| **系列课规划** | `curriculum-planner` skill 一次规划完整系列课（如 7 天入门大模型），非单页拼凑 |
| **PPT 高保真导入** | `pptx-import` skill 导入已有 PPT，保留排版并自动配中文旁白 |
| **课件内答疑** | 播放过程中直接提问，Agent 联网核实后分层回答，无需切出上下文 |
| **导出** | 可编辑 `.pptx`、交互 `.html`、可选 MP4 视频导出 |
| **声音克隆** | 集成 [[50 来源资料/代码仓库/AI/VoxCPM - 语音克隆 TTS|VoxCPM]]，课件可用自己的声音讲解 |
| **OpenClaw 集成** | 通过 [[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw]] 从飞书/Slack/Telegram 等直接生成课堂 |

## v1.0 关键升级（相对早期 Workflow 模式）

早期版本依赖**固定 Workflow**——按预设剧本走，实时生成课件、AI 老师讲课、AI 同学讨论。

v1.0 全面升级为 **Agent 形态**：

- 用户描述需求，Agent **自行决定**调用哪个 skill、是否联网核实、课程如何分解、用什么教学策略
- **PRO 专业模式**：扔 PDF 进去，读材料 → 联网核实 → 选教学策略，全程自动跑完
- **20 个内置 skill**（PRO 模式 18+）：系列课规划、深度交互、深度调研、大师讲授、页面克隆、PPT 导入、职业实训、互动工作坊等
- Skill 可下载，导入 **WorkBuddy** 或 **Codex** 本地使用

## 文章实测案例（来源：头条 7679437901129613864）

| 案例 | 输入材料 | 输出亮点 |
|------|----------|----------|
| 书籍精读 | Salman Khan《Brave New Words》样章 PDF | 12 次读材料 + 2 次联网，10 页课程，3 核心命题 + 课后练习 |
| 物理课 | MIT OCW 8.02 电磁学 Faraday's Law 讲义 | 6 页课程，磁通量交互仿真（B/A/θ 滑块联动）、磁铁穿线圈实时显示 ε/I/Φ |
| 论文精读 | Kimi K3 技术报告 | 20+ 次读材料 + 多次联网核实，11 页课程，K2 vs K3 对比表 + 架构三轴拆解 |
| 系列课 | 「7 天入门大模型」 | 完整 7 天大纲 → 自动创建系列文件夹 → 逐节生成 Day 1–7，递进关系明确 |
| PPT 导入 | MIT 12.010 第一讲真实课件 | 8 页高保真导入，封面/结构图/代码块/分栏均保留，自动配中文旁白 |

## 内置 Skill 体系（部分）

| Skill | 用途 |
|-------|------|
| `curriculum-planner` | 一次规划整个系列课程 |
| `pptx-import` | 已有 PPT 高保真导入 |
| `deep-interactive` | 深度交互仿真页面 |
| `deep-research` | 深度调研与核实 |
| `master-lecture` | 大师讲授风格 |
| `page-clone` | 页面克隆 |
| `vocational-training` | 职业实训 |
| `interactive-workshop` | 互动工作坊 |

Skill 安装方式：

```bash
# OpenClaw / ClawHub
clawhub install openmaic

# 手动复制
mkdir -p ~/.openclaw/skills
cp -R /path/to/OpenMAIC/skills/openmaic ~/.openclaw/skills/openmaic
```

也可导入 WorkBuddy（`~/.workbuddy/skills/`）或 Codex 使用。

## 部署模式

| 模式 | 说明 |
|------|------|
| **Hosted 托管** | 在 [open.maic.chat](https://open.maic.chat/) 获取 `sk-xxx` 格式访问码，无需本地部署，消耗账号每日免费额度 |
| **Self-hosted 本地** | Git clone 后本地运行，可接本地模型（Ollama/Lemonade），数据不出内网 |
| **Docker** | 仓库自带 Dockerfile 和 docker-compose，支持 PostgreSQL 持久化 |
| **Vercel** | README 提供一键 Deploy 按钮 |

## 快速开始

```bash
git clone https://github.com/THU-MAIC/OpenMAIC.git
cd OpenMAIC
pnpm install
cp .env.example .env.local
# 填入至少一个 LLM Provider API Key
pnpm dev
# 打开 http://localhost:3000
```

**环境要求**：Node.js >= 20，pnpm >= 10

**推荐模型**：Gemini 3 Flash（质量与速度平衡）；追求最高质量用 Gemini 3.1 Pro

## 支持的 LLM Provider

OpenAI、Azure OpenAI、Anthropic、Amazon Bedrock、Google Gemini、DeepSeek、Qwen、Kimi、MiniMax、Grok、OpenRouter、Doubao、腾讯混元、小米 MiMo、GLM（智谱）、Ollama（本地）、Lemonade（本地 LLM/图像/TTS/ASR）、FunASR（本地 ASR）及任意 OpenAI 兼容 API。

## 适用场景

### 更适合

- 想把**书籍/论文/讲义**快速做成互动课程的教师、研究者
- 需要**系列课规划**而非单页课件的内容创作者
- 学校/企业需要**本地部署 + 数据不出内网**的 AI 教育方案
- 已有 PPT 课件，想加 AI 配音和交互升级为可播放课程
- 通过飞书/Slack 等 IM 远程触发课程生成（OpenClaw 集成）

### 不太适合

- 只需简单 PPT 生成、不需要多智能体互动的场景
- 无法配置任何 LLM API Key 且不使用托管模式的场景
- 对课件视觉风格有严格品牌规范、不接受 AI 生成排版的场景

## 竞技场（Arena）

类似 Chatbot Arena，两个隐藏模型同时生成课件，用户投票选更好的，计入社区排行榜——但比较的是**课件生成质量**而非对话能力。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/THU-MAIC/OpenMAIC |
| **在线体验** | https://openmaic.chat/zh |
| **Hosted 托管** | https://open.maic.chat/ |
| **快速开始文档** | https://open.maic.chat/docs/getting-started |
| **部署文档** | https://open.maic.chat/docs/deployment |
| **ClawHub Skill** | `clawhub install openmaic` |
| **来源文章** | https://www.toutiao.com/article/7679437901129613864/ |

## My takeaways

1. **Agent 形态是 v1.0 最大变化**：从固定 Workflow 到自主决策调用 skill，处理复杂材料（论文、物理仿真）的能力明显更强。
2. **开源 skill 体系是核心资产**：20 个 skill 可独立下载用于 WorkBuddy/Codex/OpenClaw，不必绑定 OpenMAIC 网站。
3. **本地部署 + 本地模型**对企业/学校友好：数据不出内网，配合 Ollama/Lemonade 可完全离线运行（除联网核实时）。
4. **PPT 导入路径最短**：已有课件的老师直接导入 + AI 配音，比从零生成更实用。
5. **系列课规划 skill 价值高**：`curriculum-planner` 解决的是「7 个独立主题拼盘」问题，对做体系化课程的人更有用。
6. **生态关联**：OpenClaw（IM 触发）、VoxCPM（声音克隆）、WorkBuddy/Codex（skill 宿主）构成完整工具链。

## Related

- [[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[50 来源资料/代码仓库/AI/VoxCPM - 语音克隆 TTS|VoxCPM - 语音克隆 TTS]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
