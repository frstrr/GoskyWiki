---
id: source-20260831-cli-anything
title: CLI-Anything - 软件 Agent 原生化 CLI 框架
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/Agent框架
  - 主题/CLI
  - 主题/AI编程Agent
author: HKUDS（Yuhao Yang 等）
source_type: repo
source_url: https://github.com/HKUDS/CLI-Anything
source_author: HKUDS
source_date: 2026-08-31
summary: 让任意软件可被 AI Agent 通过 CLI 操控——含 CLI-Hub 注册表/包管理器（消费端）与 7 阶段 Harness 生成流水线（生产端），已覆盖 GIMP、Blender、LibreOffice、Godot 等数十款专业软件。
related:
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[50 来源资料/代码仓库/AI/JIT-Agent - 即时生成 Agent Harness|JIT-Agent - 即时生成 Agent Harness]]"
  - "[[50 来源资料/代码仓库/AI/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]"
---

# CLI-Anything - 软件 Agent 原生化 CLI 框架

## Source summary

**CLI-Anything**（"Making ALL Software Agent-Native"）是 HKUDS 团队发起的开源项目，目标是把**任意软件、代码库或 Web 服务**变成 AI Agent 可直接调用的**结构化 CLI 工具**，无需 GUI 自动化或重写应用。

