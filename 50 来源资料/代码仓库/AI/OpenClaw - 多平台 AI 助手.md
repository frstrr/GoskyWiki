---
id: source-20260831-openclaw
title: OpenClaw - 多平台 AI 助手
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI助手
  - 主题/Agent
  - 主题/IM集成
author: OpenClaw
source_type: repo
source_url: https://github.com/openclaw/openclaw
source_author: openclaw
source_date: 2026-08-31
summary: 跨 20+ 消息平台（飞书、Slack、Discord、Telegram、WhatsApp 等）的个人 AI 助手框架，支持 ClawHub skill 生态，OpenMAIC 官方集成，一行命令从聊天 App 生成互动课堂。
related:
  - "[[50 来源资料/代码仓库/AI/OpenMAIC - 开源多智能体交互课堂|OpenMAIC - 开源多智能体交互课堂]]"
---

# OpenClaw - 多平台 AI 助手

## Source summary

**OpenClaw** 是一个跨消息平台的个人 AI 助手框架，让用户从飞书、Slack、Discord、Telegram、WhatsApp 等 **20+ 聊天 App** 直接与 AI 助手交互，无需打开命令行。

- 开源项目，GitHub：`openclaw/openclaw`
- 拥有 **ClawHub** skill 分发生态（类似 npm 包管理，一行安装 skill）
- [[50 来源资料/代码仓库/AI/OpenMAIC - 开源多智能体交互课堂|OpenMAIC]] 官方集成，可从聊天 App 直接生成 AI 互动课堂
- **WorkBuddy** 原生兼容 OpenClaw skill 生态（腾讯 AI 编程助手）

## 核心能力

| 能力 | 说明 |
|------|------|
| **多平台接入** | 飞书、Slack、Discord、Telegram、WhatsApp 等 20+ IM |
| **Skill 生态** | ClawHub 一行安装 skill，如 `clawhub install openmaic` |
| **SOP 引导** | Skill 以 SKILL.md + references/ 结构，Agent 按步骤引导用户完成复杂任务 |
| **WorkBuddy 兼容** | WorkBuddy 支持 OpenClaw 规范 skill，可从 SkillHub / Git 仓库 / ZIP / 拖拽导入 |

## 与 OpenMAIC 的集成

OpenMAIC 在仓库 `skills/openmaic/` 提供了引导式 SOP skill：

```bash
clawhub install openmaic
# 或手动复制
mkdir -p ~/.openclaw/skills
cp -R /path/to/OpenMAIC/skills/openmaic ~/.openclaw/skills/openmaic
```

两种模式：

| 模式 | 说明 |
|------|------|
| **Hosted 托管** | 在 open.maic.chat 获取 access code，无需本地部署 |
| **Self-hosted 本地** | Skill 逐步引导 clone → 配置 API Key → 选择 pnpm dev / Docker 启动 |

配置示例（`~/.openclaw/openclaw.json`）：

```json
{
  "skills": {
    "entries": {
      "openmaic": {
        "config": {
          "accessCode": "sk-xxx",
          "repoDir": "/path/to/OpenMAIC",
          "url": "http://localhost:3000"
        }
      }
    }
  }
}
```

## WorkBuddy 中使用 OpenClaw Skill

WorkBuddy 支持四种安装方式：

1. **SkillHub 在线安装**：技能市场 → 搜索 → 筛选「OpenClaw 社区」
2. **Git 仓库直拉**：Claw 设置 → 技能管理 → 从 Git 仓库导入
3. **ZIP 包导入**：团队共享或版本归档
4. **SKILL.md 拖拽**：最轻量，直接拖入聊天框

Skill 存放路径：`~/.workbuddy/skills/<技能名>/`

## 适用场景

- 希望在飞书/Slack 等 IM 中远程触发 AI 任务（如「教我量子物理」生成课堂）
- 需要 skill 生态扩展 AI 助手能力，而非每次从头推理
- 使用 WorkBuddy 且想复用 OpenClaw 社区 skill

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/openclaw/openclaw |
| **OpenMAIC Skill** | OpenMAIC 仓库 `skills/openmaic/` |
| **ClawHub 安装** | `clawhub install openmaic` |

## My takeaways

1. **IM 入口降低使用门槛**：复杂工具（如 OpenMAIC 本地部署）通过 skill SOP 引导，用户只需在聊天中说「教我 XX」。
2. **Skill 生态互通**：OpenClaw skill 可被 WorkBuddy 直接加载，避免重复造轮子。
3. **Hosted vs Self-hosted 双模式**：access code 适合快速体验，本地部署适合数据敏感场景。

## Related

- [[50 来源资料/代码仓库/AI/OpenMAIC - 开源多智能体交互课堂|OpenMAIC - 开源多智能体交互课堂]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
