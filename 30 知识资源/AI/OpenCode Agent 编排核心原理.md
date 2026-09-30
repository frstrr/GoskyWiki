---
id: opencode-agent-orchestration-core
title: OpenCode Agent 编排核心原理
type: evergreen
status: active
created: 2026-06-10
updated: 2026-06-18
tags:
  - 知识/人工智能
  - 主题/AI编程Agent
  - 工具/OpenCode
aliases:
  - OpenCode Agent 原理
  - OpenCode 编排机制
summary: 基于 OpenCode 源码分析，说明其 Agent 编排如何通过 while 循环、LLM 流式调用、工具自动执行、分层 System Prompt 和上下文管理组成一个 LLM 驱动的自动状态机。
moc:
  - [[40 知识导航/AI 编程 Agent]]
related:
  - [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
---

# OpenCode Agent 编排核心原理

> 本文档基于 opencode 项目源码分析，整理了其 Agent 编排的核心架构和工作原理。

## 一、核心原理一句话概括

**一个 while 循环 + LLM 流式调用 + 工具自动执行**，即经典的 ReAct (Reason-Act) 模式。

## 二、整体架构

```
用户输入
  │
  ▼
prompt()          ← 入口，创建用户消息
  │
  ▼
loop()            ← 确保单会话单线程
  │
  ▼
runLoop()         ← 核心循环 while(true)
  │
  ├─ 加载消息历史
  ├─ 检查退出条件（LLM 不再调工具 → break）
  ├─ 组装 system prompt（分层策略）
  ├─ 解析当前 Agent 可用的 Tools
  ├─ 调用 LLM.stream()
  │    │
  │    └─ streamText(model, messages, tools)
  │         流式返回事件：text-delta / tool-call / tool-result
  │
  ├─ processor 实时处理每个事件，写入 DB
  │    ├─ 文本片段 → text part
  │    ├─ 工具调用 → AI SDK 自动执行 tool.execute()
  │    └─ 工具结果 → 回传给 LLM 作为下一轮上下文
  │
  └─ 判断结果：continue → 继续循环 / stop → 退出
```

## 三、关键抽象（5个核心概念）

| 抽象            | 文件                             | 职责                                        |
| ------------- | ------------------------------ | ----------------------------------------- |
| **Session**   | `session/session.ts`           | 会话生命周期、消息存储、token 统计                      |
| **Agent**     | `agent/agent.ts`               | 定义角色（build/plan/explore等）、权限、prompt 模板    |
| **Tool**      | `tool/tool.ts` + `registry.ts` | `Tool.define()` 注册工具，AI SDK 自动分发执行        |
| **LLM**       | `session/llm.ts`               | 封装 `streamText()`，返回统一的 `LLMEvent` 流      |
| **Processor** | `session/processor.ts`         | 消费事件流，更新消息 parts，决定 continue/stop/compact |

## 四、核心循环退出逻辑

```typescript
// prompt.ts 简化逻辑
// LLM 返回 finish_reason 不是 "tool-calls" 且无待执行工具 → break
```

也就是说：**LLM 不调工具了就停**。每次 LLM 调用工具，结果会回传，循环继续让 LLM 看到结果再决定下一步。

## 五、工具执行机制

不是手动 if/else 分发，而是：

1. 所有 `Tool.Def` 转成 AI SDK 的 `tool({ inputSchema, execute })` 对象
2. 传给 `streamText()`
3. LLM 输出 tool_call 时，**AI SDK 自动匹配并执行**对应 `execute()`
4. 结果自动作为 tool_result 回传给 LLM

### 工具注册代码结构

```typescript
// tool/tool.ts 核心抽象
Tool.Def = {
  id: string,
  description: string,
  parameters: Schema,
  execute: (args, ctx) => Effect<ExecuteResult>
}

// session/tools.ts 转换为 AI SDK 格式
for (const item of tools) {
  aiTools[item.id] = tool({
    inputSchema: item.jsonSchema,
    execute: async (args, options) => {
      // 桥接到 Effect，执行实际逻辑
      // 包含权限检查、插件钩子、输出截断
    }
  })
}
```

## 六、System Prompt 分层组装（深入解析）

System Prompt 是整个 Agent 行为的核心驱动力。opencode 采用**分层组装**策略，根据不同模型和场景动态构建最终的系统提示。

### 6.1 整体架构

```
┌─────────────────────────────────────────────────────────────┐
│ Layer 1: Provider 基础模板                                    │
│   (anthropic.txt / gpt.txt / gemini.txt / default.txt)      │
├─────────────────────────────────────────────────────────────┤
│ Layer 2: Agent 自定义 Prompt                                  │
│   (explore.txt / compaction.txt / 用户自定义)                │
├─────────────────────────────────────────────────────────────┤
│ Layer 3: 环境信息                                             │
│   (模型ID、工作目录、平台、日期、git状态)                     │
├─────────────────────────────────────────────────────────────┤
│ Layer 4: 项目指令文件                                         │
│   (AGENTS.md / CLAUDE.md / CONTEXT.md)                      │
├─────────────────────────────────────────────────────────────┤
│ Layer 5: 可用 Skills 列表                                     │
├─────────────────────────────────────────────────────────────┤
│ Layer 6: Reminders（模式切换提示）                            │
│   (plan-mode / build-switch)                                │
├─────────────────────────────────────────────────────────────┤
│ Layer 7: 插件 Transform 后处理                                │
└─────────────────────────────────────────────────────────────┘
```

### 6.2 Layer 1: Provider 基础模板选择

**源码位置**: `session/system.ts:25-39`

```typescript
export function provider(model: Provider.Model) {
  if (model.api.id.includes("gpt-4") || model.api.id.includes("o1") || model.api.id.includes("o3"))
    return [PROMPT_BEAST]
  if (model.api.id.includes("gpt")) {
    if (model.api.id.includes("codex")) return [PROMPT_CODEX]
    return [PROMPT_GPT]
  }
  if (model.api.id.includes("gemini-")) return [PROMPT_GEMINI]
  if (model.api.id.includes("claude")) return [PROMPT_ANTHROPIC]
  if (model.api.id.toLowerCase().includes("trinity")) return [PROMPT_TRINITY]
  if (model.api.id.toLowerCase().includes("kimi")) return [PROMPT_KIMI]
  return [PROMPT_DEFAULT]
}
```

**核心逻辑**: 根据模型 ID 匹配关键词，选择对应的 prompt 模板文件。

**关键模板示例** (`anthropic.txt` 节选):

```
You are OpenCode, the best coding agent on the planet.

You are an interactive CLI tool that helps users with software engineering tasks.
Use the instructions below and the tools available to you to assist the user.

# Tone and style
- Only use emojis if the user explicitly requests it
- Your responses should be short and concise
- Use GitHub-flavored markdown for formatting

# Tool usage policy
- When doing file search, prefer to use the Task tool
- You can call multiple tools in a single response
- If you intend to call multiple tools and there are no dependencies,
  make all independent tool calls in parallel

# Doing tasks
- Use the TodoWrite tool to plan the task if required
- Use the TodoWrite tool VERY frequently to ensure tracking
```

### 6.3 Layer 2: Agent 自定义 Prompt

**源码位置**: `agent/agent.ts`

每个 Agent 可以定义专属的 prompt，覆盖默认行为：

| Agent | Prompt 文件 | 用途 |
|-------|------------|------|
| **explore** | `agent/prompt/explore.txt` | 专注文件搜索，禁止修改 |
| **compaction** | `agent/prompt/compaction.txt` | 上下文压缩，不直接对话 |
| **summary** | `agent/prompt/summary.txt` | 生成 PR 描述风格的总结 |
| **title** | `agent/prompt/title.txt` | 生成 50 字符内的会话标题 |
| **plan** | 无专属 prompt，用 provider prompt | 只读规划模式 |

**explore agent prompt 示例**:

```
You are a file search specialist. You excel at thoroughly navigating
and exploring codebases.

Your strengths:
- Rapidly finding files using glob patterns
- Searching code and text with powerful regex patterns
- Reading and analyzing file contents

Guidelines:
- Use Glob for broad file pattern matching
- Use Grep for searching file contents with regex
- Use Read when you know the specific file path you need to read
- Do not create any files, or run bash commands that modify the
  user's system state in any way
```

**请求组装逻辑** (`session/llm/request.ts:56-66`):

```typescript
const system = [
  [
    // Agent 有自定义 prompt 则使用，否则用 provider 模板
    ...(input.agent.prompt ? [input.agent.prompt] : SystemPrompt.provider(input.model)),
    ...input.system,  // 环境、指令、skills
    ...(input.user.system ? [input.user.system] : []),
  ].filter((x) => x).join("\n")
]
```

### 6.4 Layer 3: 环境信息注入

**源码位置**: `session/system.ts:55-91`

```typescript
environment: Effect.fn("SystemPrompt.environment")(function* (model) {
  const ctx = yield* InstanceState.context
  return [
    [
      `You are powered by the model named ${model.api.id}`,
      `Here is some useful information about the environment you are running in:`,
      `<env>`,
      `  Working directory: ${ctx.directory}`,
      `  Workspace root folder: ${ctx.worktree}`,
      `  Is directory a git repo: ${ctx.project.vcs === "git" ? "yes" : "no"}`,
      `  Platform: ${process.platform}`,
      `  Today's date: ${new Date().toDateString()}`,
      `</env>`,
    ].join("\n"),
    // 项目引用（额外可访问的目录）
    references.length === 0 ? undefined : [
      "Project references provide additional directories...",
      "<available_references>",
      ...references.flatMap((ref) => [
        "  <reference>",
        `    <name>${ref.name}</name>`,
        `    <path>${ref.path}</path>`,
        `    <description>${ref.description}</description>`,
        "  </reference>",
      ]),
      "</available_references>",
    ].join("\n"),
  ]
})
```

### 6.5 Layer 4: 项目指令文件加载

**源码位置**: `session/instruction.ts`

opencode 支持从多个位置加载项目级指令文件：

**查找顺序**:
1. **全局配置**: `~/.config/opencode/AGENTS.md`, `~/.claude/CLAUDE.md`
2. **项目级**: 从当前目录向上查找到 worktree 根目录
3. **配置文件指定**: `opencode.json` 中的 `instructions` 字段（支持本地 glob 和远程 URL）

**指令文件格式**: `AGENTS.md` / `CLAUDE.md` / `CONTEXT.md`（已废弃）

**加载逻辑**:

```typescript
// instruction.ts:155-168
const system = Effect.fn("Instruction.system")(function* () {
  const paths = yield* systemPaths()
  const urls = (config.instructions ?? []).filter(
    (item) => item.startsWith("https://") || item.startsWith("http://")
  )

  const files = yield* Effect.forEach(Array.from(paths), read, { concurrency: 8 })
  const remote = yield* Effect.forEach(urls, fetch, { concurrency: 4 })

  return [
    ...Array.from(paths).flatMap((item, i) =>
      (files[i] ? [`Instructions from: ${item}\n${files[i]}`] : [])
    ),
    ...urls.flatMap((item, i) =>
      (remote[i] ? [`Instructions from: ${item}\n${remote[i]}`] : [])
    ),
  ]
})
```

**示例 AGENTS.md 内容**:

```markdown
# opencode database guide

