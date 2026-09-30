---
id: source-20260924-ponytail
title: DietrichGebert/ponytail - 让 AI Agent 少写不必要代码
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
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
summary: 给 Claude Code / Codex / Cursor 等 AI 编程 Agent 用的规则与 Skill 插件。用七级决策阶梯逼 Agent 先判断“该不该写、能不能复用、标准库/原生能力够不够”，再写最小可用实现；官方实测平均少写约 54% 代码且不牺牲安全校验。
related:
  - [[50 来源资料/代码仓库/AI/mattpocock skills - AI编程Agent Skills|mattpocock skills]]
  - [[50 来源资料/代码仓库/AI/Superpowers - Agent Skills 框架|Superpowers]]
  - [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
  - [[40 知识导航/AI 工具使用|AI 工具使用]]
---

# DietrichGebert/ponytail - 让 AI Agent 少写不必要代码

## Source summary

Ponytail 是 Dietrich Gebert 开源的 **AI 编程 Agent 规则/Skill 插件**（不是独立模型，也不自己生成代码）。它把“资深开发者式偷懒”——先质疑需求、优先复用与原生能力、最后才写最小实现——注入到现有 Agent 的上下文里。

- **145k+ Stars**（头条文章写到约 123K；仓库持续增长），MIT 协议。
- 核心口号：**The best code is the code you never wrote.**
- 定位对比：mattpocock/skills 更像“工程纪律工具箱”，Superpowers 更像“全流程流水线”；Ponytail 专攻 **反过度工程 / 少写代码** 这一刀。
- 来源线索：今日头条《GitHub 123K Star！这个项目火了！让 AI 少写代码》（前端Hardy），仓库 https://github.com/DietrichGebert/ponytail

## Key excerpts

### 解决什么问题

AI 编程常见痛点不是“不会写”，而是“太会写、太爱写”：加个日期选择就装依赖、新建组件、封装样式。Ponytail 的典型对照：

```html
<!-- ponytail: browser has one -->
<input type="date">
```

### 七级决策阶梯（How it works）

写代码前停在第一个成立的阶梯：

1. **这段代码需要存在吗？** → 不需要就跳过（YAGNI）
2. **仓库里已有吗？** → 复用，不重写
3. **标准库能做吗？** → 用标准库
4. **平台原生能力？** → 用原生（如 `<input type="date">`）
5. **已安装依赖能做？** → 用已有依赖，不新加包
6. **一行够不够？** → 一行
7. **以上都不行** → 才写“能工作的最小实现”

原则：**对方案偷懒，对读代码不偷懒**；信任边界校验、防数据丢失、安全与无障碍能力不在可砍范围内。

### 强度档位

| 档位 | 行为 |
|------|------|
| lite | 按需求做，但用一句话点出更懒的替代方案，由用户选 |
| full | 强制走阶梯；标准库/原生优先（默认） |
| ultra | YAGNI 极端：先删后加，一行交付并同时挑战其余需求 |

### 基准数据（诚实版）

真实 Claude Code 会话编辑 FastAPI + React 开源仓库，12 个功能任务，相对无 skill 基线（Haiku 4.5, n=4）：

| 指标 | ponytail | 说明 |
|------|----------|------|
| LOC | **-54%** | 过度封装场景可到 94%；已极简代码接近 0 |
| tokens | -22% | |
| cost | -20% | |
| time | -27% | |
| safe | **100%** | 裸 “YAGNI + one-liners” 提示词安全性掉到 95% |

早期单次生成基准曾报 80–94% 少代码，社区 issue #126 指出对照组水分后作者重做 agentic 基准；新数字更可辩护。完整方法见仓库 `benchmarks/results/2026-06-18-agentic.md`。

### 常用命令

| 命令 | 作用 |
|------|------|
| `/ponytail [lite\|full\|ultra\|off]` | 切换强度或关闭；无参数查看当前档 |
| `/ponytail-review` | 审查当前 diff 的过度工程，给出删除清单 |
| `/ponytail-audit` | 整库审计过度工程 |
| `/ponytail-debt` | 回收注释里的 `ponytail:` 延期捷径 |
| `/ponytail-gain` | 展示基准计分板 |
| `/ponytail-help` | 命令速查 |

### 安装速查（部分宿主）

**Claude Code**

```text
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

**Codex**

```bash
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

**Cursor（hooks）**

```bash
git clone https://github.com/DietrichGebert/ponytail
node ponytail/scripts/cursor-hooks.js install
```

也可复制 `.cursor/rules/ponytail.mdc` 做规则-only 方案（与 hooks 二选一）。另支持 Copilot CLI、OpenCode、Gemini CLI、Pi、Hermes、Devin、OpenClaw、Windsurf、Cline 等，详见 README。

可与 [caveman](https://github.com/JuliusBrussee/caveman) 并用：caveman 压缩 Agent **怎么说**，ponytail 压缩 Agent **怎么写**，几乎无重叠。

## 相关链接

| 类型 | 链接 |
|------|------|
| 仓库 | https://github.com/DietrichGebert/ponytail |
| Skill 定义 | https://github.com/DietrichGebert/ponytail/blob/main/skills/ponytail/SKILL.md |
| Agentic 基准 | https://github.com/DietrichGebert/ponytail/blob/main/benchmarks/results/2026-06-18-agentic.md |
| Cursor hooks 说明 | https://github.com/DietrichGebert/ponytail/blob/main/docs/cursor-hooks.md |
| 跨 Agent 移植 | https://github.com/DietrichGebert/ponytail/blob/main/docs/agent-portability.md |
| 头条介绍文 | https://www.toutiao.com/article/7681505347625845299/ |
| 互补项目 caveman | https://github.com/JuliusBrussee/caveman |

## My takeaways

1. **不是代码生成器，是“反膨胀”约束层**：适合已经在用 Claude Code / Cursor / Codex，却常被 Agent 过度封装折磨的场景。
2. **阶梯比口号有用**：把 YAGNI → 复用 → stdlib → 原生 → 已有依赖 → 一行 → 最小实现固化成每次动手前的检查表。
3. **看数字要分场景**：平均 -54% 来自“有过度封装陷阱”的任务；代码本身已经很薄时收益接近零。
4. **安全仍是底线**：比裸“少写”提示词更稳——不会为了短而砍掉路径穿越等校验。
5. **与现有 Skills 生态互补**：mattpocock 管工程纪律流程，Superpowers 管全链路 SDLC，Ponytail 专管“少写、别造轮子”。

## Related

- [[50 来源资料/代码仓库/AI/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]
- [[50 来源资料/代码仓库/AI/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]