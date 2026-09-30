---
id: source-20260831-agent-substrate
title: Agent Substrate - AI Agent 大规模运行时
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/基础设施
  - 主题/Kubernetes
author: Google（开源社区项目，非官方支持产品）
source_type: repo
source_url: https://github.com/agent-substrate/substrate
source_author: agent-substrate
source_date: 2026-08-31
summary: Google 开源的 AI Agent 大规模运行时，基于 Go + Kubernetes，通过 Actor/Worker 多路复用实现 30 倍超售、亚秒级挂起恢复与完整状态持久化，适合高密度 Agent 服务与多租户平台。
related:
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]"
---

# Agent Substrate - AI Agent 大规模运行时

## Source summary

**Agent Substrate** 是 Google 开源的 **AI Agent 大规模运行时基础设施**——不是 Agent SDK，而是跑 Agent 的「操作系统 / Agent 版 Kubernetes」。

- 开源项目，GitHub：`agent-substrate/substrate`
- 语言：**Go**；协议：**Apache 2.0**
- 基于 **Kubernetes** 管理 Worker Pod，在其上提供 Agent 专用调度与生命周期管理
- 核心亮点：约 **250 个 Agent 复用 8 个 Pod**（30 倍+ 超售）、**亚秒级挂起/恢复**、**内存 + 文件系统状态完整保留**
- 沙箱技术支持 **gVisor** 与 **microVM**
- 状态：**早期开发阶段**，API 不稳定，**不建议生产环境使用**（README 明确声明非 Google 官方支持产品）

## 解决什么问题

Agent 与常规 Web 服务不同：有独立状态、文件系统、终端会话，且 **大部分时间在空闲**（等人输入、等工具返回）。传统方案问题：

| 方式 | 问题 |
|------|------|
| 物理机 / VM 一 Agent 一机 | 资源利用率极低 |
| 普通容器密集部署 | 隔离不足，状态恢复不灵活 |
| 纯 Kubernetes 调度 | 不懂 Agent 空闲特性，无法高效挂起/恢复与状态持久化 |

Agent Substrate 的思路：把大量 **Actor**（Agent 实例）密集映射到少量 **Worker**（实际运行的 Pod），空闲时挂起、需要时瞬间恢复，状态不丢。

## 核心能力

| 能力 | 说明 |
|------|------|
| **多路复用（Multiplexing）** | 多个 Actor 共享同一 Worker，空闲 Actor 挂起让出资源 |
| **亚秒级 Suspend/Resume** | 基于 gVisor/microVM checkpoint/restore，配合增量快照、异步恢复、预加载 |
| **状态持久化** | 文件系统 + 内存（进程状态、fd）两层，跨休眠周期完整保留 |
| **框架无关** | 管理标准 OCI 容器，不绑定特定 Agent 框架 |
| **K8s 原生集成** | 用 CRD、Controller、Pod 等，在其上叠加 Agent 专用控制面 |

## 架构组件

```
控制面：ateapi (gRPC API) / atecontroller (K8s Controller) / atenet (DNS + Envoy 路由)
数据面：Worker Pod（内含 Actor），节点级 atelet (DaemonSet) 协调快照与状态传输
沙箱内：ateom-gvisor / ateom-microvm 执行 checkpoint/restore
```

| 组件 | 职责 |
|------|------|
| **ateapi** | 管理 Actor/Worker 生命周期（创建、挂起、恢复、销毁） |
| **atecontroller** | 调谐 WorkerPool、ActorTemplate 等 CRD |
| **atenet** | DNS、Envoy 路由、代理 Sidecar |
| **atelet** | 节点级监督，协调快照与状态传输 |
| **ateom-gvisor / ateom-microvm** | 沙箱内执行 checkpoint/restore |
| **kubectl-ate** | CLI 工具，管理 Atespace、Actor 等资源 |

核心概念：**Actor**（Agent 实例）、**Worker**（运行 Pod）、**Atespace**（命名空间/租户隔离）、**WorkerPool**、**ActorTemplate**。

## 框架兼容

| 框架 / 场景 | 说明 |
|-------------|------|
| **Agent Development Kit (ADK)** | 原生支持 ADK Actor 身份与持久化工作记忆 |
| **LangChain** | 适合长时间运行、有状态的 LangChain Agent |
| **Claude Code / Codex** | 高密度编码环境，保留终端与文件系统状态 |
| **MCP Server** | 将 MCP Server 作为持久化、沙箱隔离的工具服务部署 |