## Database
- Schema: Drizzle schema lives in packages/core/src/**/*.sql.ts
- Migrations: live in packages/core

## Development server
- Running bun dev from packages/opencode starts the live TUI
- Start it in tmux instead: tmux new-session -d -s opencode-dev

# Module shape
Do not use export namespace Foo { ... } for module organization...
```

### 6.6 Layer 5: Skills 列表注入

**源码位置**: `session/system.ts:94-106`

```typescript
skills: Effect.fn("SystemPrompt.skills")(function* (agent) {
  if (Permission.disabled(["skill"], agent.permission).has("skill")) return

  const list = yield* skill.available(agent)

  return [
    "Skills provide specialized instructions and workflows for specific tasks.",
    "Use the skill tool to load a skill when a task matches its description.",
    Skill.fmt(list, { verbose: true }),
  ].join("\n")
})
```

Skills 是可选的扩展机制，允许注册专门的子技能（如 git commit 规范、图片生成等），LLM 可以根据任务描述自动加载。

### 6.7 Layer 6: Reminders（模式切换提示）

**源码位置**: `session/reminders.ts`

根据当前 Agent 模式，动态注入行为约束：

**Plan 模式** - 只读，禁止修改：

```xml
<system-reminder>
# Plan Mode - System Reminder

CRITICAL: Plan mode ACTIVE - you are in READ-ONLY phase. STRICTLY FORBIDDEN:
ANY file edits, modifications, or system changes.
This ABSOLUTE CONSTRAINT overrides ALL other instructions.

