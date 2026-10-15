---
id: source-20261015-modly
title: Modly - 本地 AI 图像转 3D 桌面应用
type: source
status: active
created: 2026-10-15
updated: 2026-10-15
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/3D生成
  - 主题/图像转模型
  - 主题/本地推理
  - 主题/开源资源
aliases:
  - Modly
  - modly3d
author: Lightning Pixel (lightningpixel)
source_type: repo
source_url: https://github.com/lightningpixel/modly
source_author: lightningpixel
source_date: 2026-10-15
summary: 本地优先的开源桌面应用（MIT），用本机 GPU 上的开源模型把图片或提示词生成三维网格；支持扩展式模型生态（Hunyuan3D / TripoSG / Trellis2）、可视化工作流与面向 Agent 的 CLI。约 8.2k Stars。
related:
  - "[[50 来源资料/代码仓库/AI/多模态与模型/GenClaw - 代码驱动 Agent 图像生成|GenClaw]]"
  - "[[50 来源资料/代码仓库/AI/UI设计与美学/img2threejs - 图片转程序化 Three.js 模型|img2threejs]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/dsh-blender - DSH Blender 3D 生产插件|dsh-blender]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# Modly - 本地 AI 图像转 3D 桌面应用

## Source summary

**Modly**（[lightningpixel/modly](https://github.com/lightningpixel/modly)）是一款**本地、开源、AI 驱动**的桌面应用：把单张图片或提示词生成三维网格，推理完全在本机 GPU 上完成，数据不上云、无需账号与按次付费。

- **仓库**：https://github.com/lightningpixel/modly
- **官网文档**：https://modly3d.app/docs
- **规模**：约 **8.2k Stars / 763 Forks**（截至 2026-10-15，shields.io）
- **技术栈**：前端 Node.js / TypeScript + Electron；后端 Python（FastAPI）；许可证 **MIT**
- **平台**：Windows、Linux、Apple Silicon macOS（Intel Mac 不在官方支持范围）
- **触发收录**：头条介绍文 [Modly：用本地 AI 把图片变成 3D 模型的桌面应用](https://www.toutiao.com/article/7695969190452134419/)

## 核心能力

| 能力 | 说明 |
|------|------|
| **本地图像转三维** | 单张图片经工作流节点链生成网格（Image → Generate Mesh → Add to Scene） |
| **扩展式模型生态** | 从 GitHub 安装官方扩展：Hunyuan3D 2 Mini / Turbo / Fast、TripoSG、Trellis2 GGUF 等 |
| **可视化工作流** | Workflows / Generate 标签页串联节点；运行前校验连线，非法图原地报错 |
| **面向 Agent 的 CLI** | `tools/modly-cli/agent.py`（仅标准库）覆盖 health、model、workflow-run、generate 等规范化命令 |
| **网格后处理与场景** | 应用内平滑 / 抽稀；支持场景清单与场景节点串联 |
| **AMD ROCm** | Linux / Windows 上自动识别 Radeon，导向对应 PyTorch 轮子 |

## 官方扩展（示例）

| 扩展 | 模型 |
|------|------|
| [modly-hunyuan3d-mini-extension](https://github.com/lightningpixel/modly-hunyuan3d-mini-extension) | Hunyuan3D 2 Mini |
| [modly-hunyuan3d-mini-turbo-extension](https://github.com/lightningpixel/modly-hunyuan3d-mini-turbo-extension) | Hunyuan3D 2 Mini Turbo |
| [modly-hunyuan3d-mini-fast-extension](https://github.com/lightningpixel/modly-hunyuan3d-mini-fast-extension) | Hunyuan3D 2 Mini Fast |
| [modly-triposg-extension](https://github.com/lightningpixel/modly-triposg-extension) | TripoSG |
| [modly-trellis2-gguf-extension](https://github.com/lightningpixel/modly-trellis2-gguf-extension) | Trellis2 GGUF |

安装入口：应用内 **Models → Install from GitHub**，填扩展仓库 HTTPS 地址；再按需下载权重变体（可看 `size_gb` / `vram_gb`）。

## 快速上手

```bash
# 克隆后一键启动（免完整开发环境）
# Windows
launch.bat
# Linux / macOS
./launch.sh

# 或开发模式
npm install
cd api && python -m venv .venv && pip install -r requirements.txt
npm run dev
```

装好至少一个模型扩展后，再走基础工作流生成；跳过装模型通常只会得到明确的「模型未安装」提示。

CLI 示例（驱动已运行的桌面应用）：

```bash
python tools/modly-cli/agent.py generate --image ./input.png --output ./export.glb
```

## 适用场景

- 游戏 / 三维内容创作者：概念图快速变成可摆弄的网格草稿
- 研究与隐私敏感场景：素材不出本机，反复试验不同模型
- 团队内配管线：用 CLI / 工作流接入自动化，避免第三方云端上传

## 边界与代价

- 质量与速度受本机显卡与已装模型限制；量化（GGUF）变体可降低门槛
- macOS 仅 Apple Silicon；fork 衍生应用需按 MIT 保留对原项目与作者的署名
- 与云端服务比：前期有环境 / 显卡成本，换来按次零边际成本与数据私有

## 相关链接

| 链接 | 说明 |
|------|------|
| https://github.com/lightningpixel/modly | 主仓库 |
| https://github.com/lightningpixel/modly/releases | 安装包下载 |
| https://modly3d.app/docs | 官方文档 |
| https://discord.gg/BvjDCvS3yr | 社区 Discord |
| https://www.toutiao.com/article/7695969190452134419/ | 中文介绍来源 |