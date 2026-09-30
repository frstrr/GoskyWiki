---
id: source-20260924-openmontage
title: OpenMontage - Agent 化视频制作系统
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/视频制作
  - 主题/Agent
  - 主题/多智能体工作流
author: calesthio
source_type: repo
source_url: https://github.com/calesthio/OpenMontage
source_author: calesthio
source_date: 2026-09-24
summary: 开源 Agent 化视频制作系统：用 AI 编程助手当编排器，Pipeline + 导演技能 + 工具注册表驱动调研/脚本/素材/剪辑/合成；十余条流水线、大量工具与技能文件；AGPL-3.0。
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/Headroom - LLM 上下文压缩中间件|Headroom]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/codebase-memory-mcp - 代码库知识图谱 MCP|codebase-memory-mcp]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# OpenMontage - Agent 化视频制作系统

## Source summary

**OpenMontage** 自称「首个开源、Agent 驱动的视频制作系统」：把你的 AI 编程助手变成制片工作室——用自然语言描述需求，由 Agent 按流水线完成调研、脚本、素材生成、配音、剪辑、字幕与合成。

- 开源项目，GitHub：[calesthio/OpenMontage](https://github.com/calesthio/OpenMontage)
- 作者：[@calesthio](https://github.com/calesthio)
- 语言：以 Python 工具层 + Markdown/YAML 技能与清单为主（Agent-first：编排在指令里，不在巨型编排器代码里）
- 约 **50k+ stars / 6k+ forks**（截至 2026-09-24）
- 许可证：**AGPL-3.0**
- 官网：[openmontage.video](https://www.openmontage.video/)

头条文描述量级约为：**12 条制作流水线、52+ 工具、500+ Agent 技能**；README 后续宣传已到 **100+ tools / 700+ skill 文件**（随仓库演进，以当前 README 为准）。

## 解决什么问题

| 痛点 | 方案 |
|------|------|
| 视频制作环节多、工具散 | **Pipeline 驱动**：`pipeline_defs/` 清单 + 阶段导演技能 |
| Agent 乱发挥流程 | 契约优先：先读 `AGENT_GUIDE.md` / `PROJECT_CONTEXT.md`，禁止即兴改流程 |
| 不会选模型/供应商 | `tool_registry` 的 `support_envelope` / `provider_menu` 做能力发现与预检 |
| 生成质量靠玄学 prompt | Layer3 `.agents/skills/`：调用生成工具前必须读对应技能 |

## 核心架构（三层）

1. **Layer 1 工具**：`tools/tool_registry.py` 自动发现，声明依赖与能力边界  
2. **Layer 2 项目约定**：Pipeline manifest、阶段导演、checkpoint / reviewer 元技能  
3. **Layer 3 技术技能**：各供应商/模型的提示与参数最佳实践（生成前必读）

工作流要点：选 pipeline → 读 manifest → registry 预检 → **每个 stage 先读 `*-director.md`** → 调工具前读 Layer3 → 创意点人工审批（checkpoint）。

## 支持的 Agent 面（示例）

Claude Code、Cursor、GitHub Copilot、Codex、Windsurf 等（仓库内有对应 `CLAUDE.md` / `CURSOR.md` / `CODEX.md` 等入口文件）。

## 适用场景

- 小团队/个人想用 Coding Agent **端到端出片**
- 需要可重复、可审查的 **制片流水线** 而非一次性聊天生成
- 研究「Agent-first：智能在指令、代码只做工具」的架构范式

**不太适合**：不能接受 **AGPL-3.0**；只想要一行 API 出短视频、不要技能/流水线纪律；无任何生成服务 API Key 且不想配本地工具链。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/calesthio/OpenMontage |
| **中文 README** | https://github.com/calesthio/OpenMontage/blob/main/README_zh-CN.md |
| **Agent Guide** | https://github.com/calesthio/OpenMontage/blob/main/AGENT_GUIDE.md |
| **项目上下文** | https://github.com/calesthio/OpenMontage/blob/main/PROJECT_CONTEXT.md |
| **官网** | https://www.openmontage.video/ |
| **介绍文章（头条）** | https://www.toutiao.com/article/7685280926175117876/ |

## My takeaways

1. **定位**：视频制片「操作系统」——流水线 + 技能，不是单一文生视频 API 包装。
2. **Agent 就是编排器**：与传统 Python 编排框架相反，值得学其契约与阶段技能设计。
3. 许可是 AGPL，商用/闭源衍生前先评估合规。

## Related

- [[50 来源资料/代码仓库/AI/Agent工具与中间件/Headroom - LLM 上下文压缩中间件|Headroom - LLM 上下文压缩中间件]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/codebase-memory-mcp - 代码库知识图谱 MCP|codebase-memory-mcp - 代码库知识图谱 MCP]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
