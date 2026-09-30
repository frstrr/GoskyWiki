---
id: source-20260924-taste-skill
title: Leonxlnx/taste-skill - 反AI土味前端设计 Skill
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/前端设计
  - 主题/Agent Skills
  - 主题/开源资源
author: Leonxlnx
source_type: repo
source_url: https://github.com/Leonxlnx/taste-skill
source_author: Leonxlnx
source_date: 2026-09-24
summary: 便携 Agent Skills 集合，专治 AI 生成界面的 boilerplate/slop：用 VARIANCE、MOTION、DENSITY 三旋钮调气质，并提供极简/粗野/软高端、图生代码、生图参考板等多变体。
related:
  - "[[50 来源资料/代码仓库/AI/UI设计风格 Skills 资源清单|UI设计风格 Skills 资源清单]]"
  - "[[50 来源资料/代码仓库/AI/ui-ux-pro-max-skill - UI设计智能 Skill|ui-ux-pro-max-skill - UI设计智能 Skill]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# Leonxlnx/taste-skill - 反AI土味前端设计 Skill

## Source summary

**Taste Skill** 自称 *The Anti-Slop Frontend Framework for AI Agents*：用可安装的 Agent Skills 强化布局、字体、动效与间距，避免千篇一律的 AI 默认界面。MIT 许可；可用 `npx skills add` 安装。

- GitHub：`Leonxlnx/taste-skill`（约 **9 万+ Stars**）
- 安装：`npx skills add https://github.com/Leonxlnx/taste-skill`
- 默认 Skill 安装名：`design-taste-frontend`（v2 实验版；可钉 v1：`design-taste-frontend-v1`）

## 核心旋钮（taste-skill）

| 旋钮 | 低 | 高 |
|------|----|----|
| **DESIGN_VARIANCE** | 居中、干净 | 不对称、实验布局 |
| **MOTION_INTENSITY** | 轻 hover | 滚动/磁性等电影感 |
| **VISUAL_DENSITY** | 留白开阔 | 仪表盘式信息密度 |

## Skills 一览（节选）

| 安装名 | 用途 |
|--------|------|
| `design-taste-frontend` | 默认 v2：读 brief、推断设计语言、三旋钮、反破折号堆砌、GSAP 骨架、交付前检查 |
| `gpt-taste` | 面向 GPT/Codex 的更严变体 |
| `image-to-code` | 先出参考图 → 分析 → 实现 |
| `redesign-existing-projects` | 先审计再改现有 UI |
| `high-end-visual-design` / `minimalist-ui` / `industrial-brutalist-ui` | 软高端 / 编辑极简 / 工业粗野 |
| `full-output-enforcement` | 禁止半成品与占位注释 |
| `imagegen-frontend-web` 等 | 只出参考图（Web/移动/品牌板） |

## 适用场景

- 同一产品需求要多套气质对比
- Cursor / Claude Code / Codex 写前端时系统性压「AI 土味」
- 需要 image→code 或纯参考板工作流

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/Leonxlnx/taste-skill |
| **赞助** | https://github.com/sponsors/Leonxlnx |
| **联系** | hello@tasteskill.dev |
| **清单出处** | [[50 来源资料/代码仓库/AI/UI设计风格 Skills 资源清单\|UI设计风格 Skills 资源清单]] |

## My takeaways

1. 找「**可调旋钮 + 反 slop 规则**」时优先本仓库；与 ui-ux-pro-max 的品类推理互补。
2. 默认已切 v2，旧项目行为敏感时显式安装 v1。
3. 实现类与生图类 Skill 同仓同 CLI，注意按交付物选型，避免一次全装。

## Related

- [[50 来源资料/代码仓库/AI/UI设计风格 Skills 资源清单|UI设计风格 Skills 资源清单]]
- [[50 来源资料/代码仓库/AI/ui-ux-pro-max-skill - UI设计智能 Skill|ui-ux-pro-max-skill - UI设计智能 Skill]]
- [[50 来源资料/代码仓库/AI/frontend-design-pro-demo - 11种前端美学 Demo 与 Skill|frontend-design-pro-demo - 11种前端美学 Demo 与 Skill]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
