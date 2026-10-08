---
id: source-20261008-raven
title: Raven - 自进化多 Agent Harness 编排生态
type: source
status: active
created: 2026-10-08
updated: 2026-10-08
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/多智能体编排
  - 主题/Agent框架
  - 主题/Harness
author: EverMind AI
source_type: repo
source_url: https://github.com/EverMind-AI/Raven
source_author: EverMind-AI
source_date: 2026-10-08
summary: EverMind 的 Host Agent / Harness of Harnesses：为复杂任务生成 DAG，编排内置与第三方专业 Agent；基于 EverOS 跨会话记忆，支持 harness 自进化（Evolver/Curator）、SkillForge 技能检索与主动行为；含 Research/Code/Design/Oncall 四内置 Agent。
related:
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/JIT-Agent - 即时生成 Agent Harness|JIT-Agent - 即时生成 Agent Harness]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]"
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/Edict - 三省六部制 Multi-Agent 编排|Edict - 三省六部制 Multi-Agent 编排]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]"
---

# Raven - 自进化多 Agent Harness 编排生态

## Source summary

**Raven** 是 EverMind 打造的 **Harness of Harnesses（harness 之上的 harness）**，定位为面向递归自改进（RSI）的 **Host Agent**：用统一入口汇聚内置与第三方专业 Agent，为复杂任务生成 DAG、委派协作并整合结果。运行时由 [EverOS](https://github.com/EverMind-AI/EverOS) 驱动跨会话记忆；模块化架构支持对自身 harness（规划/行动方式）提出改动、评估并采纳通过验证的改进。

- 开源项目，GitHub：[EverMind-AI/Raven](https://github.com/EverMind-AI/Raven)
- 组织/生态：[EverMind](https://evermind.ai/)（记忆研究 → 产品 → Agent 集成）
- 阶段：**pre-alpha**（接口与配置可能快速变化）
- 许可证：**Apache License 2.0**
- 技术报告：[arXiv:2609.33439](https://arxiv.org/abs/2609.33439)
- 官网：[raven.evermind.ai](https://raven.evermind.ai) · 文档：[evermind-ai.github.io/Raven](https://evermind-ai.github.io/Raven/)（[中文文档](https://evermind-ai.github.io/Raven/zh/)）

一句话：**一个入口连接所有 Agent**——生成 DAG、编排多专家，并可持续改进自己的 harness。

## 解决什么问题

| 痛点 | Raven 方案 |
|------|-----------|
| 单 Agent 难扛跨域复杂交付 | Host Agent + DAG，编排 Research/Code/Design/Oncall 等专业子 Agent |
| 多工具/多 CLI Agent 割裂 | ACP / CLI / OpenAI 兼容 API 统一接入，内置约 13 个第三方 Agent 预设 |
| 会话结束即丢上下文 | EverOS 记忆跨会话保留用户上下文、经验与可复用技能 |
| Harness 僵化、难迭代 | Evolver / Curator：诊断失败 → 试改 → 基准验证后保留；运行时四策略模块可被改写 |
| 技能与专业知识难按需加载 | SkillForge 从本地库、记忆与 SkillHub（SkillCorpus，号称十万级技能）检索 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **Agent 编排** | 任务依赖、并行执行，多步协作沉淀为可复用工作流 |
| **内置四 Agent** | Raven-Research（深度研究）、Raven-Code（编程/数据分析）、Raven-Design（PPT/视觉/界面）、Raven-Oncall（无人值守流程） |
| **第三方 Agent 连接** | ACP / CLI / OpenAI 兼容 API；统一界面委派与协作 |
| **Evolver** | 独立工具，以库方式调用 Raven，在基准上评估 harness 候选改动 |
| **运行时自进化（Curator）** | 改写 Memory / Planning / Capability / Action 四席；装配前校验；实验性、随仓库提供 |
| **Persona / 数字人** | 描述所需助手 → Curator 组装主角色 + 专家分工与 harness 约束 |
| **EverOS 记忆** | 跨会话召回上下文与技能 |
| **SkillForge** | 按需检索技能（含 [SkillCorpus / SkillHub](https://github.com/EverMind-AI/SkillCorpus)） |
| **主动行为（Proactivity）** | 事件监测 + 定时执行，提醒与跟进 |
| **WebUI** | `raven web`：对话、协作进度、工作区、记忆与技能一站式浏览 |

## 内置 Agent 一览

| Agent | 定位 |
|-------|------|
| **Raven-Research** | 自主深度研究、文献/技术分析；结构化、来源可追溯报告 |
| **Raven-Code** | 需求→可测代码；实现、调试、重构、数据处理与分析 |
| **Raven-Design** | PPT、品牌物料、图表、示意图、Web 界面；布局与视觉一致性 |
| **Raven-Oncall** | 实验/调优/监控类无人值守工作流；长时间运行，关键节点才找人 |

## Showcase（README 案例）

| 案例 | 要点 |
|------|------|
| **THRESHOLD** | 约 4 天、42 轮规划/开发/验收；Godot 4 竞技场 Boss FPS；游戏+海报+演示+官网 |
| **Raven RSI** | 给定不可改评估标准的递归自改进；nanochat 预训练等实验全流程交付 |
| **产品发布包** | 浏览器物理小游戏、16 页产品介绍、中英海报与 README 等由 Raven 产出 |

更多见仓库 [`docs/showcase.md`](https://github.com/EverMind-AI/Raven/blob/main/docs/showcase.md)。

## 核心系统速查

| 系统 | 作用 |
|------|------|
| Agent Orchestration | 协调 Agent、依赖与并行、可复用工作流 |
| Evolver | harness 自进化评估与择优 |
| EverOS Memory | 跨会话记忆与技能召回 |
| SkillForge | 技能检索与按需增强 |
| Proactivity | 事件/定时驱动的主动跟进 |

## 快速开始

**让本机 Agent 代装（推荐提示词）：**

```text
阅读 https://evermind-ai.github.io/Raven/zh/quick-start/ 并按步骤安装 Raven；若已安装则更新。
```

**一键安装：**

```bash
# Linux / macOS / WSL2
curl -fsSL https://raven.evermind.ai/install.sh | bash
```

```powershell
# Windows PowerShell
irm https://raven.evermind.ai/install.ps1 | iex
# 若 5.1 拒绝重定向：
irm https://raw.githubusercontent.com/EverMind-AI/Raven/refs/heads/main/install.ps1 | iex
```

**Docker（宿主机只需 Git + Docker）：**

```bash
git clone https://github.com/EverMind-AI/Raven.git
cd Raven
docker compose -f docker/docker-compose.yml up
# 浏览器打开 http://localhost:18793
```

**WebUI：**

```bash
raven web
# 停止：raven web --stop
```

源码可编辑安装见仓库 `install.sh` / 文档；管道安装默认装发布版 wheel，避免误用脏工作树。

## EverMind 生态关联（便于后续补笔记）

| 项目 | 关系 |
|------|------|
| [EverOS](https://github.com/EverMind-AI/EverOS) | 本地优先、Markdown 原生长期记忆运行时（Raven 依赖） |
| [SkillCorpus](https://github.com/EverMind-AI/SkillCorpus) | 可检索技能语料 / SkillHub |
| [EverMe](https://evermind.ai/) | 跨设备/跨 Agent 个人记忆 CLI 与插件 |
| OpenClaw / Hermes / DeepSeek Harness / Dify | 生态内记忆集成插件（与已有 OpenClaw 笔记可联动） |

## 适用场景

- 需要 **Host Agent** 统一编排研究、编码、设计、无人值守实验等多域交付
- 关注 **harness 自进化 / RSI**、Curator 四模块改写，或与 [[50 来源资料/代码仓库/AI/Agent运行时与编排/JIT-Agent - 即时生成 Agent Harness|JIT-Agent]]（即时生成 harness）对照
- 已有 Claude Code / Codex 等 CLI，想通过统一表面接入并做 DAG 协作
- 需要 **跨会话记忆 + 大规模技能检索** 的长期 Agent 工作台（WebUI）

**不太适合**：只要轻量单 Agent 对话；对 pre-alpha API 稳定性要求极高的生产强依赖；不想引入 EverMind/EverOS 记忆栈的极简方案（可看 [[50 来源资料/代码仓库/AI/Agent运行时与编排/Luvus - AI Agent 任务控制中心|Luvus]] / [[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi]] 等不同定位）。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/EverMind-AI/Raven |
| **官网** | https://raven.evermind.ai |
| **文档（英文）** | https://evermind-ai.github.io/Raven/ |
| **文档（中文）** | https://evermind-ai.github.io/Raven/zh/ |
| **快速开始（中文）** | https://evermind-ai.github.io/Raven/zh/quick-start/ |
| **中文 README** | https://github.com/EverMind-AI/Raven/blob/main/README.zh-CN.md |
| **技术报告 (arXiv)** | https://arxiv.org/abs/2609.33439 |
| **EverOS** | https://github.com/EverMind-AI/EverOS |
| **SkillCorpus** | https://github.com/EverMind-AI/SkillCorpus |
| **EverMind 主站** | https://evermind.ai/ |
| **Discussions** | https://github.com/EverMind-AI/Raven/discussions |
| **Issues** | https://github.com/EverMind-AI/Raven/issues |
| **License** | Apache-2.0 |

## My takeaways

1. **定位清晰**：不是又一个单领域 coding agent，而是「编排 harness 的 Host」——与 JIT-Agent（生成任务专属 harness）、Edict（制度化三省六部）、AionUi（桌面 Cowork GUI）形成互补光谱。
2. **记忆与技能是一等公民**：EverOS + SkillForge 把长期上下文和大技能库接到编排层，适合做「可沉淀」的多日复杂项目。
3. **自进化分两层**：Evolver（离线/研发向基准评估）与 Curator（运行时改四策略模块）；Curator 仍实验性，跟进时注意是否进正式安装包。
4. **交付叙事强**：README Showcase 强调端到端完整产物（游戏/实验/发布包），评估时建议同时看基准图与真实交付链路。
5. **生态可扩展笔记**：EverOS、SkillCorpus 等尚未入库；若后续深挖记忆栈，可在「RAG知识与记忆」另建来源笔记并与本页互链。
6. **成熟度提醒**：pre-alpha，接入生产或长期自动化前先锁定版本并跟文档变更。

## Related

- [[50 来源资料/代码仓库/AI/Agent运行时与编排/JIT-Agent - 即时生成 Agent Harness|JIT-Agent - 即时生成 Agent Harness]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/Edict - 三省六部制 Multi-Agent 编排|Edict - 三省六部制 Multi-Agent 编排]]
- [[50 来源资料/代码仓库/AI/Agent运行时与编排/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