- 开源项目，GitHub：[HKUDS/CLI-Anything](https://github.com/HKUDS/CLI-Anything)
- 约 **48,000+ stars**（2026-08-31）
- 语言：**Python 3.10+**；Harness 基于 Click CLI
- 官网 / CLI-Hub：[clianything.cc](https://clianything.cc)
- PyPI 包：[cli-anything-hub](https://pypi.org/project/cli-anything-hub/)
- 中文文档：[README_CN.md](https://github.com/HKUDS/CLI-Anything/blob/main/README_CN.md)

项目分两条主线：

| 主线 | 用途 | 入口 |
|------|------|------|
| **消费端（CLI-Hub）** | 浏览、搜索、安装、启动社区已做好的 CLI Harness | `pip install cli-anything-hub` → `cli-hub` |
| **生产端（Generator）** | 为尚无 Harness 的软件/代码库自动生成 CLI | 各 Agent 平台的 `/cli-anything` 插件/skill |

## 解决什么问题

AI Agent 擅长推理，但难以稳定操控**真实专业软件**。常见方案各有缺陷：

| 痛点 | CLI-Anything 方案 |
|------|-------------------|
| GUI 自动化（截图/点击）脆弱、易碎 | 直接对接软件真实后端 API（bpy、ODF、Script-Fu 等），无 RPA 依赖 |
| 官方 API 零散、需大量封装 | 7 阶段流水线自动生成统一 Click CLI + JSON 输出 + REPL |
| 简化版重写丢失 90% 功能 | 保留完整专业能力（Blender 渲染、LibreOffice 转 PDF 等） |
| Agent 不知如何发现/安装工具 | CLI-Hub 注册表 + `SKILL.md` 自动生成，Agent 可自主发现安装 |
| 各 Agent 平台集成方式不同 | 提供 Claude Code / Cursor / Pi / OpenClaw / Codex / Hermes 等多平台插件 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **CLI-Hub 包管理器** | `cli-hub list/search/info/install/update/uninstall/launch`，统一管理 Harness |
| **7 阶段 Harness 生成** | Analyze → Design → Implement → Plan Tests → Write Tests → Document → Publish |
| **SKILL.md 自动生成** | Phase 6.5 为每个 CLI 生成 Agent 可发现的 skill 定义 |
| **JSON + 人类可读双输出** | Agent 消费结构化 JSON；调试时可用可读格式 |
| **REPL 交互模式** | ReplSkin 统一交互体验；支持 undo/redo 与会话状态 |
| **Preview / Live Preview** | 部分 Harness 支持预览轨迹（如 Blender、FreeCAD 演示） |
| **Refine 迭代扩展** | `/cli-anything:refine` 分析能力缺口并增量补全命令 |
| **生产级测试** | 2280+ 测试；单元 + E2E + 真实软件验证 |
| **多平台 Agent 集成** | Generator 插件 + Consumer meta-skill 双轨支持 |

## 7 阶段流水线

```
/cli-anything <software-path-or-repo>
  1. Analyze   — 扫描源码，映射 GUI 操作到 API
  2. Design    — 设计命令组、状态模型、输出格式
  3. Implement — 构建 Click CLI（REPL、JSON、undo/redo）
  4. Plan Tests— 生成 TEST.md 测试计划
  5. Write Tests— 实现单元 + E2E 测试
  6. Document  — 更新文档与测试结果
  7. Publish   — 生成 setup.py，安装到 PATH
  (+ 6.5) SKILL.md 自动生成
```

## 已支持的软件领域（部分）

| 领域 | 代表 Harness | CLI 命令示例 |
|------|-------------|-------------|
| 图像/3D | GIMP, Blender, Inkscape, Krita, FreeCAD | `cli-anything-gimp`, `cli-anything-blender` |
| 音视频 | Audacity, OBS Studio, Kdenlive, Shotcut, MuseScore | `cli-anything-obs-studio` |
| 办公/知识 | LibreOffice, Obsidian, Joplin, Zotero, Calibre | `cli-anything-libreoffice` |
| 游戏 | Godot, Slay the Spire II | `cli-anything-godot` |
| 开发/DevOps | WireMock, n8n, JumpServer, iTerm2 | `cli-anything-wiremock` |
| GIS/科学 | QGIS, ImageJ, ParaView | `cli-anything-qgis` |
| AI/搜索 | Exa, Ollama, Stable Diffusion WebUI | 社区持续扩展 |
| 通信 | Zoom, Mailchimp | `cli-anything-zoom` |

完整列表见 [CLI-Hub 注册表](https://clianything.cc/registry.json) 或 `cli-hub list`。

## 快速开始

### 消费端：安装并使用现有 CLI

```bash
pip install cli-anything-hub

cli-hub list
cli-hub search blender
cli-hub install blender
cli-hub info blender
cli-hub launch blender --help
```

给 Agent 安装 meta-skill，使其能自主发现 CLI：

```bash
npx skills add HKUDS/CLI-Anything --skill cli-hub-meta-skill -g -y
```

然后提示 Agent：

```text
Find appropriate CLI software in CLI-Hub and complete the task: ...
```

### 生产端：为 Cursor 生成新 Harness

```powershell
git clone https://github.com/HKUDS/CLI-Anything.git
.\CLI-Anything\cursor-plugin\scripts\install.ps1
# 重载 Cursor 窗口后：
# /cli-anything ./gimp
# /cli-anything-refine ./shotcut "picture-in-picture workflows"
```

### 使用生成的 CLI

```bash
cd gimp/agent-harness && pip install -e .
cli-anything-gimp --help
cli-anything-gimp project new --width 1920 --height 1080 -o poster.json
cli-anything-gimp --json layer add -n "Background" --type solid --color "#1a1a2e"
cli-anything-gimp   # 进入 REPL
```

## 支持的 Agent 平台

| 平台 | 安装方式 | 命令 |
|------|---------|------|
| **Cursor** | `cursor-plugin/scripts/install.ps1` | `/cli-anything`, `/cli-anything-refine` 等 |
| **Claude Code** | `/plugin marketplace add HKUDS/CLI-Anything` | `/cli-anything ./gimp` |
| **Pi** | `.pi-extension/cli-anything/install.sh` | `/cli-anything` |
| **OpenClaw** | 复制 `openclaw-skill/SKILL.md` | `@cli-anything build a CLI for ...` |
| **Codex** | `codex-skill/scripts/install.ps1` | 自然语言描述任务 |
| **Hermes / Reasonix** | 各 skill 安装脚本 | 自然语言描述任务 |
| **GitHub Copilot CLI** | `copilot plugin install ./cli-anything-plugin` | `/cli-anything ./gimp` |
| **Consumer（通用）** | `npx skills add ... cli-hub-meta-skill` | Agent 自主搜索安装 CLI |

## 适用场景

- **让 Agent 操控专业软件**：Blender 建模、GIMP 修图、LibreOffice 文档、OBS 推流等
- **统一零散 Web API**：把多个 REST 端点包装成一个 stateful CLI，省 token、易编排
- **替代 GUI Agent**：用 CLI + Preview 轨迹做可复现、可评估的自动化任务
- **为开源项目快速加 Agent 接口**：对任意 GitHub 仓库跑 `/cli-anything`
- **构建 CLI 工具生态**：社区贡献 Harness，CLI-Hub 即时收录

**不太适合**：软件完全无 API/脚本接口且找不到开源替代；或只需一次性简单脚本、不需要 Agent 可发现性的场景。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/HKUDS/CLI-Anything |
| **CLI-Hub 官网** | https://clianything.cc |
| **PyPI 包** | https://pypi.org/project/cli-anything-hub/ |
| **注册表 JSON** | https://clianything.cc/registry.json |
| **Agent 可读资源** | https://clianything.cc/llms.txt |
| **中文 README** | https://github.com/HKUDS/CLI-Anything/blob/main/README_CN.md |
| **贡献指南** | https://github.com/HKUDS/CLI-Anything/blob/main/CONTRIBUTING.md |
| **Cursor 插件文档** | https://github.com/HKUDS/CLI-Anything/tree/main/cursor-plugin |
| **CLI-Hub meta-skill** | `npx skills add HKUDS/CLI-Anything --skill cli-hub-meta-skill -g -y` |
| **ClawHub** | https://clawhub.ai/yuh-yang/cli-anything-hub |
| **SkillHub** | https://www.skillhub.club/web/skills/itsyuhao-cli-anything-hub |

## My takeaways

1. **Agent-Software Gap 的系统性解法**：不是让 Agent 点 GUI，而是把软件后端能力结构化暴露为 CLI——与 [[50 来源资料/代码仓库/AI/JIT-Agent - 即时生成 Agent Harness|JIT-Agent]] 的「即时生成 harness」思路互补，CLI-Anything 更偏「标准化 7 阶段工程流水线 + 社区注册表」。
2. **双轨架构实用**：消费端（CLI-Hub）立即可用；生产端（Generator）按需扩展——Cursor 用户两条线都能走。
3. **SKILL.md 生态**：每个 Harness 自带 skill 定义，配合 `npx skills` 和 meta-skill，Agent 可自主发现→安装→使用，降低人工配置成本。
4. **真实软件验证**：2280+ 测试覆盖 GIMP/Blender/LibreOffice 等 18+ 复杂应用，不是玩具 demo。
5. **与当前工作区关联**：repair 分支 `agent-native-cli-research` 名称暗示可能在调研 Agent 原生化 CLI 方案——CLI-Anything 是成熟参考，尤其 Cursor 插件安装路径与 `/cli-anything` 命令可直接试用。

## Related

- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[50 来源资料/代码仓库/AI/JIT-Agent - 即时生成 Agent Harness|JIT-Agent - 即时生成 Agent Harness]]
- [[50 来源资料/代码仓库/AI/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]
- [[50 来源资料/代码仓库/AI/Agent Substrate - AI Agent 大规模运行时|Agent Substrate - AI Agent 大规模运行时]]
- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
