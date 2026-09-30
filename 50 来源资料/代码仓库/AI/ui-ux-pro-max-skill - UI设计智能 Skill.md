---
id: source-20260924-ui-ux-pro-max-skill
title: nextlevelbuilder/ui-ux-pro-max-skill - UI设计智能 Skill
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
author: nextlevelbuilder
source_type: repo
source_url: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
source_author: nextlevelbuilder
source_date: 2026-09-24
summary: 面向多端多栈的 UI/UX 设计智能 Agent Skill；v2 旗舰能力为 Design System Generator，按品类推理风格/色板/字体/落地页结构，并附反模式与交付前检查清单。
related:
  - "[[50 来源资料/代码仓库/AI/UI设计风格 Skills 资源清单|UI设计风格 Skills 资源清单]]"
  - "[[50 来源资料/代码仓库/AI/taste-skill - 反AI土味前端设计 Skill|taste-skill - 反AI土味前端设计 Skill]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# nextlevelbuilder/ui-ux-pro-max-skill - UI设计智能 Skill

## Source summary

**ui-ux-pro-max-skill** 为 AI 编码代理提供「设计智能」：不是单一审美 prompt，而是可检索的风格/色板/字体/产品类型/UX 准则库，并在 v2 用 **Design System Generator** 按需求一次生成完整设计系统。

- GitHub：`nextlevelbuilder/ui-ux-pro-max-skill`（约 **13 万+ Stars**，体量极大）
- 官网：https://uupm.cc 、 https://ui-ux-pro-max-skill.nextlevelbuilder.io
- 定位：Web/移动多平台 UI/UX；支持 React、Next.js、Vue、Svelte、SwiftUI、RN、Flutter、Tailwind、shadcn/ui、HTML/CSS 等
- 典型动作：plan / design / build / review / improve UI

## 核心能力

| 能力 | 说明 |
|------|------|
| **Design System Generator（v2）** | 按产品需求并行检索品类/风格/色板/落地页 pattern/字体，输出 Pattern、Style、Colors、Typography、Effects、Anti-patterns、Pre-delivery checklist |
| **风格与资产库** | 50+ UI 风格、大量色板与字体配对、产品类型推理规则、图表类型、UX 准则 |
| **多技术栈** | 前端与移动主流栈均可引导生成 |
| **案例库** | 多行业真实站点案例（SaaS、教育、电商、金融、医疗、游戏等），可按 light/dark 筛选 |
| **交付纪律** | 含无 emoji 当图标、可点击元素 cursor-pointer 等检查项，抑制常见 AI 界面陋习 |

## 适用场景

- 落地页 / SaaS / Dashboard / 电商 / 作品集等需要「先定设计系统再写代码」
- 希望 Agent 按**行业品类**自动推荐风格，而不是空泛说「做漂亮一点」
- 与 Taste Skill 搭配：Pro Max 定系统，Taste 控反 slop 与旋钮气质

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/nextlevelbuilder/ui-ux-pro-max-skill |
| **官网 uupm.cc** | https://uupm.cc |
| **产品站** | https://ui-ux-pro-max-skill.nextlevelbuilder.io |
| **SKILL.md** | https://github.com/nextlevelbuilder/ui-ux-pro-max-skill/blob/main/.claude/skills/ui-ux-pro-max/SKILL.md |
| **清单出处** | [[50 来源资料/代码仓库/AI/UI设计风格 Skills 资源清单\|UI设计风格 Skills 资源清单]] |

## My takeaways

1. 找「**按业务类型自动配齐设计系统**」的开源 Skill 时，优先想到本仓库。
2. 体量与社区热度高，适合作为团队默认 UI Skill；注意与项目已有设计系统冲突时要显式约束。
3. 与 awesome-design-md（抄品牌）路线不同：本库是**推理生成**，不是粘贴某品牌 DESIGN.md。

## Related

- [[50 来源资料/代码仓库/AI/UI设计风格 Skills 资源清单|UI设计风格 Skills 资源清单]]
- [[50 来源资料/代码仓库/AI/taste-skill - 反AI土味前端设计 Skill|taste-skill - 反AI土味前端设计 Skill]]
- [[50 来源资料/代码仓库/AI/awesome-design-md - 大厂 DESIGN.md 设计语言库|awesome-design-md - 大厂 DESIGN.md 设计语言库]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
