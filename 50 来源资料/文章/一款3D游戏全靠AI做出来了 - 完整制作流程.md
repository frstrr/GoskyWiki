---
id: source-20261019-ai-3d-little-prince
title: 一款 3D 游戏全靠 AI 做出来了：把完整制作流程公开了
type: source
status: active
created: 2026-10-19
updated: 2026-10-19
tags:
  - 来源/文章
  - 主题/AI游戏制作
  - 主题/3D管线
  - 主题/Unity
  - 主题/Blender
aliases:
  - AI 小王子游戏制作流程
  - 战族狼魂 AI 3D 游戏流程
summary: 头条文章公开「小王子」主题 3D 游戏的 AI 全流程：Fable→Image→Tripo→Opus 操控 Blender/Unity→Astra 修 Bug；含场景/角色拆解与可复用提示词。开源尚未发布。
source_type: article
source_url: https://www.toutiao.com/article/7696353308520284722/
source_author: 战族狼魂
source_date: 2026-10-16
---

# 一款 3D 游戏全靠 AI 做出来了：把完整制作流程公开了

## 来源信息

- 标题：一款 3D 游戏全靠 AI 做出来了：把完整制作流程公开了
- 作者/媒体：战族狼魂
- 发布日期：2026-10-16
- 原文：https://www.toutiao.com/article/7696353308520284722/
- 说明视频：文中称《小王子游戏 AI 制作流程全拆解》同步制作，详见 Butterfly 号（全网同名）
- 开源状态：文中写「开源需要等段时间」，暂无公开仓库链接

## 摘要

作者以「小王子」主题可玩 3D 游戏为案例，公开从 PRD、场景与人物参考图、3D 资产生成、Blender/Unity 搭建到动作接入与 Bug 修复的完整 AI 流水线，并给出多条可复用提示词。核心论点是：真正耗时的建模与编码可交给 AI，人主要负责想法澄清、参考图定稿与提示词驱动。

## 原始摘录（结构化）

### 总体流水线（五步）

1. **Fable 5.1**：PRD，梳理核心场景、人物与剧情故事线
2. **GPT Image 2.5**：生成场景与人物参考图
3. **Tripo**：生成核心 3D 资产与人物模型（场景道具建议 H3.1；人物用 Smart Mesh P2.0 多视图）
4. **Opus 5.5**：搭建核心代码，操控 Blender + Unity 建模
5. **Astra**：迭代优化与 Bug 修复

### 工具配置

- **Blender**：CLI 优化 Tripo 模型并完成整体 3D；付费 **Auto-Rig Pro** 做绑骨蒙皮
- **Unity**：引擎打包场景/人物/交互；装 Unity CLI + **Pipeline** 包，经官方 **Unity MCP** 让 Claude/Agent 直接操作项目
- **Mixamo**：下载通用动作 FBX，再按角色骨骼适配；复杂接触动作在 Blender 二次细修

### 场景构建要点

- Image 2.5 先定一张满意效果图，再补俯视/斜侧等多视角
- 用 AI 标注：哪些资产用 Tripo 单独生成，哪些用 Blender/Unity 直接搭
- 将 Tripo 道具 + 多视角参考图交给 Opus，对照复刻并「多机位盲审」迭代

### 角色构建要点（以小王子为例）

1. Image 2.5 生成 **T-pose 四视图**（细节统一）
2. Tripo「多视图生成 3D」+ **Smart Mesh P2.0**（面数可控、利于绑骨）→ Texture / edit mesh
3. Blender + Auto-Rig Pro 绑骨蒙皮，并测抬手/弯腿/转身
4. Mixamo 搜动作并下载 FBX，由 Opus 适配骨骼
5. 接触类动作（如提水壶）在 Blender 用手臂 IK + 手指/道具位置细修

### 文中可复用提示词（摘录）

1. 场景全貌参考图：基于上传场景参考图，生成含多视角与局部细节的全面貌参考图
2. 资产拆解：技术栈 Tripo+Blender+Unity，分析哪些用 Tripo、哪些用 Blender，并在图上标注
3. 场景复刻：对照内外景多视角参考图与 Tripo 资产完美复刻，完成后派审查 agent 多机位盲审并持续迭代
4. 绑骨蒙皮：用 Auto-Rig Pro 对齐关节，测基础动作并修正变形/拉扯/穿模
5. 提水壶细修：保留 Mixamo 适用部分，用 Auto-Rig Pro 手臂 IK 与握持细节补全，避免穿模/悬空

### 成品场景举例

- 日落时分看第 44 次夕阳
- 大人的世界：酒馆与酒杯
- 狐狸的驯养
- 沙漠尽头的遗迹

### 工具链速查

Fable 5.1 · GPT Image 2.5 · Tripo（H3.1 / Smart Mesh P2.0）· Opus 5.5 · Blender（Auto-Rig Pro）· Unity（Pipeline + MCP）· Mixamo · Astra

## 可提炼的问题

- AI 驱动 3D 小游戏的推荐阶段划分与工具选型是什么？
- 场景资产如何在「生成式 3D」与「程序化/手搭」之间分工？
- 角色从参考图到可交互动画的最短可用路径是什么？

## 已提炼笔记

- [[30 知识资源/游戏开发/AI驱动的3D游戏制作流水线|AI驱动的3D游戏制作流水线]]
