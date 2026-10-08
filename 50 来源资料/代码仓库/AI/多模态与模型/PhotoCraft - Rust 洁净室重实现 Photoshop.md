---
id: source-20261008-photocraft
title: PhotoCraft - Rust 洁净室重实现 Photoshop
type: source
status: active
created: 2026-10-08
updated: 2026-10-08
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/图像编辑
  - 主题/Photoshop
  - 主题/Rust
  - 主题/MCP
  - 主题/Agent
  - 主题/开源资源
author: ArtCraft / storytold
source_type: repo
source_url: https://github.com/storytold/photocraft
source_author: storytold
source_date: 2026-10-08
summary: ArtCraft 开源的专业图像编辑器：用纯 Rust 对 Adobe Photoshop 做洁净室（clean-room）重实现，支持图层/蒙版/调整图层/图层样式/文字/矢量/PSD，并可经 CLI、JSON 控制通道与 MCP 被 AI Agent 驱动。Early Alpha；双许可 MIT / Apache-2.0。
related:
  - "[[50 来源资料/代码仓库/AI/多模态与模型/GenClaw - 代码驱动 Agent 图像生成|GenClaw]]"
  - "[[50 来源资料/代码仓库/AI/多模态与模型/OpenMontage - Agent 化视频制作系统|OpenMontage]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# PhotoCraft - Rust 洁净室重实现 Photoshop

## Source summary

