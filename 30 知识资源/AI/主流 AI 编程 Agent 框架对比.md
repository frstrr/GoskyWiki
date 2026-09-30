---
id: ai-agent-framework-comparison
title: 主流 AI 编程 Agent 框架对比
type: evergreen
status: active
created: 2026-06-10
updated: 2026-06-10
tags:
  - 知识/人工智能
  - 主题/AI编程Agent
  - 类型/框架对比
aliases:
  - Agent框架对比
  - OpenCode Codex Claude Code对比
summary: 对 OpenCode、OpenAI Codex (CLI/Web)、Claude Code 三大 AI 编程 Agent 的核心架构、编排模式、记忆系统、扩展机制进行系统对比，评估各自的适用场景与优劣势。
moc:
  - [[40 知识导航/AI 编程 Agent]]
related:
  - [[30 知识资源/AI/OpenCode Agent 编排核心原理|OpenCode Agent 编排核心原理]]
---

# 主流 AI 编程 Agent 框架对比

> 基于 OpenCode 源码分析、Codex 官方文档与 GitHub 开源仓库、Claude Code 官方文档整理。
> 最后更新：2026-06-10

---

## 一、三大框架概览

| 维度 | OpenCode | Codex CLI (OpenAI) | Claude Code (Anthropic) |
|------|----------|---------------------|--------------------------|
| **定位** | 开源多 Provider Agent | 本地终端 Agent + 云端沙箱 | 全表面 Agent（终端/IDE/Web） |
| **实现语言** | TypeScript (AI SDK + Effect) | Rust (96%) + SDK | TypeScript |
| **开源** | 是 | CLI 开源 (Apache-2.0) | 否 |
| **执行环境** | 本地进程 | 本地终端 / 云端沙箱 | 本地终端 / 云端 |
| **支持的模型** | 任意 Provider（Anthropic/OpenAI/Gemini/Kimi 等） | GPT 系列 (codex-1/codex-mini) | Claude 系列 |
| **GitHub Stars** | 新兴项目 | ~91k | 闭源无公开 |

---

## 二、核心架构对比

### 2.1 循环编排模式

```
三个框架的核心编排都是 ReAct (Reason-Act) 模式，但细节差异显著：

OpenCode:    while(true) → LLM.stream() → 工具自动执行 → 结果回传 → LLM 不调工具则退出
Codex CLI:   单主循环 → 工具调用 → 沙箱执行 → 结果回传 → finish 退出
Claude Code: 单主循环 → 工具调用 → Hook 拦截 → 结果回传 → 可触发 Subagent
```

| 特性 | OpenCode | Codex CLI | Claude Code |
|------|----------|-----------|-------------|
| 循环退出 | LLM 不调工具则 break | finish_reason 检测 | LLM 不调工具则 break |
| 并发控制 | 单会话单线程 | 本地单线程 | 单会话单线程 + Background Subagent |
| 工具分发 | AI SDK 自动匹配 tool_call | 内置 tool 匹配 | 内置 tool 匹配 |
| 流式输出 | 完整 streamText 事件流 | 非流式（云端异步） | 流式 |

### 2.2 Agent 类型体系

**OpenCode** — 7 个内置 Agent：

| Agent | 模式 | 特点 |
|-------|------|------|
| build | primary | 默认，全工具 |
| plan | primary | 只读，禁止修改 |
| general | subagent | 通用子 Agent |
| explore | subagent | 只读探索 |
| compaction | hidden | 上下文压缩 |
| title | hidden | 生成会话标题 |
| summary | hidden | 会话总结 |

**Claude Code** — 3 个内置 Subagent + 无限自定义：

| Subagent | 模型 | 特点 |
|----------|------|------|
| Explore | Haiku（快） | 只读搜索，不加载 CLAUDE.md |
| Plan | 继承主会话 | Plan 模式专用 |
| General-purpose | 继承主会话 | 全工具，复杂任务 |

**Codex CLI** — 无显式子 Agent，单主循环完成所有任务。
云端 Codex 则支持多任务并行（每个任务独立沙箱），但沙箱间无通信。

### 2.3 Subagent 能力对比