## Responsibility
Your current responsibility is to think, read, search, and delegate
explore agents to construct a well-formed plan...
</system-reminder>
```

**Build 模式** - 允许修改：

```xml
<system-reminder>
Your operational mode has changed from plan to build.
You are no longer in read-only mode.
You are permitted to make file changes, run shell commands,
and utilize your arsenal of tools as needed.
</system-reminder>
```

**完整 Plan Mode 工作流** (`plan-mode.txt`):

```xml
<system-reminder>
Plan mode is active. You MUST NOT make any edits except the plan file.

## Plan Workflow

### Phase 1: Initial Understanding
- Launch up to 3 explore agents IN PARALLEL
- After exploring, use the question tool to clarify ambiguities

### Phase 2: Design
- Launch general agent(s) to design the implementation
- Default: Launch at least 1 Plan agent

### Phase 3: Review
- Read the critical files identified by agents
- Use question tool to clarify remaining questions

### Phase 4: Final Plan
- Write your final plan to the plan file
- Include paths of critical files to modify
- Include a verification section

### Phase 5: Call plan_exit tool
- Your turn should only end with either asking a question or calling plan_exit
</system-reminder>
```

### 6.8 Layer 7: 插件 Transform

**源码位置**: `session/llm/request.ts:69-78`

```typescript
// 插件可以修改 system prompt
yield* input.plugin.trigger(
  "experimental.chat.system.transform",
  { sessionID: input.sessionID, model: input.model },
  { system }
)

