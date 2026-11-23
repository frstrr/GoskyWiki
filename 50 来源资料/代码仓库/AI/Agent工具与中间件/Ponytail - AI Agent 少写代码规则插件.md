---
id: source-20260924-ponytail
title: DietrichGebert/ponytail - 让 AI Agent 少写不必要代码
type: source
status: active
created: 2026-09-24
updated: 2026-11-23
tags:
  - 来源/代码仓库
  - 主题/AI编程Agent
  - 主题/工程技能
  - 主题/YAGNI
author: Dietrich Gebert
source_type: repo
source_url: https://github.com/DietrichGebert/ponytail
source_author: Dietrich Gebert
source_date: 2026-06
summary: 给 Claude Code / Codex / Cursor 等 AI 编程 Agent 用的规则与 Skill 插件（Ponytail 5）。用决策阶梯逼 Agent 先判断「该不该写、能不能复用、标准库/原生能力够不够」，再写最小可用实现；官方 2026-10-07 基准相对无 skill：约 -53% 代码、-41% 时间、-26% 成本、-45% tokens，风险逻辑带测试约 98%。
related:
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/mattpocock skills - AI编程Agent Skills|mattpocock skills]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# DietrichGebert/ponytail - 让 AI Agent 少写不必要代码

## Source summary

Ponytail 是 Dietrich Gebert 开源的 **AI 编程 Agent 规则/Skill 插件**（不是独立模型，也不自己生成代码）。它把「资深开发者式偷懒」——先质疑需求、优先复用与原生能力、最后才写最小实现——注入到现有 Agent 的上下文里。

- **约 197k Stars**（持续增长），MIT 协议；官网 https://ponytail.dev
- 核心口号：**The best code is the code you never wrote.**
- 当前主线为 **Ponytail 5**（相对 v4 再降代码量与成本；5.3 起 Claude Code 增加 band 状态条与本地 Board）。
- 定位对比：mattpocock/skills 更像「工程纪律工具箱」，Superpowers 更像「全流程流水线」；Ponytail 专攻 **反过度工程 / 少写代码** 这一刀。
- 仓库：https://github.com/DietrichGebert/ponytail

## Key excerpts

### 解决什么问题

AI 编程常见痛点不是「不会写」，而是「太会写、太爱写」：加个日期选择就装依赖、新建组件、封装样式。Ponytail 5 的典型对照（同仓库已有 `Input`、浏览器自带 date picker）：

- 无 Ponytail：约 335 行日历/依赖方案
- 有 Ponytail 5：把现有组件与 `<input type="date">` 拼起来，约 10 行

### 决策阶梯（How it works）

阶梯在**理解问题之后**才跑：先读改动触及的代码、追真实数据流，再选最低一档。对方案偷懒，对读代码不偷懒。

写代码前停在第一个成立的阶梯：

1. **这段代码需要存在吗？** → 不需要就跳过（YAGNI）
2. **仓库里已有吗？** → 复用，不重写
3. **标准库能做吗？** → 用标准库
4. **平台原生能力？** → 用原生（如 `<input type="date">`）
5. **已安装依赖能做？** → 用已有依赖，不新加包
6. **一行够不够？** → 一行
7. **以上都不行** → 才写「能工作的最小实现」

硬约束：信任边界校验、防数据丢失、安全与无障碍**不在可砍范围**。带分支/循环/解析器/金钱/安全的逻辑要留下小测试；每次回复末尾交代跳过项、未检查项与风险。

规则不是「最少 tokens」，而是只写任务需要的东西；代码变短是副作用。

### 强度档位

| 档位 | 行为 |
|------|------|
| lite | 按需求做，但用一句话点出更懒的替代方案，由用户选 |
| full | 强制走阶梯；标准库/原生优先（默认） |
| ultra | YAGNI 极端：先删后加，一行交付并同时挑战其余需求 |
| off | 关闭 |

默认可用 `~/.config/ponytail/config.json` 或环境变量 `PONYTAIL_DEFAULT_MODE`（Windows：`%APPDATA%\ponytail\config.json`）。

### 基准数据（Ponytail 5，2026-10-07）

Claude Code + Opus 5.5，39 任务 × 5 runs（含真实 FastAPI + React 仓库与安全/隐私类 hidden check）。相对无 skill：

| 指标 | Ponytail 5 | 说明 |
|------|------------|------|
| LOC | **-53%** | 区间约 -63%～-41% |
| gzip 信息量 | -41% | |
| output tokens | **-45%** | |
| cost | **-26%** | 均次约 $0.142 vs 无 skill $0.200 |
| time | **-41%** | |
| 需测逻辑有测试 | **98%** | 无 skill 约 68%；v4.13 约 95% |
| 六项安全任务 | **30/30** | 少写代码未牺牲安全 |

盲评：Ponytail 5 回复相对旧版 Ponytail 约 110:67 胜出。完整方法与分任务表见 `benchmarks/results/2026-10-07-agentic.md`。旧版（约 -54% LOC）见 `2026-06-18-agentic.md`，可作历史对照。

### 审查与审计（Ponytail 5 重建）

- `/ponytail-review`：不再只找「能删什么」。按会被 pager 叫起来的资深视角读**改动触及的代码**（不只 diff），查 bug、安全、真实负载、缺测、慢路径、可砍项；每条发现说明现状、会怎样坏、怎么修、不修后果。可用自然语言缩小范围：`uncommitted` / `staged` / `branch` / PR 链接。
- `/ponytail-audit`：对整库做同样检查；先画入口、数据流与负载预期，再按优先级排序。旧审计几乎只列「该删什么」。