| 能力 | OpenCode | Codex CLI | Claude Code |
|------|----------|-----------|-------------|
| 自定义 Subagent | 有限（Agent 定义） | 不支持 | 完整（YAML frontmatter + Markdown） |
| 独立 System Prompt | 是 | — | 是 |
| 独立工具集 | 是（permission） | — | 是（tools/disallowedTools） |
| 独立模型 | 否 | — | 是（model 字段） |
| 独立 Hooks | 否 | — | 是（PreToolUse/PostToolUse/Stop） |
| 独立 MCP Server | 否 | — | 是（mcpServers 字段） |
| 持久记忆 | 否 | — | 是（memory 字段：user/project/local） |
| Worktree 隔离 | 否 | — | 是（isolation: worktree） |
| 嵌套 Subagent | 否 | — | 是（支持递归子 Agent） |
| 后台运行 | 否（伪并行） | 云端可并行 | 是（background: true） |
| Agent Teams | 否 | 云端多任务并行 | 是（真正多 Agent 并行） |

---

## 三、Prompt 体系对比

### 3.1 OpenCode：7 层分层组装（最精细）

```
Layer 1: Provider 基础模板 (anthropic.txt / gpt.txt / gemini.txt)
Layer 2: Agent 自定义 Prompt
Layer 3: 环境信息（模型ID/工作目录/平台/日期/git状态）
Layer 4: 项目指令文件（AGENTS.md / CLAUDE.md）
Layer 5: Skills 列表
Layer 6: Reminders（plan-mode / build-switch 模式切换）
Layer 7: Plugin Transform 后处理（拦截器钩子）
```

### 3.2 Claude Code：多层 CLAUDE.md + Rules + Memory

```
Managed Policy (组织级)           ← IT 统一部署
User CLAUDE.md (~/.claude)        ← 个人全局偏好
Project CLAUDE.md (./CLAUDE.md)   ← 团队共享
Local CLAUDE.md (./CLAUDE.local.md) ← 个人项目偏好
.claude/rules/*.md (路径匹配规则)   ← 按文件路径懒加载
Auto Memory (MEMORY.md)           ← Agent 自动积累的知识
Skills (按需加载)                  ← 任务驱动加载
```

### 3.3 Codex：AGENTS.md + 系统提示

```
System Prompt (公开，固定)
AGENTS.md (仓库内任意层级，越深优先级越高)
用户任务 (唯一输入)
```

### 3.4 Prompt 体系评估

| 维度 | OpenCode | Codex | Claude Code |
|------|----------|-------|-------------|
| 定制深度 | 最高（7 层可单独覆盖） | 最低（AGENTS.md） | 高（多层 + Rules + Memory） |
| 动态注入 | Plugin Transform 拦截 LLM 请求 | 不支持 | Hooks |
| 上下文节省 | Prompt Caching 精控 | 自动（API 层） | Path-scoped Rules（懒加载） |

---

## 四、记忆系统对比

### 各框架记忆能力

| 维度 | OpenCode | Codex CLI | Claude Code |
|------|----------|-----------|-------------|
| 会话内记忆 | 有（完整消息历史） | 有 | 有 |
| 上下文压缩 | 自动（compaction agent） | 无（192k 够大） | 手动 `/compact` |
| 跨会话记忆 | **无** | **无** | **Auto Memory** |
| 项目级指令 | AGENTS.md | AGENTS.md | CLAUDE.md (多层) |
| 路径绑定规则 | 无 | 无 | `.claude/rules/` + paths |
| Subagent 专属记忆 | 无 | 无 | 有（memory 字段） |
| 自动积累知识 | 无 | 无 | 有（Claude 自己写 MEMORY.md） |

### Claude Code Auto Memory 机制

```
每会话开始：读取 MEMORY.md 前 200 行 (或 25KB)
会话中：Claude 主动写入发现的规律、命令、调试技巧
下次会话：自动加载，形成累积效应

存储：~/.claude/projects/<project>/memory/
├── MEMORY.md           ← 索引（每会话加载）
├── debugging.md        ← 详细笔记（按需读取）
└── api-conventions.md  ← 领域知识（按需读取）
```

> **评估**：这是 Claude Code 相对其他两者最大的差异化优势。Auto Memory 让 Agent 随时间越来越了解你的项目，而 OpenCode 和 Codex 每次都是从零开始。

---

## 五、扩展机制对比

### 5.1 扩展点总览

| 扩展类型 | OpenCode | Codex CLI | Claude Code |
|----------|----------|-----------|-------------|
| Skills | 有（SKILL.md + 描述匹配） | 无 | 有（Skills + /命令） |
| Plugins | 有（三个 Transform 钩子） | 无 | 有（Plugins） |
| Hooks | 无 | 无 | 有（PreToolUse/PostToolUse/Stop/SubagentStart/SubagentStop 等） |
| MCP | 有 | 有 | 有 |
| 自定义工具 | 有（Tool.define） | 有（SDK） | 有（MCP + Hooks） |

### 5.2 Plugin Transform 详解（OpenCode 亮点）

