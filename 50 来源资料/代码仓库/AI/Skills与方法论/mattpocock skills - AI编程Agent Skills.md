---
id: source-20260618-mattpocock-skills
title: mattpocock/skills - Skills for Real Engineers
type: source
status: active
created: 2026-06-18
updated: 2026-06-18
tags:
  - 来源/代码仓库
  - 主题/AI编程Agent
  - 主题/工程技能
author: Matt Pocock
source_type: repo
source_url: https://github.com/mattpocock/skills
source_author: Matt Pocock
source_date: 2026-06-18
summary: Matt Pocock 开源的 AI 编程 Agent Skills 集合，针对 Claude Code / Codex 等编码代理常见失败模式，提出一套小而可组合的工程纪律技能。
related:
  - [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
  - [[40 知识导航/AI 工具使用|AI 工具使用]]
---

# mattpocock/skills - Skills for Real Engineers

## Source summary

Matt Pocock（TypeScript 社区知名教育者）开源的个人 Agent Skills 库，从自己的 `.claude` 目录中沉淀出来。核心理念：**AI 编程时代，软件工程基本功比以往更重要**。不是 vibe coding，而是把数十年工程经验浓缩为可复用、可组合的 Skills，让 AI 代理遵循纪律而非自由发挥。

- 134k Stars / 11.6k Forks，影响力极大。
- MIT 协议开源。
- 通过 `npx skills@latest add mattpocock/skills` 一键安装到任何编码代理。

## Key excerpts

### 四大 AI 编程失败模式及修复方案

#### 1. Agent 没做我想要的事（对齐问题）

> "No-one knows exactly what they want" — The Pragmatic Programmer

**修复：Grilling Session（拷问会话）**

- `/grill-me`：非编码场景，让 agent 反复追问直到决策树每个分支都解决。
- `/grill-with-docs`：编码场景，grilling + 同步构建领域模型 + 更新 CONTEXT.md 和 ADR。

#### 2. Agent 输出太啰嗦（共享语言缺失）

> "With a ubiquitous language, conversations and code are derived from the same domain model." — Eric Evans, DDD

**修复：Shared Language / CONTEXT.md**

- 建立项目专属术语表，让 agent 用 1 个词代替 20 个词。
- 示例：不说“课程中的一个 section 内的 lesson 被实例化到文件系统”，而说“materialization cascade”。
- 附带收益：变量命名一致、代码更易导航、token 消耗更少。

#### 3. 代码跑不起来（反馈循环缺失）

> "Always take small, deliberate steps. The rate of feedback is your speed limit." — The Pragmatic Programmer

**修复：反馈循环**

- `/tdd`：Red-Green-Refactor 循环，先写失败测试再修复。
- `/diagnosing-bugs`：纪律化调试循环，复现 → 最小化 → 假设 → 插桩 → 修复 → 回归测试。

#### 4. 代码变成大泥球（架构腐化加速）

> "Invest in the design of the system every day." — Kent Beck

**修复：主动架构关注**

- `/improve-codebase-architecture`：扫描代码库发现“深化”机会，输出可视化 HTML 报告。
- `/to-prd`：在创建 PRD 前先追问涉及哪些模块。
- 建议每隔几天运行一次架构改进。

### Skills 分类体系

| 类型 | 调用方 | 职责 |
|------|--------|------|
| User-invoked | 用户手动触发（如 `/grill-me`） | 编排流程 |
| Model-invoked | 用户或 agent 自动触发 | 持有可复用纪律 |

User-invoked 可调用 model-invoked，但不可调用另一个 user-invoked。

### 核心 Skills 清单

**Engineering（日常编码）**：

- `ask-matt`：路由技能，帮你判断该用哪个 skill。
- `grill-with-docs`：拷问 + 领域建模 + ADR。
- `triage`：issue 分诊状态机。
- `improve-codebase-architecture`：代码库架构扫描与改进。
- `to-issues`：将 PRD 拆分为可独立抓取的 vertical slice issue。
- `to-prd`：对话转 PRD。
- `prototype`：构建一次性原型验证设计。
- `tdd`：TDD Red-Green-Refactor（model-invoked）。
- `diagnosing-bugs`：纪律化调试（model-invoked）。
- `domain-modeling`：领域模型构建与锐化（model-invoked）。
- `codebase-design`：深模块设计纪律（model-invoked）。

**Productivity（通用工作流）**：

- `grill-me`：通用拷问会话。
- `handoff`：对话压缩为交接文档。
- `teach`：多轮教学。
- `grilling`：拷问循环核心（model-invoked）。

**Misc**：

- `git-guardrails`：Claude Code hooks 阻止危险 git 命令。
- `setup-pre-commit`：Husky + lint-staged + Prettier。

## My takeaways

1. **Skills 设计哲学值得学习**：小而专注、可组合、不试图接管全流程。对比 GSD/BMAD/Spec-Kit 这类“接管流程”的方案，Matt 选择了“给你工具箱”路线。
2. **Grilling Session 是最高 ROI 实践**：在写代码之前花时间对齐，远比事后返工高效。
3. **CONTEXT.md / Shared Language 是宝藏技术**：让 AI 用领域语言思考，而非通用语言废话。
4. **User-invoked vs Model-invoked 分层**：编排逻辑与执行纪律分离，是 skills 设计的好模式。
5. **架构腐化在 AI 编程下加速**：因为 AI 能极快写代码，所以熵增也更快，需要更频繁地关注架构。

## Related

- [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