**PhotoCraft**（仓库 [storytold/photocraft](https://github.com/storytold/photocraft)）是 ArtCraft 团队开源的专业图像编辑器：按公开规范与对 Photoshop 行为的观察，用 **纯 Rust** 做 **clean-room reimplementation**，而不是套壳或调用 Adobe 代码。目标是原生、跨平台、离线可用的 Photoshop 类工作流，并让同一套编辑引擎可被 GUI / CLI / Agent 共同驱动。

- 约 **18.1k stars / 2.4k forks**（2026-10-08 GitHub API）；主语言 **Rust**；状态 **early alpha**
- 许可证：**MIT OR Apache-2.0**（双许可任选）
- 官网：[getartcraft.com/apps/photocraft](https://getartcraft.com/apps/photocraft) · 落地页：[photocraft.one](https://photocraft.one/)
- 平台：macOS / Windows / Linux / FreeBSD 原生，引擎与 UI 也可编译为 **WebAssembly** 在浏览器运行
- 组织：[ArtCraft / storytold](https://github.com/storytold)（同系列还有 VectorCraft、FilmCraft、LightCraft 等 Crafting Apps）

## 来源线索

整理自头条文章《离谱！Photoshop修修补补几十年，一夜之间被人“偷家”了》（gid `7693904066098774568`，作者「科技先生」）：

- 亮点不只是「界面像 PS」，而是重做图层、蒙版、调整图层、图层样式、滤镜、文字、矢量、颜色管理、PSD 等背后工程
- PSD 引擎按 Adobe 公开规范编写，并用真实 PSD 语料与 Photoshop 合成结果做比对
- 500+ Command 统一命令体系：GUI、CLI、JSON 控制通道、MCP 调同一套引擎
- 明确声明 clean-room，无证据表明反编译 Adobe 源码；项目方称未使用 Adobe 专有代码/Shader/素材
- 仍是 Early Alpha，生成式 AI、部分工具、专业排版与插件生态等尚有差距，不宜当作日常生产替代品

原文：[今日头条文章](https://www.toutiao.com/article/7693904066098774568/)

## 解决什么问题

| 痛点 | PhotoCraft 方案 |
|------|----------------|
| 专业图层工作流被闭源垄断 | 开源原生编辑器，覆盖图层/蒙版/调整图层/样式等核心路径 |
| PSD 互通难 | 独立 PSD crate，打开/编辑/回写；宣称对 psd-tools 测试集 309 中 307 保持渲染一致 |
| Agent 只能「看屏幕点鼠标」猜操作 | 菜单/工具/对话框统一为 Command；CLI / JSON / MCP 可脚本化驱动 |
| 大画布与撤销成本高 | Copy-on-Write 256² 稀疏图块 + wgpu GPU 合成（Metal/Vulkan/DX12/WebGPU） |
| 桌面与 Web 两套引擎 | 同一引擎可编译到桌面与 Wasm |

## 核心能力

| 能力 | 说明 |
|------|------|
| **熟悉的交互** | 菜单、快捷键、面板布局贴近 Photoshop 习惯 |
| **非破坏编辑** | 16 种调整图层（Curves/Levels/Vibrance 等）+ 70+ 实时滤镜；智能对象与智能滤镜 |
| **图层样式** | 投影、内外发光、斜面浮雕、描边、叠加等，可在文字层上实时预览 |
| **选择与蒙版** | 选区工具 + Quick/Object/Select Subject；选区可转图层蒙版/矢量/形状；本地运行，无需云账号 |
| **真实 PSD** | 打开、编辑、保存分层 PSD/PSB；未建模数据尽量保留结构 |
| **Agent-ready** | 500+ 命令注册表；`photocraft-cli`、`--control` 控制通道、`photocraft-cli mcp` |
| **架构分层** | 文档模型与命令引擎和 UI 分离；约 24 个 crate；CPU 合成器作参考 oracle，与 GPU 合成互测 |
| **测试密度** | 仓库宣称 1700+ 测试；另有 [photocraft-corpus](https://github.com/storytold/photocraft-corpus) 真实文件语料 |

## Agent / 自动化用法（摘要）

```sh
# 无头：打开 → 命令编辑 → 导出
photocraft-cli run wave.psd \
  --cmd filter.sharpen.smartSharpen     --params '{"amount":80}' \
  --cmd layer.newAdjustmentLayer.curves --params '{"points":[[0,0],[64,48],[192,212],[255,255]]}' \
  --out wave-final.png

# 对文件夹批量套用动作列表
photocraft-cli batch --actions grade.json --in ./raw --out ./graded

# 让 Agent 经 MCP 驱动（无头或桥接到正在运行的应用）
photocraft-cli mcp
```

桌面端还可开仅本机回环的控制通道：`photocraft --control`（鉴权），用于检查 UI 状态、指针驱动工具、离屏截图。详见仓库 `docs/control-protocol.md`。

## 快速开始

需要 **Rust 1.90+**（以仓库 README 为准）：

```sh
git clone https://github.com/storytold/photocraft.git
cd photocraft
cargo run --release -p photocraft
```

官网提供预构建安装包与 Web 构建下载（版本随发布变化，见 Releases / 官网）。

## ArtCraft 同系列（Crafting Apps）

| 应用 | 方向 | 仓库 |
|------|------|------|
| **PhotoCraft** | 图像编辑 / PSD | https://github.com/storytold/photocraft |
| **VectorCraft** | 矢量插画 | https://github.com/storytold/vectorcraft |
| **FilmCraft** | 视频剪辑、调色与声音 | https://github.com/storytold/filmcraft |
| **LightCraft** | 图库与 RAW 显影（类 Lightroom） | https://github.com/storytold/lightcraft |
| **PdfCraft** | PDF 阅读整理 | https://github.com/storytold/pdfcraft |

约定共性：洁净室、纯 Rust、桌面原生 + Wasm、可被 Agent 驱动。

## 适用边界

**适合**：研究 PS 级图层/PSD 工程、需要开源离线编辑器、想让 Coding Agent 通过 MCP/CLI 做批量修图脚本。

**暂不适合**：把 Early Alpha 当作生产环境日常 PS 替代；强依赖生成式 AI/完整插件生态/专业排版工作流的场景。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/storytold/photocraft |
| **官网应用页** | https://getartcraft.com/apps/photocraft |
| **落地页** | https://photocraft.one/ |
| **ArtCraft** | https://getartcraft.com/ |
| **测试语料仓库** | https://github.com/storytold/photocraft-corpus |
| **控制协议文档** | https://github.com/storytold/photocraft/blob/main/docs/control-protocol.md |
| **开发文档** | https://github.com/storytold/photocraft/blob/main/docs/development.md |
| **Releases** | https://github.com/storytold/photocraft/releases |
| **Discord** | https://discord.gg/artcraft |
| **介绍文章（头条）** | https://www.toutiao.com/article/7693904066098774568/ |
| **许可证** | MIT OR Apache-2.0 |

## My takeaways

1. **价值不在「长得像 PS」**，而在洁净室重做合成/PSD/图层样式等硬核能力，并用测试语料对齐 Photoshop 行为。
2. **Agent 友好是一等公民**：500+ Command + CLI/MCP/控制通道，比「截屏点 UI」更适合自动化修图流水线。
3. **定位 Early Alpha**：功能覆盖广，但生产替代仍早；适合学习、试验与 Agent 集成，不宜立刻替换专业日常工作流。
4. **与 GenClaw / OpenMontage 互补**：GenClaw 偏代码驱动生成图像；OpenMontage 偏 Agent 视频制片；PhotoCraft 偏可脚本化的专业图层编辑引擎。
5. **同系列 Crafting Apps** 值得一并收藏（矢量/视频/RAW/PDF），技术路线一致。

## Related

- [[50 来源资料/代码仓库/AI/多模态与模型/GenClaw - 代码驱动 Agent 图像生成|GenClaw - 代码驱动 Agent 图像生成]]
- [[50 来源资料/代码仓库/AI/多模态与模型/OpenMontage - Agent 化视频制作系统|OpenMontage - Agent 化视频制作系统]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]