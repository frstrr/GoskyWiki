---
id: source-20260901-img2threejs
title: img2threejs - 图片转程序化 Three.js 模型
type: source
status: active
created: 2026-09-01
updated: 2026-09-01
tags:
  - 来源/代码仓库
  - 主题/AI编程Agent
  - 主题/Three.js
  - 主题/3D重建
  - 主题/前端
  - 主题/工程技能
author: img2threejs
source_type: repo
source_url: https://github.com/img2threejs/img2threejs
source_author: img2threejs
source_date: 2026-07-15
summary: 面向编码代理的图片→程序化 Three.js 技能：用参考图生成纯代码、可动画、质量门控的 THREE.Group 工厂，而非摄影测量或网格导出。强调 Token 高效（脚本做校验门控，模型只做视觉判断）。
related:
  - [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
  - [[40 知识导航/AI 工具使用|AI 工具使用]]
  - [[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
  - [[50 来源资料/代码仓库/AI/Skills与方法论/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]
---

# img2threejs - 图片转程序化 Three.js 模型

## Source summary

**img2threejs** 是一套面向 Claude Code / Codex / OpenCode 等编码代理的 **Agent Skill**：给定一张物体/角色参考图，生成可在浏览器运行的 **纯代码程序化 Three.js 模型**（`THREE.Group` 工厂 + TypeScript），而不是摄影测量网格、下载素材包或 GLB 资产包。

- **~14.7k Stars / ~1.2k Forks**（截至 2026-09-01）
- **Apache-2.0** 协议，当前版本约 **v1.5.1**
- 官网演示画廊：[https://img2threejs.io/](https://img2threejs.io/)
- 工具链：Python 3.10+ 标准库（`forge/` 脚本，零 pip 依赖）+ Three.js 运行时
- 来源线索：头条文章介绍该 GitHub 项目（图片秒变 Three.js）

核心一句话：**用代码重建参考图中的对象，并带质量门控与动画就绪层级。**

## 核心特性

| 特性 | 说明 |
|------|------|
| **Code-only 程序化重建** | 输出 TypeScript 工厂函数，用图元、程序化着色器与生成几何体重建对象；结果可 diff、可版本管理 |
| **分阶段雕刻管线** | `blockout → structural → form → material → surface → lighting → interaction → optimization`，逐 pass 生成并视觉评审 |
| **严格质量门控** | `detailInventory` + `--strict-quality`：身份关键细节未落到真实组件前禁止 codegen |
| **动画就绪层级** | 暴露 pivots / sockets / colliders；角色构建可绑定骨架与测地线蒙皮 |
| **对象 / 角色 / 混合路由** | 硬表面走 object 管线；角色走解剖感知轨道（比例、面部地标、姿态） |
| **Token 高效设计** | 机械校验与状态机交给确定性 Python 脚本；模型 tokens 只用于并排对比图的 pass/fail 判断 |
| **可恢复本地工作流** | `forge/state.py` + `forge/next.py --state` 支持跨会话断点续做 |
| **可选增强** | 多视角 silhouette carving、人物 likeness 最大化、CS2 武器专用评审门、GLB 作测量基线的角色管线 |

## 它不做什么

- **不是** 摄影测量 / NeRF / 网格提取
- **不是** 直接导出可商用的高精度扫描模型
- 单张图无法看到背面；未见区域会明确标低置信度或镜像推断，而不是假装精确
- 角色是风格化重建，不是照片级人像复制

## 产出物

1. **ObjectSculptSpec JSON**：组件树、材质、重复系统、sockets、各 pass 评审历史
2. **TypeScript 工厂**：`createXxxModel(spec, options)` → `THREE.Group`，`userData.sculptRuntime` 暴露节点/插槽/碰撞体
3. **角色额外**：`userData.rig`（骨骼、共享 Skeleton、绑定状态）
4. **对比评审图**：每 pass 参考图 vs 渲染并排

## 快速开始

```bash
git clone https://github.com/img2threejs/img2threejs.git ~/.claude/skills/img2threejs
```

多主机共用同一 checkout（避免漂移）：

```text
~/.claude/skills/img2threejs -> <your checkout>
~/.codex/skills/img2threejs  -> <your checkout>
```

在 Claude Code 中附上参考图并调用：

```text
/img2threejs Rebuild this object as a Three.js model, keep the proportions, angles, and colours.
```

跨会话重建可先建状态：

```bash
python3 forge/state.py init --reference <image> --profile character --spec object-sculpt-spec.json
python3 forge/next.py --state .img2threejs/state.json
```

## 管线要点（Token 效率）

- **脚本强制，模型判断**：校验、门控、spec 写作、PBR 证据、对比图打包由脚本完成；模型只看一张对比图做决策
- **零依赖**：纯 Python 标准库，无需 pip / PIL / numpy / Playwright
- **按 pass 解锁生成**：每次只生成当前解锁阶段，不整模反复重读
- **失败即停**：浅层 spec 在 codegen 前被挡住，避免白烧 tokens
- **文本产物**：TypeScript + JSON，而非数 MB 网格二进制

## 版本与路线图（摘要）

| 版本 | 主题 |
|------|------|
| v1.0–v1.3 | 物体管线、细节优先、人形角色、质量与效率门控 |
| v1.4 | Weapon Update（CS2 图像匹配重建、武器族适配器） |
| v1.5 | Character Update（骨架、测地线蒙皮、发型子系统、材质门控、可恢复状态） |
| v1.6（规划） | Environment：建筑/房间/街道/植被 |
| v1.7（规划） | Game Pipeline：Unity/Unreal 导出、Blender 桥、LOD/碰撞 |
| v1.8–v2.0（规划） | 动画、AI Studio Web UI、程序化世界与插件生态 |

Showcase 演示仓库：[img2threejs/img2threejs-showcase](https://github.com/img2threejs/img2threejs-showcase)

## 相关链接

| 类型 | 链接 |
|------|------|
| GitHub 仓库 | https://github.com/img2threejs/img2threejs |
| 在线演示画廊 | https://img2threejs.io/ |
| Showcase 仓库 | https://github.com/img2threejs/img2threejs-showcase |
| 架构文档 | https://github.com/img2threejs/img2threejs/blob/main/docs/ARCHITECTURE.md |
| Token 成本说明 | https://github.com/img2threejs/img2threejs/blob/main/docs/TOKEN_COST.md |
| 路线图 | https://github.com/img2threejs/img2threejs/blob/main/ROADMAP.md |
| Issues | https://github.com/img2threejs/img2threejs/issues |
| 赞助 | https://ko-fi.com/iamnick |
| 介绍文章（头条） | https://www.toutiao.com/article/7670352441531761195/ |

## My takeaways

1. **定位清晰**：给前端/Agent 一条「有图就能出可维护 Three.js 代码」的路径，解决「难的是没有模型」而非 Three.js API 本身。
2. **和普通 image-to-3D 不同**：强调程序化代码 + 质量门控 + 动画层级，产物适合进前端仓库，而不是再导入 DCC。
3. **适合作为 Skill 沉淀**：与 Superpowers / mattpocock skills 同类——装进 agent skills 目录后用斜杠命令驱动，适合需要批量做硬表面/风格化角色原型时调用。
4. **硬表面更稳，角色需预期风格化**：官方也诚实写明单图局限；要高保真人像需多视角或 likeness 路径，且仍不保证 100%。
5. **后续关注点**：v1.7 Unity/Unreal 导出若落地，可与游戏项目管线衔接；当前更适合 Web Three.js 场景与原型验证。
6. **与 unicore 关系**：unicore 是 Unity 框架；本项目当前主战场是浏览器 Three.js。短期可作「程序化资产生成思路」参考，Unity 导出需等路线图或自行桥接。

## Related

- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]