### 5.3：band 与 Board（Claude Code）

Claude Code ≥ 2.1.290 时，提示符上方可显示 band：当前任务成本、tokens、agent 时间、复用既有代码频率、上次改动后测试是否通过。按钮可切档位、跑 review/audit、复制任务为 Markdown、折叠为一行。`/ponytail band` 开关显示。Board 为本地页，默认数据在 `~/.config/ponytail/band/`；`/ponytail band import` 可导入历史 Claude Code 会话。不在 VS Code chat、云端会话或 `claude -p` 中显示。

### 常用命令

| 命令 | 作用 |
|------|------|
| `/ponytail [lite\|full\|ultra\|off]` | 切换强度或关闭；无参数：关闭时打开默认档，否则报告当前档 |
| `/ponytail band [on\|off]` | 仅 Claude Code：显示/隐藏 band |
| `/ponytail band import` | 仅 Claude Code：导入历史会话到 Board |
| `/ponytail-review` | 审查当前改动（bug/安全/负载/缺测/慢路径/可砍） |
| `/ponytail-audit` | 整库审计，重要项优先 |
| `/ponytail-debt` | 回收 `shortcut:` 延期注释成台账（旧笔记里的 `ponytail:` 标记以仓库当前约定为准） |
| `/ponytail-gain` | 展示基准计分板 |
| `/ponytail-help` | 命令速查 |

完整 slash 命令需 skill 能力宿主（Claude Code、Codex、Devin CLI、OpenCode、Gemini 等）。Cursor 若用 hooks：以普通消息发 `/ponytail` 仅切档；规则文件-only 方案无命令。Codex 下可能是 `$ponytail:ponytail-review` 命名空间形式。

### 安装速查（部分宿主）

**Claude Code**（两条分开发送）

```text
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

**Codex**

```bash
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

然后在 `/hooks` 中信任两个 lifecycle hooks，并新开线程。

**Cursor（hooks）**

```bash
git clone https://github.com/DietrichGebert/ponytail
node ponytail/scripts/cursor-hooks.js install
```

也可复制 `.cursor/rules/ponytail.mdc` 做规则-only（与 hooks **二选一**）。通用：`npx skills add DietrichGebert/ponytail`，或复制 [`AGENTS.md`](https://github.com/DietrichGebert/ponytail/blob/main/AGENTS.md) / [`skills/ponytail/SKILL.md`](https://github.com/DietrichGebert/ponytail/blob/main/skills/ponytail/SKILL.md)。更多宿主见 [INSTALL.md](https://github.com/DietrichGebert/ponytail/blob/main/INSTALL.md)。

只从 GitHub `DietrichGebert/ponytail` 或 npm `@dietrichgebert/ponytail` 安装；官方不发布 `.exe`/`.dll`。

可与 [caveman](https://github.com/JuliusBrussee/caveman) 并用：caveman 压缩 Agent **怎么说**，ponytail 压缩 Agent **怎么写**，几乎无重叠。

## 相关链接

| 类型 | 链接 |
|------|------|
| 仓库 | https://github.com/DietrichGebert/ponytail |
| 官网 | https://ponytail.dev |
| Skill 定义 | https://github.com/DietrichGebert/ponytail/blob/main/skills/ponytail/SKILL.md |
| 紧凑规则 AGENTS.md | https://github.com/DietrichGebert/ponytail/blob/main/AGENTS.md |
| 安装说明 | https://github.com/DietrichGebert/ponytail/blob/main/INSTALL.md |
| Ponytail 5 Agentic 基准 | https://github.com/DietrichGebert/ponytail/blob/main/benchmarks/results/2026-10-07-agentic.md |
| 旧版 Agentic 基准 | https://github.com/DietrichGebert/ponytail/blob/main/benchmarks/results/2026-06-18-agentic.md |
| Cursor hooks 说明 | https://github.com/DietrichGebert/ponytail/blob/main/docs/cursor-hooks.md |
| 跨 Agent 移植 | https://github.com/DietrichGebert/ponytail/blob/main/docs/agent-portability.md |
| 头条介绍文（历史线索） | https://www.toutiao.com/article/7681505347625845299/ |
| 互补项目 caveman | https://github.com/JuliusBrussee/caveman |

## My takeaways

1. **不是代码生成器，是「反膨胀」约束层**：适合已经在用 Claude Code / Cursor / Codex，却常被 Agent 过度封装折磨的场景。
2. **阶梯比口号有用**：把 YAGNI → 复用 → stdlib → 原生 → 已有依赖 → 一行 → 最小实现固化成每次动手前的检查表；且先读后砍。
3. **看数字要分场景**：Ponytail 5 平均约 -53% LOC 来自「有过度封装陷阱」的任务；代码本身已经很薄时收益接近零。
4. **安全与测试仍是底线**：少写不等于砍校验；风险逻辑应留小测，review/audit 也按「会坏什么」来排。
5. **与现有 Skills 生态互补**：mattpocock 管工程纪律流程，Superpowers 管全链路 SDLC，Ponytail 专管「少写、别造轮子」。
6. **Claude Code 用户可关注 band/Board**：把成本、复用率与测试结果压到会话表面上，便于自觉约束。

## Related

- [[50 来源资料/代码仓库/AI/Skills与方法论/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
