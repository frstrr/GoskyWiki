---
id: source-20261105-craft-anything
title: Craft Anything - AI 研究重建检查引擎
type: source
status: active
created: 2026-11-05
updated: 2026-11-05
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/MCP
  - 主题/CLI
  - 主题/Rust
  - 主题/逆向工程
  - 主题/净室
  - 主题/开源资源
  - 主题/AI编程Agent
author: ArtCraft / storytold
source_type: repo
source_url: https://github.com/storytold/craft-anything
source_author: storytold
source_date: 2026-11-05
summary: ArtCraft 开源的纯 Rust「研究→构建→检查→收敛」引擎：指向网站/应用/游戏，用净室或深度模式采集证据，再让编码 Agent 重建并对照原件返修。提供桌面应用、CLI 与 MCP；Early Alpha；双许可 MIT / Apache-2.0。
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA]]"
  - "[[50 来源资料/代码仓库/AI/多模态与模型/PhotoCraft - Rust 洁净室重实现 Photoshop|PhotoCraft]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# Craft Anything - AI 研究重建检查引擎

## Source summary

**Craft Anything**（仓库 [storytold/craft-anything](https://github.com/storytold/craft-anything)）是 ArtCraft 开源的纯 Rust 引擎与产品：把目标指给它，说清你想要什么，它用真实检查工具研究原作，让编码 Agent 建新项目，再与原件比对并反复收敛。

- 约 **12.9k stars**（2026-11-05 页面抓取；头条文章称创建后约 4 天已超 8.4k）；主语言 **Rust**；状态 **early alpha**；最新 Release **v0.3.0**
- 许可证：**MIT OR Apache-2.0**
- 官网 / 系列：[getartcraft.com](https://getartcraft.com/) · Discord：[discord.gg/artcraft](https://discord.gg/artcraft)
- 入口：独立桌面应用（简单 / 进阶）、`craft-anything-cli`、MCP 服务；部分能力可 Wasm 跑在浏览器查看器
- 组织：[ArtCraft / storytold](https://github.com/storytold)（同系列还有 PhotoCraft、VectorCraft、FilmCraft 等）

## 来源线索

整理自头条文章《4天8460星，复刻你想要的任何软件！Rea的强劲对手Craft Anything》（gid `7704105615135703552`，作者「老年人学AI」）：

- 核心卖点不是「一键抄软件」，而是可操作流程：**参考对象 → 分析证据 → 新项目 → 差异检查 → 返修**
- 与 [[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA]] 的对比宜落在产品形态：Craft Anything 把研究/构建/检查收进同一项目界面；REA 更偏把分析能力嵌入已有 Agent 工作流
- 网站检查强调多屏宽（如 1440 / 768 / 390）与布局/资源差异；相似度达标不等于业务功能完成
- 默认 **净室（Clean Room）**：不读原件内部代码/源码；**深度（Deep）** 才开放内部分析与可选 Ghidra
- 构建侧当前优先 **Claude Code**；Codex / Hermes 等有适配但验证程度不一
- 合规提醒：只分析自己拥有或获准检查的目标；不绕过登录 / DRM / 反作弊；商标与素材需另确认授权

原文：[今日头条文章](https://www.toutiao.com/article/7704105615135703552/)

## 解决什么问题

| 痛点 | Craft Anything 方案 |
|------|---------------------|
| AI「写完了」但结果对不上原作观感 | 把检查写入流程：对照布局、资源、溢出、菜单等，生成可返修的差异清单 |
| 只会截图提示、缺少可复查证据 | 研究阶段留下证据台账；深度模式可看函数/字符串/伪代码/反汇编 |
| 想复用参考站/小工具/小游戏的交互，但不会逆向 | 净室按可见行为与测量结果重建；深度按需读内部结构 |
| 工具散落在 CLI / 逆向器 / 编码助手之间 | 同一引擎同时服务桌面、CLI、MCP（约 300+ 工具） |
| 希望独立界面推进，而不是只在聊天里推进 | 简单模式四步进度 + 审批卡；进阶模式完整工作台 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **四步流程** | 研究 → 构建 → 检查 → 就绪；可继续提要求收敛 |
| **两种界面** | 简单模式面向目标与进度；进阶模式展开工具/证据/伪 C/反汇编等 |
| **净室 / 深度** | 默认净室只依据可见与允许观察的行为；深度开放内部读取与可选 Ghidra |
| **多目标类型** | 网站、桌面/手机应用、游戏与文件结构；游戏侧可面向 Unity / Unreal / Godot 等重建路径 |
| **网站差异检查** | 多屏宽比对布局与图像结构，记录文字/颜色/字体/间距/横向溢出、菜单与加载失败等 |
| **Agent-ready** | 每个工具同时是 CLI 子命令与 MCP 工具；`setup` / `mcp` / `rebuild` / `doctor` |
| **纯 Rust 引擎** | 自带解码/反汇编/反编译路径；装 Ghidra 时可增强深度分析 |
| **跨平台发布** | Windows / macOS / Linux / FreeBSD 安装包与便携版；另有浏览器 Wasm 查看器 |

## 与 REA 怎么选（摘要）

| 维度 | Craft Anything | REA |
|------|----------------|-----|
| 主要入口 | 独立桌面应用 + CLI/MCP | 以 MCP/CLI 嵌入已有助手为主 |
| 工作组织 | 内置研究、构建、检查、返修项目流 | Agent 依证据继续调查、实现与测试 |
| 分析侧重点 | 内置 Rust 分析引擎，可选 Ghidra | 内置能力 + Hopper/Ghidra/IDA 等外部引擎 |
| 产品侧重点 | 同一项目里推进重建与检查 | 把软件调查能力接到现有编码 Agent |

二者不必二选一：已习惯在助手里做调查可继续 REA；想要独立界面与连续检查流可试用 Craft Anything。以同一小目标实测证据质量、能否跑通、返修是否改善为准。

## Agent / CLI 用法（摘要）

```sh
# 环境自检
craft-anything-cli doctor
craft-anything-cli commands

# 接入编码 Agent / 启动 MCP
craft-anything-cli setup
craft-anything-cli mcp

# 重建示例（需本机已登录可用的编码 Agent）
craft-anything-cli rebuild https://example.com "Recreate this website"
craft-anything-cli rebuild - "A snake game in Rust" --rounds 1
```

项目目录通常含 `craft.json`、`capture/`、`app/`、`checks/`、`investigation.cany/`、`inbox/` 等（以本机版本 README 为准）。源码构建需 **Rust 1.97+**。

## 快速开始

普通用户优先用 [Releases](https://github.com/storytold/craft-anything/releases) 安装包（文章核查时最新正式版为 **v0.3.0**），不必先编译。

源码路线：

```sh
git clone https://github.com/storytold/craft-anything && cd craft-anything
cargo run --release -p craft-anything
cargo install --path apps/craft-anything-cli
```

首次建议：小目标（单页作品集、贪吃蛇级小游戏）、检查 Auto mode / 审批规则、用临时目录，并单独估算模型与素材生成费用（开源不等于免费额度）。

## ArtCraft 同系列（Crafting Apps）

| 应用 | 方向 | 仓库 |
|------|------|------|
| **Craft Anything** | 研究 / 重建 / 检查任意目标 | https://github.com/storytold/craft-anything |
| **PhotoCraft** | 图像编辑 / PSD | https://github.com/storytold/photocraft |
| **VectorCraft** | 矢量插画 | https://github.com/storytold/vectorcraft |
| **FilmCraft** | 视频剪辑、调色与声音 | https://github.com/storytold/filmcraft |
| **LightCraft** | 图库与 RAW 显影 | https://github.com/storytold/lightcraft |
| **PdfCraft** | PDF 阅读整理 | https://github.com/storytold/pdfcraft |

## 适用边界

**适合**：小范围试用网站/独立小工具重建；需要「生成后必须留下检查结果」的工作流；想经 CLI/MCP 把分析与重建接到 Agent。

**暂不适合**：把 Early Alpha 当生产级「复刻任意大型商业软件」方案；重要生产目录直接敞开自动模式；未获授权的目标分析；把外观相似度误当成功能/安全/权限验收完成。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/storytold/craft-anything |
| **Releases** | https://github.com/storytold/craft-anything/releases |
| **中文 README** | https://github.com/storytold/craft-anything/blob/main/README.zh.md |
| **状态说明** | https://github.com/storytold/craft-anything/blob/main/docs/STATUS.md |
| **工具清单** | https://github.com/storytold/craft-anything/blob/main/docs/tools.md |
| **ArtCraft** | https://getartcraft.com/ |
| **Discord** | https://discord.gg/artcraft |
| **介绍文章（头条）** | https://www.toutiao.com/article/7704105615135703552/ |
| **对照项目 REA** | https://github.com/morluto/rea · https://rea.tools/ |
| **许可证** | MIT OR Apache-2.0 |

## My takeaways

1. **真正可借鉴的是闭环**：研究证据 → 生成 → 差异检查 → 返修，而不是口号式的「复刻任何软件」。
2. **与 REA 是产品形态差异**，不是「一个只会研究、一个才会开发」；选型看入口习惯与检查流是否需要独立项目界面。
3. **净室默认有价值**：先按可见行为重建，降低误读内部实现与合规风险；深度模式再按需打开。
4. **早期 Alpha + 模型成本**：先小目标、限轮数、看一次完整任务消耗，再扩大范围。
5. **同属 ArtCraft 洁净室 Rust 路线**：可与 [[50 来源资料/代码仓库/AI/多模态与模型/PhotoCraft - Rust 洁净室重实现 Photoshop|PhotoCraft]] 等 Crafting Apps 一并收藏。

## Related

- [[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA - Agent 逆向分析 MCP]]
- [[50 来源资料/代码仓库/AI/多模态与模型/PhotoCraft - Rust 洁净室重实现 Photoshop|PhotoCraft - Rust 洁净室重实现 Photoshop]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]