// 插件可以修改请求参数
const params = yield* input.plugin.trigger(
  "chat.params",
  { sessionID, agent, model, provider, message },
  { temperature, topP, topK, maxOutputTokens, options }
)

// 插件可以添加自定义 headers
const { headers } = yield* input.plugin.trigger(
  "chat.headers",
  { sessionID, agent, model, provider, message },
  { headers: {} }
)
```

### 6.9 最终组装流程

**源码位置**: `session/prompt.ts:1327-1335`

```typescript
// 1. 并行获取各层内容
const [skills, env, instructions, modelMsgs] = yield* Effect.all([
  sys.skills(agent),      // Layer 5: Skills
  sys.environment(model), // Layer 3: 环境信息
  instruction.system(),   // Layer 4: 项目指令
  MessageV2.toModelMessagesEffect(msgs, model),
])

// 2. 组装 system 数组
const system = [...env, ...instructions, ...(skills ? [skills] : [])]

// 3. 传递给 request.prepare()
// prepare() 会：
//    - 添加 Layer 1 (provider prompt) 或 Layer 2 (agent prompt)
//    - 添加 Layer 7 (插件 transform)
//    - 最终拼接成完整的 system prompt 字符串
```

## 七、代码搜索策略

**LLM 自己决定的，不是硬编码的。**

### 工具是"菜单"，LLM 是"点菜的人"

| 工具 | 能力 |
|------|------|
| `grep` | 按正则搜内容 |
| `glob` | 按文件名模式查找 |
| `read` | 读文件/目录 |
| `task` (explore) | 派子 agent 做深度探索 |

LLM 看到这些工具的描述后，**自主决定**：搜什么关键词、用哪个工具、搜几轮、要不要再深入。

### 软约束在引导 LLM

1. **System prompt** — 写了"encouraged to use search tools extensively"
2. **工具描述** — 每个 tool 的 `description` 告诉 LLM 适用场景
3. **Agent prompt** — `explore` agent 有专门优化的搜索指导 prompt

### 本质

> 搜索策略 = **工具定义（硬能力）** + **prompt 引导（软策略）** + **LLM 自主推理（实际决策）**

项目代码里**没有任何硬编码的搜索流程**，不存在 "先 glob 再 grep 再 read" 这样的固定逻辑。每一步都是 LLM 实时推理的结果。

## 八、Agent 类型与权限

### 内置 Agent 定义

| Agent | 模式 | 特点 |
|-------|------|------|
| **build** | primary | 默认 agent，允许所有工具 |
| **plan** | primary | 只读，禁止编辑（只能写 plan 文件） |
| **general** | subagent | 通用子 agent |
| **explore** | subagent | 只读探索，禁止修改 |
| **compaction** | hidden | 上下文压缩 |
| **title** | hidden | 生成会话标题 |
| **summary** | hidden | 生成会话总结 |

### 权限控制示例

```typescript
// agent.ts
{
  name: "explore",
  mode: "subagent",
  prompt: PROMPT_EXPLORE,
  permission: {
    deny: ["edit", "write", "shell", "todowrite", ...],
    allow: ["grep", "glob", "list", "bash", "webfetch", "read"]
  }
}
```

## 九、端到端流程图

```
User sends message
        |
        v
