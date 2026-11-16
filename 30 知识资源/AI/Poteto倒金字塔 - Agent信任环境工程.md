---
id: evergreen-20261116-poteto-trust-pyramid
title: Poteto倒金字塔 - Agent信任环境工程
type: evergreen
status: evergreen
created: 2026-11-16
updated: 2026-11-16
tags:
  - 知识/人工智能
  - 主题/AI编程Agent
  - 主题/代码质量
  - 主题/信任工程
aliases:
  - All Code Is Slop
  - Poteto 倒金字塔
  - 审证据不审代码
  - Agent 信任环境
summary: 信任 Agent 不是态度，是环境工程。按成本从低到高：Codebase → Static analysis → Verification → Prose → Human review；每次纠正优先落成确定性检查，人只审证据。
moc:
  - "[[40 知识导航/AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用]]"
source:
  - "[[50 来源资料/文章/All Code Is Slop深度解析 - 如何让Agent写不出垃圾代码|All Code Is Slop深度解析]]"
related:
  - "[[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]"
  - "[[30 知识资源/AI/复杂任务下 AI 连贯开发工作流|复杂任务下 AI 连贯开发工作流]]"
---

# Poteto倒金字塔 - Agent信任环境工程

## TL;DR

当 Agent 一天产出几百个 PR，「逐行 Code Review」物理上失效。信任不是更好的 prompt，而是五层环境：让错误写不出来（Codebase）、写出来就被拦（Static analysis）、意图被证明（Verification）、自然语言只作软约束（Prose）、人只审证据（Human review）。纠正一次，优先落成一条机器检查。

## Key points

- **审证据，不审代码**：录像、截图、状态机图、形式化结果才是评审输入；diff 降级为可选。
- **倒金字塔按单位成本排序**：顶部最宽＝最便宜、覆盖最大；底部最贵。别把力气花在狂加 AGENTS.md。
- **Agent 是复制机**：坏 workaround、辩护型注释、垃圾 Markdown 会被下一个 Agent 当成规范；杂草有复利。
- **Prose 有两种失败**：Miss（漏看）与 Over（过度外推）。Lint 不会被「过度理解」。
- **先修内循环再自动化**：自动化坏循环只会批量产 slop，并烧掉更贵的 token。

## Notes

### 五层模型（从上到下，成本递增）

| 层 | 手段 | 作用 |
| --- | --- | --- |
| ① Codebase | API 设计、架构边界、持续重构 | 让错误写法根本写不出来 |
| ② Static analysis | Lint、类型、编译器、测试、CI | 每次 commit 确定性拦截 |
| ③ Verification | 运行时验证、TLA+/Lean、属性测试 | 证明意图与全路径成立 |
| ④ Prose | Rules、Skills、`AGENTS.md` | 软约束，不可证明被遵守 |
| ⑤ Human review | 人看验证产物 | 兜底；吞吐高时必须退化 |

### Codebase 三把抓手

1. **消灭 nuance API**：凡「用好需要微妙判断」的 API（如裸 `useEffect`），禁掉并只留安全封装。
2. **目录即上下文**：顶层强隔离（如 renderer/main 连类型都不许直引）；文件名自带约束（如 `tick.hot.ts`）。
3. **注释是负债**：不可执行、会过期、会成辩护词；规则写进 lint 报错（Why / Fix / Do NOT）。

### 好 lint 的三行模板

1. **Why**：危害与意图，别只报错码。
2. **Fix**：给出可复制的具体 API。
3. **Do NOT**：堵死「包一层绕过检查」。

### Verification 分工

- 运行时：Feature Map + CLI 点击/截图 → 找错 + 供审。
- `/statechart`：画用户状态迁移 → 检验 Agent 是否懂你。
- TLA+：穷举事件顺序（录屏测不到的乱序）。
- Lean：算法证明。
- 属性测试（如 Bombadil）：UI 随机探索。

顺序不变：能被 Codebase/Static 拦住的，不要跳级上形式化。

### 最小落地顺序

1. 归档近两周被打回的 Agent PR，提取可机器化的重复模式。
2. 先写成本最低的 lint + 依赖边界；养成「纠正即落规则」。
3. 清辩护型注释与垃圾 Markdown。
4. 补 Feature Map + 截图命令；评审先看证据。
5. 只对高代价、顺序敏感链路再上 TLA+/Lean；低风险类别才试自动合并。

### 金句

> Whenever you're correcting your agent, think about how you can make that a deterministic check, instead of adding another line to your rules or your AGENTS.md.

下次被 Agent 气到：先问「能不能变成机器检查」，再问「能不能用架构让它写不出来」——最后才写进 prose。

## Related

- [[50 来源资料/文章/All Code Is Slop深度解析 - 如何让Agent写不出垃圾代码|原文整理（含幻灯片图片与习题）]]
- [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
- [[30 知识资源/AI/复杂任务下 AI 连贯开发工作流|复杂任务下 AI 连贯开发工作流]]