OpenCode 的 Plugin Transform 是三个框架中**唯一能在 LLM 请求发送前拦截并修改请求参数**的机制：

```
三个钩子：
1. experimental.chat.system.transform → 修改 System Prompt
2. chat.params → 修改 LLM 参数（maxOutputTokens/temperature 等）
3. chat.headers → 添加自定义 HTTP Headers
```

实际用例：OpenAI Codex 插件用 `chat.params` 移除输出 token 上限；GitHub Copilot 插件用 `chat.headers` 注入 API 版本。

> **评估**：这套机制在多 Provider 适配场景中非常优雅，将 Provider 差异逻辑从核心代码中解耦。Codex 因为是单一 Provider，无此需求；Claude Code 也是单一 Provider，由内部处理。

### 5.3 Hooks 系统详解（Claude Code 亮点）

Claude Code 的 Hooks 是在工具调用前后执行的 Shell 命令，可基于退出码决定是否阻止操作：

```
PreToolUse  → 工具执行前验证（可阻止）
PostToolUse → 工具执行后触发（后处理）
Stop        → Agent 完成时触发
SubagentStart/SubagentStop → 子 Agent 生命周期

退出码约定：
  0 → 允许继续
  2 → 阻止本次工具调用
```

> **评估**：这让 Claude Code 具备了**超越 LLM 推理的硬约束能力**——比如强制在 Bash 命令里禁止写操作，或每次文件编辑后自动跑 linter。OpenCode 的 permission ruleset 只能做粗粒度的工具级允许/拒绝，无法做到命令级内容验证。

---

## 六、上下文管理对比

### 6.1 Prompt Caching

**OpenCode**（最完整）：
```
层 1: 消息级缓存断点
  - 前 2 条 system 消息 + 后 2 条非 system 消息打标
  - 按 Provider 用不同格式（Anthropic/Bedrock/OpenRouter/Alibaba）

层 2: Session 级缓存 Key
  - 用 sessionID 作为 promptCacheKey
  - 同会话多轮请求共享服务端缓存

计费分离：cache read 按 input 10% 计费，cache write 按 input 125% 计费
```

**Codex CLI**：API 层自动缓存，无用户可控缓存断点。

**Claude Code**：Anthropic 自动管理，用户无法精确控制断点位置。

### 6.2 上下文压缩

| 触发方式 | OpenCode | Codex | Claude Code |
|----------|----------|-------|-------------|
| 自动（token 超限） | 有 | 无 | 无 |
| 被动（Provider 报错） | 有 | — | 无 |
| 手动触发 | 有 | — | 有（`/compact`） |
| 压缩执行者 | compaction agent | — | Claude 自身 |
| 压缩后保留 | 摘要 + 最近 2 轮 | — | 摘要 + CLAUDE.md 重注入 |

### 6.3 工具输出处理

| 场景 | OpenCode | Codex | Claude Code |
|------|----------|-------|-------------|
| 长输出截断 | 截断到 toolOutputMaxChars | 自然截断 | 自然截断 |
| 旧工具输出清理 | Pruning（保护最近 40k tokens） | 无 | 压缩时处理 |
| 清理标记 | `[Old tool result content cleared]` | — | — |

---

## 七、安全与权限对比

| 维度 | OpenCode | Codex CLI | Claude Code |
|------|----------|-----------|-------------|
| 权限粒度 | Agent 级 allow/deny | 沙箱隔离 | 工具级 + 命令级 + Hooks |
| 文件写保护 | 通过 permission 配置 | 沙箱网络隔离 | 审批流 + Hooks |
| 网络访问控制 | 无 | 云端版本：执行时无网络 | 有（sandbox） |
| 操作审计 | 无 | 无（有 Citations 追溯） | Hooks 可记录 |
| 命令内容验证 | 不支持 | 不支持 | PreToolUse Hook（退出码 2 阻止） |

> **评估**：Codex 的云端沙箱隔离是最强的物理安全边界（无网络）。但 Claude Code 的 Hooks 系统在逻辑层提供了最细的运行时约束——可以检查每条 Bash 命令的内容再决定是否允许执行。

---

## 八、多表面覆盖对比

| 界面 | OpenCode | Codex | Claude Code |
|------|----------|-------|-------------|
| 终端 CLI | ✅ | ✅ | ✅ |
| VS Code | ❌ | ✅ | ✅ |
| JetBrains | ❌ | ❌ | ✅ |
| Web | ❌ | ✅ (chatgpt.com/codex) | ✅ (claude.ai/code) |
| iOS | ❌ | ❌ | ✅ |
| Desktop App | ❌ | ✅ (codex app) | ✅ |
| CI/CD 集成 | ❌ | GitHub 原生 | GitHub Actions / GitLab CI |
| Slack/Chat | ❌ | ❌ | ✅ |