SessionPrompt.prompt(input)
    |
    +-- createUserMessage(input)
    |       解析 agent, model, 文件附件
    |       持久化用户消息到 DB
    |
    +-- loop({sessionID})
            |
            +-- SessionRunState.ensureRunning()
                    确保单会话单线程
                    |
                    +-- runLoop(sessionID)
                            |
                            while(true) {
                                1. 加载压缩后的消息历史
                                2. 检查退出条件
                                3. 处理子任务/压缩任务
                                4. 检查上下文溢出
                                5. 解析 agent 配置 + 权限
                                6. 应用 reminders (plan mode 等)
                                7. 创建 assistant 消息
                                8. SessionProcessor.create() -> Handle
                                9. SessionTools.resolve() -> AI SDK tools
                               10. 构建 system prompt (分层组装)
                               11. 转换历史为 ModelMessage[]
                               12. Handle.process(streamInput)
                                    |
                                    +-- LLM.stream(input)
                                    |       选择运行时 (AI SDK / 原生)
                                    |       返回 Stream<LLMEvent>
                                    |
                                    +-- Stream.tap(handleEvent)
                                    |       处理每个事件:
                                    |       - text-delta -> 追加文本
                                    |       - tool-call -> 执行工具
                                    |       - tool-result -> 完成工具
                                    |
                                    +-- 返回 "continue" | "stop" | "compact"
                                |
                                13. 判断结果 -> break 或继续循环
                            }
```

## 十一、Plugin Transform 详解

Plugin Transform 是 opencode 的**扩展钩子机制**，允许内置插件和用户自定义插件在每次调 LLM 前**拦截并修改**请求的三个部分。

### 工作机制

插件通过 `opencode.json` 配置加载，每次 LLM 调用前会依次触发：

```
触发钩子 → 遍历所有注册的插件 → 插件原地修改 output 对象 → 传给 LLM
```

```typescript
// plugin/index.ts - 触发逻辑
for (const hook of s.hooks) {
  const fn = hook[name]
  if (!fn) continue
  yield* Effect.promise(() => fn(input, output))  // 插件直接改 output
}
```

### 三个 Transform 钩子

#### 1. `experimental.chat.system.transform` — 修改 System Prompt

```typescript
// 插件可以往系统提示里加东西
"experimental.chat.system.transform": async (input, output) => {
  output.system.unshift("你是一个遵守XXX规范的助手")
}
```

目前没有内置插件使用，但**用户自定义插件**可以用来注入额外的指令。比如给 LLM 加团队规范、安全审计提示等。

#### 2. `chat.params` — 修改 LLM 参数

内置插件实际用例：

```typescript
// OpenAI codex 插件 — 移除输出 token 上限
"chat.params": async (input, output) => {
  if (input.model.providerID !== "openai") return
  output.maxOutputTokens = undefined
}

// GitHub Copilot 插件 — GPT 模型不限 token + 禁用 Anthropic tool streaming
"chat.params": async (input, output) => {
  if (input.model.providerID.includes("github-copilot")) return
  if (input.model.api.id.includes("gpt")) {
    output.maxOutputTokens = undefined
  }
  if (input.model.api.npm === "@ai-sdk/anthropic") {
    output.options.toolStreaming = false
  }
}

// Cloudflare 插件 — OpenAI 推理模型不限 token
"chat.params": async (input, output) => {
  if (input.model.providerID !== "cloudflare-ai-gateway") return
  if (!input.model.capabilities.reasoning) return
  output.maxOutputTokens = undefined
}
```

#### 3. `chat.headers` — 添加自定义 HTTP Headers

```typescript
// OpenAI codex 插件 — 添加 originator、User-Agent、session-id
"chat.headers": async (input, output) => {
  if (input.model.providerID !== "openai") return
  output.headers.originator = "opencode"
  output.headers["session-id"] = input.sessionID
}

