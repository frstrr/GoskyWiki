---
id: source-20260901-edict
title: Edict - 三省六部制 Multi-Agent 编排
type: source
status: active
created: 2026-09-01
updated: 2026-09-01
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/多智能体编排
  - 主题/工作流自动化
author: cft0808
source_type: repo
source_url: https://github.com/cft0808/edict
source_author: cft0808
source_date: 2026-09-01
summary: 基于 OpenClaw 的「三省六部」多 Agent 编排系统：太子分拣、中书规划、门下审核封驳、尚书派发、六部并行执行；军机处实时看板、完整审计、模型热切换与 Skills 管理。
related:
  - "[[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[50 来源资料/代码仓库/AI/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]"
  - "[[50 来源资料/代码仓库/AI/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
---

# Edict - 三省六部制 Multi-Agent 编排

## Source summary

**Edict（三省六部）** 用中国古代三省六部制度重新设计 AI 多 Agent 协作：分权制衡、专职审核、全程可观测、可干预。构建在 [[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw]] 之上，通过飞书 / Telegram / Signal 等渠道「下旨」，由 12 个专职 Agent 完成规划→审议→派发→执行→回奏。

- 开源项目，GitHub：[cft0808/edict](https://github.com/cft0808/edict)
- 语言：**Python**（看板后端 + 编排脚本）+ **TypeScript/React**（军机处前端）
- 约 **16,700+ stars**（2026-09-01）；约 **1,700+ forks**
- 许可证：**MIT**
- 依赖运行时：**OpenClaw**（完整安装）；Docker 可单独体验看板 Demo
- 平台：安装脚本面向 **macOS / Linux**；看板 Demo 可用 Docker

## 解决什么问题

多数 Multi-Agent 框架是「几个 Agent 自己聊完交结果」——难复现、难审计、难中途干预。

| 痛点 | Edict 方案 |
|------|-----------|
| 无强制质量关卡 | **门下省**专职审议，可封驳打回重做（架构级，非可选插件） |
| 过程黑盒 | **军机处看板**实时看状态、流转链、Agent 心跳与 thinking |
| 无法中途干预 | 叫停 / 取消 / 恢复任务 |
| 协作无制度边界 | 权限矩阵：谁能给谁发消息白纸黑字；状态机拒绝非法跳转 |
| 缺少审计 | 奏折归档 + 五阶段时间线 + audit 日志 |
| 模型/技能难管 | 看板内按 Agent 热切换 LLM；Skills 可 UI/CLI/API 增补 |

对比 CrewAI / MetaGPT / AutoGen：差异点是 **制度性审核 + 完全可观测 + 实时可干预**。

## 核心能力

| 能力 | 说明 |
|------|------|
| **十二部制 Agent** | 太子分拣 + 三省（中书/门下/尚书）+ 七部（户礼兵刑工吏 + 早朝官） |
| **门下省封驳** | 审查规划质量；不合格直接打回，强制返工循环 |
| **军机处看板** | 旨意 Kanban、省部调度、奏折阁、旨库、官员总览、天下要闻、模型/技能配置、会话监控、上朝仪式、朝堂议政 |
| **状态机保护** | `kanban_update.py` 校验合法转换路径，非法跳转拒绝并记日志 |
| **异步事件总线** | Redis Streams EventBus + Outbox Relay，状态变更可审计 |
| **并行调度** | Dispatch Worker：并行执行、指数退避重试、资源锁；DAG 编排依赖 |
| **模型热切换** | 每个 Agent 独立 LLM，应用后约 5 秒重启 Gateway 生效 |
| **Skills 管理** | 看板 / CLI / API 从 GitHub 或 URL 添加远程 Skill，可版本更新 |
| **圣旨模板** | 9 个预设（周报、代码审查、API 设计、竞品分析等） |
| **新闻聚合** | 天下要闻：科技/财经采集 + 分类订阅 + 飞书推送 |

## Agent 角色一览

| 部门 | Agent ID | 职责 |
|------|----------|------|
| 太子 | `taizi` | 消息分拣：闲聊直回 / 旨意建任务 |
| 中书省 | `zhongshu` | 接旨、规划、拆解子任务 |
| 门下省 | `menxia` | 审议、把关、封驳 |
| 尚书省 | `shangshu` | 派发、协调、汇总回奏 |
| 户部 | `hubu` | 数据、资源、核算 |
| 礼部 | `libu` | 文档、规范、报告 |
| 兵部 | `bingbu` | 代码、算法、巡检 |
| 刑部 | `xingbu` | 安全、合规、审计 |
| 工部 | `gongbu` | CI/CD、部署、工具 |
| 吏部 | `libu_hr` | 人事、Agent 管理 |
| 早朝官 | `zaochao` | 每日早朝、新闻聚合 |

流程：`皇上 → 太子分拣 → 中书规划 → 门下审议 → 尚书派发 → 六部执行 → 回奏`（门下可封驳回中书）。

## 技术栈要点

| 层 | 技术 |
|----|------|
| 前端 | React 18 + TypeScript + Vite + Zustand |
| 看板服务 | Python `http.server` 零依赖（API + 静态）；另有 SQLAlchemy + Redis 异步后端 |
| 编排 | 状态机、EventBus、Outbox、Dispatch/Orchestrator Worker |
| 集成 | OpenClaw Gateway / Workspace / Skills；飞书等 IM 下旨 |

## 安装与快速体验

**仅看板 Demo（无需 OpenClaw）：**

```bash
docker run -p 7891:7891 cft0808/sansheng-demo
# x86 若遇 exec format error：
docker run --platform linux/amd64 -p 7891:7891 cft0808/sansheng-demo
```

打开 http://localhost:7891 。

**完整安装（需已装 OpenClaw、Python 3.10+）：**

```bash
git clone https://github.com/cft0808/edict.git
cd edict
chmod +x install.sh && ./install.sh
chmod +x start.sh && ./start.sh
# 看板：http://127.0.0.1:7891
```

首次安装需先配置 API Key（如 `openclaw agents add taizi`），再跑 `install.sh` 同步到全部 Agent。生产可用 `edict.service` / `edict.sh` 做 systemd 托管。

## 适用场景

- 需要 **可审核、可审计、可干预** 的复杂多 Agent 任务流（开发、文档、运维、数据分析等）
- 已在用或计划用 **OpenClaw**，希望在 IM 里「下旨」并由多角色协作落地
- 想对照 CrewAI/AutoGen，研究 **制度化分权**（门下省封驳、权限矩阵、状态机）的编排范式
- 需要实时看板观察 Agent 健康、Token、thinking 与任务流转

**不太适合**：不想依赖 OpenClaw、只要单 Agent 对话、或不需要制度级审核与看板运维成本的轻量场景（可看 [[50 来源资料/代码仓库/AI/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi]] / [[50 来源资料/代码仓库/AI/Luvus - AI Agent 任务控制中心|Luvus]] 等定位不同的方案）。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/cft0808/edict |
| **项目主页（OpenClaw）** | https://openclaw.ai |
| **Docker Demo** | `cft0808/sansheng-demo`（端口 7891） |
| **快速上手** | https://github.com/cft0808/edict/blob/main/docs/getting-started.md |
| **任务分发架构（必读）** | https://github.com/cft0808/edict/blob/main/docs/task-dispatch-architecture.md |
| **远程 Skills 指南** | https://github.com/cft0808/edict/blob/main/docs/remote-skills-guide.md |
| **Roadmap** | https://github.com/cft0808/edict/blob/main/ROADMAP.md |
| **贡献指南** | https://github.com/cft0808/edict/blob/main/CONTRIBUTING.md |
| **Issues** | https://github.com/cft0808/edict/issues |

## My takeaways

1. **制度隐喻落地为工程约束**：权限矩阵 + 门下封驳 + 状态机，不是皮肤，而是强制质量关卡与不可绕过流程。
2. **强依赖 OpenClaw**：完整能力建立在 OpenClaw Workspace/Gateway/Skills 上；与已有 [[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw]] 笔记应联动查阅。
3. **可观测是一等公民**：军机处看板覆盖任务、Agent、模型、技能、审计，适合做复杂协作的「指挥台」。
4. **Docker Demo 与完整安装分离**：无 OpenClaw 时可先跑看板体感；真用需安装脚本 + API Key 同步。
5. **与 AionUi/Luvus 互补**：AionUi 偏桌面 Cowork GUI，Luvus 偏终端多 Agent Mission Control，Edict 偏「OpenClaw 上的制度化编排 + IM 下旨 + 审核闭环」。

## Related

- [[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[50 来源资料/代码仓库/AI/AionUi - 开源 Multi-AI Agent Cowork 桌面应用|AionUi - 开源 Multi-AI Agent Cowork 桌面应用]]
- [[50 来源资料/代码仓库/AI/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]
- [[50 来源资料/代码仓库/AI/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
