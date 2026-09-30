---
id: source-20260831-vibe-coding-cn
title: Vibe Coding CN - AI 结对编程指南
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI编程
  - 主题/提示词工程
  - 主题/Agent
  - 主题/开发方法论
author: tukuaiai / 2025Emma
source_type: repo
source_url: https://github.com/2025Emma/vibe-coding-cn
source_author: tukuaiai
source_date: 2026-08-31
summary: 中文 Vibe Coding 终极工作站——规划驱动 + 上下文固定 + AI 结对执行；含成体系提示词库、Skills 技能库、方法论文档与 prompts-library 工具链，适合 AI 辅助软件/游戏开发全流程。
related:
  - "[[50 来源资料/代码仓库/AI/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]"
---

# Vibe Coding CN - AI 结对编程指南

## Source summary

**Vibe Coding CN** 是一个与 AI 结对编程的**终极工作流程指南**（中文社区版），旨在帮助开发者丝滑地将想法变为可维护代码。核心理念是 **规划驱动 + 上下文固定 + AI 结对执行**，强调「规划就是一切」——避免让 AI 自主规划导致代码库失控。

- GitHub：`2025Emma/vibe-coding-cn`（MIT 许可，约 **2.2 万+ Stars**）
- 上游/维护：`tukuaiai/vibe-coding-cn`（README 中 Issue/PR 链接指向此仓库）
- 原始灵感：EnzeD 的 [vibe-coding](https://github.com/EnzeD/vibe-coding) 工作流（本仓库为中文增强版）
- AI 解读： [zread.ai/tukuaiai/vibe-coding-cn](https://zread.ai/tukuaiai/vibe-coding-cn/1-overview)

## 核心定位

| 维度 | 说明 |
|------|------|
| **工作流** | 需求 → 上下文文档 → 实施计划 → 分步实现 → 自测 → 进度记录，全程可复盘、可移交 |
| **提示词体系** | `coding_prompts`（需求澄清/计划/执行链）、`system_prompts`（行为边界）、`assistant_prompts`、`user_prompts` |
| **Skills 库** | `i18n/zh/skills/` 模块化技能，含生成 Skill 的元 Skill |
| **知识库** | 方法论、架构模板、开发经验、系统提示词构建原则等 |
| **工具链** | `prompts-library` 支持 Excel ↔ Markdown 互转，Makefile 自动化 |

## 方法论框架

### 元方法论（递归自优化）

- **α-提示词（生成器）**：生成其他提示词/技能
- **Ω-提示词（优化器）**：优化其他提示词/技能
- 循环：创生 → 自省进化 → 创造 → 递归飞跃，使系统持续自我超越

### 道法术器

| 层级 | 要点 |
|------|------|
| **道** | 凡 AI 能做的就不人工做；上下文是第一性要素；先结构后代码；奥卡姆剃刀 |
| **法** | 一句话目标+非目标；能抄不写；按职责拆模块；接口先行；文档即上下文 |
| **术** | 明确能改/不能改；Debug 给预期 vs 实际 + 最小复现；代码一多就切会话 |
| **器** | IDE（Cursor/VSCode/Neovim）、AI 模型（Claude Opus、Codex、gpt-5.x）、Augment/Zread/tmux 等 |

## 推荐工具栈

### AI 模型（编码性能分级）

- **第一梯队**：`codex-5.1-max-xhigh`、`claude-opus-4.5-xhigh`、`gpt-5.2-xhigh`
- **第二梯队**：`claude-sonnet-4.5`、`kimi-k2-thinking`、`gemini-3.0-pro` 等
- **第三梯队**：`qwen3`、`SWE`、`grok4`

### 入门推荐

- **Claude Opus 4.5** + Claude Code（CLI 或 VSCode 扩展）
- **gpt-5.1-codex (xhigh)** + Codex CLI
- Cursor 可用但 README 认为不如原生 Claude Code / Codex CLI 强大

## 完整设置流程（摘要）

1. **游戏/产品设计文档** → `game-design-document.md`（非游戏项目可换 PRD）
2. **技术栈** → `tech-stack.md`，用 `/init` 生成 `CLAUDE.md` / `AGENTS.md` 规则
3. **实施计划** → `implementation-plan.md`（分步指令，不含代码，每步含验证测试）
4. **Memory Bank** → `memory-bank/` 目录集中存放上下文、进度、架构文档
5. **分步执行** → 每步验证后更新 `progress.md` 和 `architecture.md`，切会话继续

## 仓库结构（核心目录）

```
i18n/zh/
├── documents/          # 方法论、模板、教程
├── prompts/
│   ├── coding_prompts/     # 编程流程专用提示词
│   ├── system_prompts/     # 系统级行为约束
│   ├── assistant_prompts/
│   └── user_prompts/
└── skills/             # 可集成 Skills 技能库
libs/external/prompts-library/   # Excel ↔ MD 提示词管理工具
```

## 适用场景

- 想用 **AI 结对编程** 从 0 到 1 构建软件/游戏/应用
- 需要**成体系的提示词 + Skills + 方法论**，而非零散 prompt
- 希望建立 **Memory Bank + 实施计划 + 分步验证** 的可审计开发流水线
- 中文社区资源：Telegram 交流群、提示词在线表格、元提示词库
- 与 unicore 等游戏框架配合：GDD → 技术栈 → 模块化实施计划 → AI 分步开发

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库（用户提供）** | https://github.com/2025Emma/vibe-coding-cn |
| **上游仓库** | https://github.com/tukuaiai/vibe-coding-cn |
| **原始 vibe-coding** | https://github.com/EnzeD/vibe-coding |
| **Cursor 版指南 (v1.1)** | https://github.com/EnzeD/vibe-coding/tree/1.1.1 |
| **AI 解读 (Zread)** | https://zread.ai/tukuaiai/vibe-coding-cn/1-overview |
| **元提示词库（Google 表格）** | https://docs.google.com/spreadsheets/d/1ngoQOhJqdguwNAilCl1joNwTje7FWWN9WiI2bo5VhpU/edit?gid=1770874220 |
| **在线提示词数据库** | https://docs.google.com/spreadsheets/d/1ngoQOhJqdguwNAilCl1joNwTje7FWWN9WiI2bo5VhpU/edit?gid=2093180351 |
| **Telegram 交流群** | https://t.me/glue_coding |
| **Telegram 频道** | https://t.me/tradecat_ai_channel |
| **Skills 生成器** | https://github.com/yusufkaraaslan/Skill_Seekers |
| **第三方系统提示词库** | https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools |
| **维护者 GitHub** | https://github.com/tukuaiai |
| **维护者 X/Twitter** | https://x.com/123olp |

## My takeaways

1. **不是代码框架，是 AI 编程操作系统**：提供完整的方法论、提示词库、Skills 和 Memory Bank 工作流，适合作为 AI 辅助开发的「 playbook 」参考。
2. **与 Agent Skills 生态互补**：本仓库侧重 Vibe Coding 全流程（规划→执行→复盘），mattpocock/skills 等侧重具体 Agent 技能定义；可组合使用。
3. **对 unicore 游戏开发有直接参考价值**：README 内置游戏开发示例（GDD、实施计划、分步构建），与 Unity 游戏框架的 AI 辅助开发场景高度契合。
4. **社区活跃、Star 极高**：2 万+ Stars，中文文档完善，Telegram 社区和在线提示词表格降低上手门槛。
5. **注意 fork 关系**：用户提供的 `2025Emma/vibe-coding-cn` 与上游 `tukuaiai/vibe-coding-cn` 内容基本一致；Issue/PR 建议指向上游仓库。

## Related

- [[50 来源资料/代码仓库/AI/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]