// GitHub Copilot 插件 — 添加 API 版本、交互类型标记
"chat.headers": async (input, output) => {
  if (!input.model.providerID.includes("github-copilot")) return
  output.headers["X-GitHub-Api-Version"] = "2025-04-01"
  output.headers["x-initiator"] = "agent"
}
```

### 一句话总结

> Plugin Transform = **LLM 请求发送前的拦截器**。让不同 Provider 的适配逻辑（去 token 上限、加特殊 header、改 system prompt）**从核心代码中解耦出来**，同时给用户留了自定义入口。

## 十二、历史消息编排

### 整体流程

```
DB 全部消息 → filterCompacted() → toModelMessagesEffect() → ProviderTransform.message() → 发给 LLM
```

### Step 1: `filterCompacted()` — 选择有效历史

`session/message-v2.ts:532-583`

从最新消息**往回走**，收集到压缩边界就停止。压缩后只保留：

```
[压缩摘要 + 保留的最新几条消息] + [压缩后的新对话]
```

旧消息被丢弃，由压缩摘要替代。

### Step 2: `toModelMessagesEffect()` — 转换为 ModelMessage[]

`session/message-v2.ts:142-426`

按时间顺序遍历消息，构建 AI SDK 格式的 `UIMessage[]`，关键处理：

| 场景 | 处理方式 |
|------|---------|
| 空消息（0 parts） | 跳过 |
| 出错的 assistant 消息 | 跳过 |
| 已压缩的工具输出 | 替换为 `"[Old tool result content cleared]"` |
| 长工具输出 | 截断到 `toolOutputMaxChars` |
| 换模型后的 reasoning | 降级为普通文本 |
| 不支持 media 的 provider | 提取为合成的 user 消息 |
| `step-start` part | 发送前过滤掉 |
| 达到步数上限 | 追加 prefill: `[{role: "assistant", content: MAX_STEPS}]` |

### Step 3: `ProviderTransform.message()` — Provider 适配 + 缓存标记

`provider/transform.ts:430-445`

最后一步按 provider 做规范化，包括添加缓存断点（下面详述）。

## 十三、上下文压缩触发条件

有 **3 个触发点**：

### 触发 1: 每轮 token 用量检查（主动预防）

`session/prompt.ts:1214-1221`

```typescript
// 每轮 LLM 回复后检查
if (lastFinished.tokens >= usable(model)) {
  yield* compaction.create({ auto: true })
  continue  // 下一轮先做压缩
}
```

**阈值计算** (`overflow.ts`):

```typescript
const COMPACTION_BUFFER = 20_000

// 可用预算 = 输入上限 - 输出token预留
usable = model.limit.input - maxOutputTokens
// 或 context - maxOutputTokens（无独立 input 限制时）
```

### 触发 2: Provider 返回 Context Overflow 错误（被动响应）

`session/processor.ts:926-936`

```typescript
if (ContextOverflowError.isInstance(error)) {
  ctx.needsCompaction = true  // 标记需要压缩
  return "compact"            // 告知循环
}
```

循环收到 `"compact"` 后创建 `overflow: true` 的压缩任务，会额外 **strip media**。

### 触发 3: 用户手动触发

通过 `/compact` 等命令手动发起。

### 压缩过程本身

```
1. 找到上次压缩摘要（避免重复摘要）
2. select() 切分 head/tail：
   - head = 旧消息（会被摘要掉）
   - tail = 保留最近的 2 轮（preserveRecentBudget = 可用预算的25%，2k~8k tokens）
3. head 消息 stripMedia + 工具输出截断到 2000 字符
4. 发给 compaction agent 做摘要
5. 结果写入摘要消息
```

### 额外：Pruning（工具输出清理）

`compaction.ts:253-297`，在循环退出时执行：

```typescript
const PRUNE_MINIMUM = 20_000   // 至少要清理 20k tokens
const PRUNE_PROTECT = 40_000   // 保护最近 40k tokens 的工具输出