---

## 九、优劣势汇总

### OpenCode

**优势：**
- 7 层 Prompt 分层最精细，Provider/Agent/环境/指令/Skills/Reminders/Plugin 可独立定制
- Plugin Transform 钩子解耦多 Provider 适配逻辑
- Prompt Caching 支持最完整（精控断点位置 + Session Key + 分账计费）
- 开源透明，易二次开发，支持任意 LLM Provider
- 上下文压缩全自动（主动预防 + 被动响应 + 手动）

**劣势：**
- 无跨会话记忆，每会话从零开始
- 主循环单线程，Subagent 是伪并行
- Subagent 机制较弱（无 Hooks/独立 memory/worktree 隔离/嵌套）
- 无 IDE 集成，仅 TUI
- 无 Hooks 系统，权限只能粗粒度工具级

### Codex CLI (OpenAI)

**优势：**
- Rust 实现，本地执行性能极佳
- 云端版本可多任务真正并行，每任务独立沙箱
- 沙箱隔离 + 无网络访问，安全性最高
- Citations 系统：每步操作可追溯到文件/终端日志
- AGENTS.md 规范被多框架采纳

**劣势：**
- 架构封闭（codex-1 模型是黑盒，CLI 版能力受限）
- 无 Subagent 编排，复杂任务只靠单主循环
- 无跨会话记忆
- 云端版不可中途修正（fire-and-forget）
- 无 Prompt Caching 用户控制
- 工具集简单（主要靠 Bash + 文件读写）
- 绑定 OpenAI 生态，无法切换 Provider

### Claude Code

**优势：**
- 记忆系统最完善：Auto Memory 跨会话学习 + CLAUDE.md 多层 + 路径绑定 Rules
- Subagent 最完整：自定义 prompt/tools/model/hooks/memory/isolation + 嵌套
- Hooks 系统：6 种生命周期钩子，可执行任意 Shell 命令
- 多表面覆盖广：Terminal/VS Code/JetBrains/Desktop/Web/iOS 体验一致
- Agent Teams：真正多 Agent 并行（各自独立上下文）
- Path-scoped Rules：规则只在操作匹配文件时加载，节省 token
- `/agents` 交互式管理 + `@-mention` 显式调度
- CI/CD 集成 + Slack + Webhooks

**劣势：**
- 无 OpenCode 那样的 Plugin Transform 拦截层（无法精控 LLM 请求参数）
- Prompt Caching 由 Anthropic 自动管理，用户无法精控
- 闭源，核心编排逻辑不透明
- Subagent 嵌套深度有上限
- Auto Memory 仅限机器本地，不跨设备同步
- 绑定 Anthropic 生态，无法切换 Provider

---

## 十、场景选型建议

| 场景 | 推荐框架 | 原因 |
|------|----------|------|
| 深度定制 LLM 请求 / Prompt 层 | **OpenCode** | 7 层分层 + Plugin Transform 无出其右 |
| 多任务并行 + 安全沙箱 | **Codex (Cloud)** | 独立沙箱 + 物理网络隔离 |
| 日常开发 + 跨会话记住项目知识 | **Claude Code** | Auto Memory 持续积累 |
| 团队协作 + CI/CD 自动化 | **Claude Code** | 多表面覆盖 + Hooks + CI 集成 |
| 开源二次开发 + 多 Provider | **OpenCode** | 开源透明 + 任意模型 |
| 高性能本地执行 | **Codex CLI** | Rust 实现，启动快 |
| 复杂多步骤任务 + 并行子任务 | **Claude Code** | 嵌套 Subagent + 后台运行 |
| 细粒度操作审计与安全 | **Claude Code** | Hooks + 命令内容验证 |

---

## 总结

> 一句话概括三者定位差异：
> - **OpenCode** 赢在**可控性和透明性**（Prompt 精控、多 Provider 适配、完全开源）
> - **Codex** 赢在**并行沙箱和执行性能**（Rust + 云端隔离 + Citations 追溯）
> - **Claude Code** 赢在**记忆系统和 Agent 编排的完整性**（Auto Memory + 嵌套 Subagent + Hooks + 全表面覆盖）

选择哪个框架，核心取决于你的需求权重：**透明度定制** → OpenCode，**安全并行** → Codex，**持续学习 + 完整编排** → Claude Code。

---

## Related

- [[30 知识资源/AI/OpenCode Agent 编排核心原理|OpenCode Agent 编排核心原理]]
