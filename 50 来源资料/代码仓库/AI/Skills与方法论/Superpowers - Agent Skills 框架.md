---
id: source-20260831-superpowers
title: obra/superpowers - Agentic Skills Framework
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI编程Agent
  - 主题/工程技能
  - 主题/软件工程方法论
author: Jesse Vincent (obra)
source_type: repo
source_url: https://github.com/obra/superpowers
source_author: Jesse Vincent / Prime Radiant
source_date: 2025-10-09
summary: 面向编码代理的完整软件开发方法论与可组合 Skills 框架。通过自动触发的技能链（头脑风暴→设计→计划→子代理执行→TDD→代码审查→分支收尾）约束 AI 代理遵循工程纪律，而非自由发挥。
related:
  - [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
  - [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
  - [[40 知识导航/AI 工具使用|AI 工具使用]]
  - [[50 来源资料/代码仓库/AI/Skills与方法论/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]
---

# obra/superpowers - Agentic Skills Framework

## Source summary

Superpowers 是由 Jesse Vincent（GitHub: obra，Prime Radiant 创始人）开源的 **Agentic Skills 框架 + 完整软件开发方法论**。核心理念：编码代理不应一上来就写代码，而应通过自动触发的 Skills 链强制执行规范化的 SDLC 流程。

- **279k+ Stars / 25k+ Forks**（截至 2026-08-31），AI 编程领域影响力最大的 Skills 项目之一。
- **MIT 协议**开源。
- 支持 **15+ 编码代理平台**：Claude Code、Cursor、Codex、OpenCode、Gemini CLI、GitHub Copilot CLI、Devin、Pi、Hermes 等。
- 与 mattpocock/skills 的"工具箱"路线不同，Superpowers 更偏向 **全流程方法论接管**——Skills 在任务前自动检查并强制触发，不是可选建议。

## Key excerpts

### 核心工作流（The Basic Workflow）

代理在任意任务前自动检查相关 Skills，按序触发：

1. **brainstorming** — 写代码前先通过苏格拉底式提问澄清需求，分块展示设计供用户确认，保存设计文档。
2. **using-git-worktrees** — 设计通过后创建隔离 worktree + 新分支，跑项目 setup，验证测试基线干净。
3. **writing-plans** — 将工作拆成 2-5 分钟粒度的小任务，每个任务含精确文件路径、完整代码、验证步骤。
4. **subagent-driven-development** / **executing-plans** — 每个任务派生子代理 + 两阶段审查（规格合规 → 代码质量），或分批执行带人工检查点。
5. **test-driven-development** — 强制 RED-GREEN-REFACTOR：先写失败测试 → 看失败 → 写最少代码 → 看通过 → 提交。删除测试前写的代码。
6. **requesting-code-review** — 任务间审查，按严重度报告问题，Critical 阻断进度。
7. **finishing-a-development-branch** — 任务完成后验证测试，提供 merge/PR/keep/discard 选项，清理 worktree。

> **The agent checks for relevant skills before any task.** Mandatory workflows, not suggestions.

### Skills 库分类

**Testing**
- `test-driven-development` — RED-GREEN-REFACTOR 循环（含 testing anti-patterns 参考）

**Debugging**
- `systematic-debugging` — 四阶段根因分析（含 root-cause-tracing、defense-in-depth、condition-based-waiting）
- `verification-before-completion` — 确保修复真实有效

**Collaboration**
- `brainstorming` — 苏格拉底式设计精炼
- `writing-plans` — 详细实施计划
- `executing-plans` — 分批执行带检查点
- `dispatching-parallel-agents` — 并发子代理工作流
- `requesting-code-review` / `receiving-code-review` — 代码审查双向流程
- `using-git-worktrees` — 并行开发分支
- `finishing-a-development-branch` — Merge/PR 决策工作流
- `subagent-driven-development` — 两阶段审查的快速迭代

**Meta**
- `writing-skills` — 按最佳实践创建新 Skills（含测试方法论）
- `using-superpowers` — Skills 系统入门引导

### 设计哲学

- **Test-Driven Development** — 永远先写测试
- **Systematic over ad-hoc** — 流程优于猜测
- **Complexity reduction** — 简洁是首要目标
- **Evidence over claims** — 验证后再宣告成功

### 安装方式（Cursor）

```text
/add-plugin superpowers
```

或在插件市场搜索 "superpowers"。

其他平台安装命令各异，详见 README。OpenCode 需单独安装：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
```

## 相关链接

| 类型 | 链接 |
|------|------|
| 仓库 | https://github.com/obra/superpowers |
| Issues | https://github.com/obra/superpowers/issues |
| 发布说明 | https://blog.fsck.com/2025/10/09/superpowers/ |
| 社区 Discord | https://discord.gg/35wsABTejz |
| 版本通知订阅 | https://primeradiant.com/superpowers/ |
| 企业支持 | sales@primeradiant.com |
| 关联 Marketplace | https://github.com/obra/superpowers-marketplace |
| 评估框架 | https://github.com/prime-radiant-inc/superpowers-evals |
| 官网/团队 | https://primeradiant.com |

## My takeaways

1. **与 mattpocock/skills 形成互补对比**：Matt 给"工具箱"（用户按需选用），Superpowers 给"流水线"（自动强制触发）。前者灵活，后者纪律性更强。
2. **子代理驱动开发是亮点**：`subagent-driven-development` 让每个任务由全新子代理执行 + 两阶段审查，适合长时间自主运行（可达数小时）。
3. **Git Worktree 集成**：设计阶段就创建隔离环境，避免污染主分支，与 `finishing-a-development-branch` 形成闭环。
4. **跨平台覆盖极广**：15+ 编码代理平台均有安装指南，是目前兼容性最好的 Skills 框架之一。
5. **贡献门槛较高**：官方明确说不一般接受新 Skills 贡献，且所有 Skills 必须跨平台兼容；测试依赖 `superpowers-evals` drill eval harness。
6. **遥测可选关闭**：brainstorming 的可选视觉伴侣会加载 Prime Radiant logo（含版本号），设 `SUPERPOWERS_DISABLE_TELEMETRY=1` 可关闭。

## Related

- [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]