## 适用场景

- **大规模 Agent 服务**：万级用户各有一个 Agent 实例，显著降本（Demo：250 Actor / 8 Pod）
- **MCP Server 托管**：按需启动、空闲挂起、状态持久化
- **CI/CD 沙箱**：microVM 硬件级隔离 + 构建缓存跨运行保留
- **多租户 Agent 平台**：Atespace 隔离，底层统一调度

**不太适合**：个人开发者本地跑少量 Agent（直接用 Docker/本地运行即可，上手成本偏高）。

## 快速开始（本地 kind）

前置：Go、kubectl、Docker

```bash
# 1. 创建 kind 集群
hack/create-kind-cluster.sh

# 2. 安装 Agent Substrate
hack/install-ate-kind.sh --deploy-ate-system

# 3. 安装 Counter Demo
hack/install-ate-kind.sh --deploy-demo-counter

# 4. 安装 CLI
go install ./cmd/kubectl-ate

# 5. 创建 Atespace 和 Actor
kubectl ate create atespace demo
kubectl ate create actor my-counter-1 -a demo --template=ate-demo-counter/counter

# 6. 端口转发
kubectl port-forward -n ate-system svc/atenet-router 8000:80
```

验证（新终端）：

```bash
curl -X POST -H "Host: my-counter-1.demo.actors.resources.substrate.ate.dev" -i http://localhost:8000/
```

Counter Demo 演示挂起/恢复后计数器状态仍保留。

## Demo 列表

| Demo | 说明 |
|------|------|
| [Counter](https://github.com/agent-substrate/substrate/tree/main/demos/counter) | 有状态 HTTP 服务，演示 suspend/resume 与 CRD 路由 |
| [Sandbox (Antigravity)](https://github.com/agent-substrate/substrate/tree/main/demos/sandbox) | Alpine 沙箱，任意 shell 执行，文件系统状态保留 |
| [Claude Code Multiplex](https://github.com/agent-substrate/substrate/tree/main/demos/claude-code-multiplex) | 多 Claude Code 实例复用有限 Worker |
| [Multi-Template](https://github.com/agent-substrate/substrate/tree/main/demos/multi-template) | 多 ActorTemplate 共享 WorkerPool |
| [Request Parking](https://github.com/agent-substrate/substrate/tree/main/demos/parking) | 池饱和时路由层暂存请求而非 503 |
| [Autoscaled WorkerPool](https://github.com/agent-substrate/substrate/tree/main/demos/autoscaled-workerpool) | HPA 按 assigned-worker 自动扩缩 |

## 局限与注意事项

- **早期开发**：API 随时可能变，无向后兼容保证
- **强依赖 Kubernetes**：无 K8s 环境上手成本高
- **学习曲线陡**：Actor/Atespace/WorkerPool/ActorTemplate 等概念需时间理解
- **文档仍在完善**：高级用法可能需读源码

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/agent-substrate/substrate |
| **架构文档** | https://github.com/agent-substrate/substrate/blob/main/docs/architecture.md |
| **API 配置指南** | https://github.com/agent-substrate/substrate/blob/main/docs/api-guide.md |
| **术语表** | https://github.com/agent-substrate/substrate/blob/main/docs/glossary.md |
| **Demo 视频** | https://www.youtube.com/watch?v=ZEzkCFJkzjY |
| **Agent Executor（基于 Substrate 的分布式 Agent 运行时）** | https://github.com/google/ax |
| **社区 Google Group** | https://groups.google.com/g/ate-dev |
| **CNCF Slack** | #substrate-users / #substrate-dev（[申请邀请](https://slack.cncf.io/)） |
| **参考文章（Go语言中文网）** | https://mp.weixin.qq.com/s/nQ_v_ndQHEln1wTylQrYEg |

## My takeaways

1. **定位清晰**：不做 Agent 开发框架，专注「大规模跑 Agent」的基础设施层，与 ADK/LangChain/Claude Code 等互补。
2. **成本模型创新**：利用 Agent 高空闲率做超售，是 Agent 平台化时的关键降本手段。
3. **K8s 之上而非替代**：复用 Pod/CRD/Controller，Agent 专用调度与状态管理是增量价值。
4. **现阶段观望为主**：思路正确、Demo 亮眼，但 API 不稳定，适合技术预研与架构参考，生产需等成熟。

## Related

- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
- [[30 知识资源/AI/OpenCode Agent 编排核心原理|OpenCode Agent 编排核心原理]]