// 从后往前遍历工具输出，保护最新的，标记旧的要清除
```

旧的工具输出内容会被替换为 `"[Old tool result content cleared]"`，但保留结构。

## 十四、Prompt Caching 机制

opencode **有完整的 prompt caching 支持**，分两层：

### 层 1: 消息级缓存断点（`applyCaching`）

`provider/transform.ts:323-372`

策略：**在第 1~2 条 system 消息 + 最后 2 条非 system 消息上打标记**。

```typescript
function applyCaching(msgs, model) {
  const system = msgs.filter(msg => msg.role === "system").slice(0, 2)  // 前2条
  const final = msgs.filter(msg => msg.role !== "system").slice(-2)      // 后2条

  // 不同 provider 用不同标记格式
  const providerOptions = {
    anthropic:        { cacheControl: { type: "ephemeral" } },
    bedrock:          { cachePoint: { type: "default" } },
    openrouter:       { cacheControl: { type: "ephemeral" } },
    openaiCompatible: { cache_control: { type: "ephemeral" } },
    copilot:          { copilot_cache_control: { type: "ephemeral" } },
    alibaba:          { cacheControl: { type: "ephemeral" } },
  }

  // 在标记位置设置 providerOptions
  for (const msg of unique([...system, ...final])) {
    msg.providerOptions = mergeDeep(msg.providerOptions ?? {}, providerOptions)
  }
}
```

**只对特定 provider 启用**：Anthropic / Bedrock / OpenRouter / Alibaba / Copilot。OpenAI 直连**不启用**（OpenAI 自动缓存，不需要显式标记）。

**效果示意**：
```
[System 1 ← 缓存断点]     ← 基础 prompt，很少变化，缓存命中率高
[System 2 ← 缓存断点]     ← 环境信息+指令，每会话基本稳定
[...历史消息...]
[倒数第2条 ← 缓存断点]    ← 较旧的消息
[最新1条]                  ← 每次变化的用户输入
```

这样 LLM 每次只处理尾部新增的消息，前缀部分从缓存读取。

### 层 2: Session 级缓存 Key

`provider/transform.ts:1179-1197`

```typescript
// 同会话的多次请求共享缓存
if (model.providerID.startsWith("opencode")) {
  options["promptCacheKey"] = sessionID
}
if (model.providerID === "venice") {
  options["promptCacheKey"] = sessionID
}
if (model.providerID === "openrouter") {
  options["prompt_cache_key"] = sessionID
}
if (model.api.npm === "@ai-sdk/gateway") {
  options["gateway"] = { caching: "auto" }
}
```

用 sessionID 作为缓存 key，确保同一会话的多轮对话命中服务端缓存。

### 缓存 Token 追踪与计费

`session/session.ts:384-452`

```typescript
const tokens = {
  input: inputTokens - cacheReadTokens - cacheWriteTokens,  // 扣除缓存部分
  cache: { write: cacheWriteTokens, read: cacheReadTokens },
}

// 缓存读取和写入有独立定价
cost = input * inputPrice
     + cache.read * cacheReadPrice    // 通常是 input 的 10%
     + cache.write * cacheWritePrice  // 通常是 input 的 125%
     + output * outputPrice
```

### 缓存感知的溢出检测

`overflow.ts:31-32`

```typescript
// 缓存 token 也算 context window 占用
total = input + output + cache.read + cache.write
```

### 缓存总结

```
历史消息编排: filterCompacted → toModelMessages(过滤/替换/截断) → 加缓存断点
压缩触发:     token 超限(主动) / Provider 报错(被动) / 用户手动
缓存优化:     消息级断点(前2+后2) + Session 级 key + 分开展开计费
```

## 十五、关键技术点总结

| 特性 | 实现方式 |
|------|---------|
| 循环控制 | `while(true)` + finish_reason 检测 |
| 并发控制 | `SessionRunState` 确保单会话单线程 |
| 工具分发 | AI SDK `streamText()` 自动匹配 tool_call |
| 上下文管理 | 自动压缩（compaction agent） |
| 权限控制 | Agent 级别 permission ruleset |
| 扩展机制 | Skills + Plugins + MCP tools |
| 模式切换 | Reminders 注入 <system-reminder> |
| Prompt 定制 | 分层组装 + Agent 覆盖 + 插件 transform |

## 十六、一句话总结

> Agent = **while 循环反复调 LLM** + **LLM 自主决定调哪些工具** + **工具结果自动回传** + **LLM 不调工具时退出**。本质上是一个 **LLM 驱动的自动状态机**。

## Related

- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
