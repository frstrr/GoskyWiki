---
id: source-20261012-dsh-blender
title: dsh-blender - DSH Blender 3D 生产插件
type: source
status: active
created: 2026-10-12
updated: 2026-10-12
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/Blender
  - 主题/3D建模
  - 主题/Skills
  - 主题/DeepSeek Harness
  - 主题/开源资源
author: CheshireJCat
source_type: repo
source_url: https://github.com/CheshireJCat/blender
source_author: CheshireJCat
source_date: 2026-08-14
summary: DeepSeek Harness（DSH）插件 dsh-blender：无需打开 Blender GUI 或安装 BlenderMCP，由插件自行拉起后台 Blender 进程，覆盖粗模→渲染验收→细化→校验→导出全流程；提供 30 个 Skill、13 个工具与 26 个分析 Helper。
related:
  - [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]
  - [[50 来源资料/代码仓库/AI/UI设计与美学/img2threejs - 图片转程序化 Three.js 模型|img2threejs]]
  - [[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill]]
  - [[50 来源资料/代码仓库/AI/Agent运行时与编排/TeamAI CLI - 团队 AI Harness 统一管理|TeamAI CLI]]
  - [[40 知识导航/游戏开发|游戏开发]]
  - [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
  - [[40 知识导航/AI 工具使用|AI 工具使用]]
aliases:
  - CheshireJCat/blender
  - dsh-blender
  - blender DSH 插件
---

# dsh-blender - DSH Blender 3D 生产插件

## Source summary

**dsh-blender**（仓库 [CheshireJCat/blender](https://github.com/CheshireJCat/blender)）是面向 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 的 **Blender 3D 生产插件**（npm 包名 `dsh-blender`）。给 DSH 装上后，Agent 可在**不打开 Blender GUI、不依赖 BlenderMCP 控制端口**的情况下，由插件每次自行启动后台 `blender -b` 进程，完成建模、重建、渲染验收、结构校验与多格式导出。

- 约 **41 stars / 1 fork**（截至 2026-10-12 GitHub API）；许可证 **MIT**；主语言标注 **Python**
- 当前版本约 **v0.2.2**（GitHub Release / npm `0.2.2`，2026-09-16）
- Topics：`3d-modeling` / `agent-skills` / `blender` / `deepseek-harness` / `dsh-plugin`
- npm：[https://www.npmjs.com/package/dsh-blender](https://www.npmjs.com/package/dsh-blender)
- 插件导航页：[https://plugin.dshx.dev/item/CheshireJCat/blender/](https://plugin.dshx.dev/item/CheshireJCat/blender/)
- 中文 README：[README.zh-CN.md](https://github.com/CheshireJCat/blender/blob/main/README.zh-CN.md)
- 来源线索：今日头条文章《不用开 Blender、不用点连接：AI 在后台把 3D 模型建好了》（[文章链接](https://www.toutiao.com/article/7693868601983468070/)）

核心一句话：**DSH + 后台 Blender 进程 = 可编排、可验收的本地 3D 生产管线。**

## 核心特性

| 特性 | 说明 |
|------|------|
| **后台 Blender，无需 GUI** | 每次操作独立启动 `blender -b`；与手动打开的 Blender 是两个世界，不必点「连接」 |
| **不依赖 BlenderMCP** | 不要求安装 Blender MCP addon，也不开放控制端口 |
| **30 个 Skill** | `create-3d-model` 总编排 + 29 个领域 Skill（如 `blender-modeling`、`reference-to-3d`、`wireframe-to-3d`、`blender-animation`） |
| **13 个运行时工具** | 状态检查、场景/对象只读信息、导入、bpy 执行、预览/渲染、导出、场景与导出校验、Helper 目录与执行 |
| **26 个确定性 Helper** | 参考图、线框、轮廓、多视图、UV、贴图、外观、修复与动画 QA；依赖插件目录私有 `.venv`（OpenCV / NumPy / Pillow / SciPy） |
| **典型流水线** | 粗模 → 渲染验收 → 细化 → 结构校验 → 导出 → 干净进程重新导入验证 |
| **多格式导出** | GLB/glTF、FBX、OBJ、STL、USD、PLY 等；导入还支持 DAE |
| **Workspace 安全边界** | 默认 `restrictToWorkspace`；禁内嵌脚本自动执行；默认不覆盖已有文件；可关闭高风险 `blender_python` |

## 与 BlenderMCP 的区别（速记）

| 维度 | dsh-blender | BlenderMCP（常见用法） |
|------|-------------|------------------------|
| 运行形态 | 插件自己起后台 Blender 进程 | 通常要你开着 Blender + addon，经 MCP 端口控制 |
| 宿主 | DeepSeek Harness（DSH）插件生态 | MCP 客户端（Cursor / Cline 等） |
| 验收闭环 | 内置渲染预览、场景/导出校验与 Helper QA | 取决于你如何编排 MCP 工具 |
| 适用感 | 「不用开 Blender、不用点连接」的后台生产 | 「看着 AI 在已打开的 Blender 里干活」 |

文章建议：若走 dsh-blender 路线，BlenderMCP 的 addon 不必为了本流程强留。

## 环境要求

- DeepSeek Harness `0.1.0-rc.6`
- Node.js 20+
- Blender 4.3+（实现已面向 Blender 5.x API 适配）；`blender` 在 `PATH`，或在 `cordis.patch.yml` 配绝对路径
- 参考图/线框等 Helper：Python 3.10+（`dsh-blender-setup` / `pnpm setup:analysis` 会建 `.venv`）

## 快速开始

### 从 npm 安装

```bash
npx @deepseek-ai/dsh plugin --profile web add dsh-blender
npx @deepseek-ai/dsh plugin --profile web exec dsh-blender-setup
npx @deepseek-ai/dsh --profile web --dump-config
npx @deepseek-ai/dsh web
```

npm 上有同名「破壁机」类插件时，文章建议用 GitHub 固定版以免装错：

```bash
npx @deepseek-ai/dsh plugin --profile web add github:CheshireJCat/blender#v0.2.2
```

### 示例提示词

打开 http://127.0.0.1:3080，会话 workspace 指向建模目录后：

```text
使用 create-3d-model 创建一个低多边形台灯。
先调用 blender_status，粗模和最终版本分别保存并渲染验收，
最后导出 GLB，同时保留版本化 .blend 源文件。
```

### Headless 冒烟

```bash
npx @deepseek-ai/dsh plugin --profile headless add .
npx @deepseek-ai/dsh --profile headless \
  使用