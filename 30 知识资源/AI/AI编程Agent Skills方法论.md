---
id: evergreen-20260618-ai-agent-skills
title: AI编程Agent Skills方法论
type: evergreen
status: evergreen
created: 2026-06-18
updated: 2026-06-18
tags:
  - 知识/人工智能
  - 主题/AI编程Agent
  - 主题/软件工程
aliases:
  - AI编码纪律
  - Agent Skills 设计模式
  - Matt Pocock Skills方法论
summary: AI编程代理的核心失败模式及对应纪律化技能体系。通过 Grilling 对齐、Shared Language 降熵、TDD 反馈循环、主动架构关注四个维度，把软件工程基本功转化为 AI 时代可复用的 Agent Skills。
moc:
  - [[40 知识导航/AI 编程 Agent]]
  - [[40 知识导航/AI 工具使用]]
source:
  - [[50 来源资料/代码仓库/AI/Skills与方法论/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]
related:
  - [[30 知识资源/AI/复杂任务下 AI 连贯开发工作流|复杂任务下 AI 连贯开发工作流]]
---

# AI编程Agent Skills方法论

## TL;DR

AI编程代理有四个典型失败模式：对齐偏差、输出冗余、代码不可用、架构腐化。每个失败模式都有对应的“技能”（Skill）来约束代理行为，形成纪律。核心思想是：**让代理遵循工程纪律，而非自由发挥；技能应小而可组合，不试图接管全流程**。

## Key points

- **先对齐再动手**：Grilling Session 是最高 ROI 实践，写代码前花时间让 agent 追问直到需求完全澄清。
- **用领域语言思考**：共享术语表（CONTEXT.md）让 agent 用精确的领域词汇替代通用废话，同时改善命名、导航和 token 效率。
- **反馈循环是质量底线**：TDD Red-Green-Refactor + 纪律化调试循环，确保 agent 产出的代码有验证。
- **架构需要主动投资**：AI 能极快写代码，因此代码熵增也更快，必须定期运行架构扫描与改进。

## Notes

### 四大失败模式与四维修复

| 失败模式 | 根因 | 修复技能 | 核心动作 |
|----------|------|----------|----------|
| Agent 没做我想要的事 | 对齐 gap | Grilling Session | 反复追问直到决策树全部分支解决 |
| Agent 输出太啰嗦 | 缺乏共享语言 | CONTEXT.md / Domain Modeling | 建立项目术语表，让 1 词 = 20 词 |
| 代码跑不起来 | 无反馈循环 | TDD + Bug Diagnosis | Red→Green→Refactor + 复现→最小化→修复 |
| 变成大泥球 | 架构腐化加速 | Architecture Scan | 定期扫描+深化+PRD前审模块边界 |

### Skill 设计模式

**分层原则**：User-invoked（编排）vs Model-invoked（纪律）

- User-invoked Skills：由用户显式触发（如 `/grill-me`），负责流程编排。
- Model-invoked Skills：agent 可自动选用（如 `/tdd`），持有可复用纪律。
- 编排层可调用纪律层，反之不行；同层不互相调用。

**设计原则**：

1. 小而专注：一个 skill 只做一件事。
2. 可组合：多个 skill 可以串联完成复杂流程。
3. 不接管控制权：给工具箱，不是接管流程（对比 GSD/BMAD/Spec-Kit）。
4. 模型无关：任何 LLM 都能用。

### Grilling Session 详解

最有效的单一技能。核心循环：

1. Agent 针对你的计划/设计不断追问。
2. 每个模糊点都要澄清。
3. 决策树的每个分支都要 resolve。
4. 所有结论记录下来（PRD / ADR / CONTEXT.md）。

编码版（`/grill-with-docs`）额外做：

- 构建/更新领域术语表。
- 生成/更新 ADR（Architecture Decision Record）。
- 追问涉及哪些模块，防止架构越界。

### 实践建议

1. **每次开始新任务前**：先跑 Grilling Session。
2. **每个项目初始化时**：建立 CONTEXT.md / Shared Language。
3. **每隔几天**：运行 `/improve-codebase-architecture`。
4. **写代码时**：开启 TDD 循环。
5. **遇到 bug 时**：遵循纪律化调试循环，不要让 agent 瞎猜。

### 与其他方法对比

| 方案 | 策略 | 优势 | 劣势 |
|------|------|------|------|
| GSD/BMAD | 接管全流程 | 一站式 | 失控感，bug 难排查 |
| Spec-Kit | 从 spec 驱动 | 结构清晰 | 前期太重 |
| **Matt Pocock Skills** | 工具箱 + 纪律 | 灵活可组合 | 需要自己组装 |
| 无任何约束 | 自由发挥 | 零成本 | 质量靠运气 |

## Related

- [[50 来源资料/代码仓库/AI/Skills与方法论/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]
- [[30 知识资源/AI/复杂任务下 AI 连贯开发工作流|复杂任务下 AI 连贯开发工作流]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
