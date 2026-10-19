---
id: evergreen-20261019-ai-3d-game-pipeline
title: AI驱动的3D游戏制作流水线
type: evergreen
status: active
created: 2026-10-19
updated: 2026-10-19
tags:
  - 知识/游戏开发
  - 主题/AI游戏制作
  - 主题/3D管线
  - 主题/Unity
  - 主题/Blender
  - 主题/Tripo
aliases:
  - AI 3D 游戏流水线
  - 小王子 AI 游戏制作流程
  - Fable-Tripo-Unity AI 管线
summary: 以可玩 3D 小游戏为目标的 AI 制作流水线：PRD→参考图→Tripo 资产→Agent 操控 Blender/Unity→动作接入与 Astra 修 Bug。人负责定想法与参考图，AI 承担建模与编码主力。
moc:
  - "[[40 知识导航/游戏开发|游戏开发]]"
  - "[[40 知识导航/知识地图|知识地图]]"
source:
  - "[[50 来源资料/文章/一款3D游戏全靠AI做出来了 - 完整制作流程|一款 3D 游戏全靠 AI 做出来了：把完整制作流程公开了]]"
related:
  - "[[30 知识资源/游戏开发/Mixamo - 免费人形角色动画平台|Mixamo - 免费人形角色动画平台]]"
  - "[[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]"
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/dsh-blender - DSH Blender 3D 生产插件|dsh-blender]]"
  - "[[50 来源资料/代码仓库/AI/多模态与模型/Modly - 本地AI图像转3D桌面应用|Modly]]"
---

# AI驱动的3D游戏制作流水线

## 核心结论

做可玩的小型 3D 游戏时，可以把制作拆成一条 **AI 流水线**：先用 AI 写清 PRD 与剧情，再用图像模型定场景/角色参考图，用生成式 3D（如 Tripo）出核心资产，再由 Agent（如 Opus）通过 Blender CLI + Unity MCP 完成摆放、代码与交互，最后用另一模型做迭代修 Bug。人的关键工作是 **把想法讲清楚、把参考图定死、用准确提示词驱动每一步**，而不是手工逐帧搭模型。

案例背景（来源文章）：「小王子」主题可玩 3D 游戏；作者称主要制作工作由 AI 完成，开源尚在准备中。

## 适用场景

### 更适合

- 独立/原型期要快速验证「可交互 3D 场景 + 角色动作」
- 已接受「生成式资产 + Agent 改引擎工程」的工作方式
- 技术栈可落到 Blender + Unity（含 CLI / MCP）

### 不太适合

- 需要严格版权闭环、资产全自研的商业大作管线
- 非人形/强物理接触演出占主导（生成与 Mixamo 预设都偏弱）
- 无法使用付费绑骨插件或商业 3D 生成服务的环境

## 五步流水线

| 步骤 | 工具（文中版本） | 产出 |
|------|------------------|------|
| 1. PRD / 叙事 | Fable 5.1 | 核心场景、人物、剧情线 |
| 2. 视觉锚定 | GPT Image 2.5 | 场景与人物参考图（多视角） |
| 3. 3D 资产 | Tripo（道具 H3.1；人物 Smart Mesh P2.0） | 可导入的网格与贴图 |
| 4. 工程落地 | Opus 5.5 + Blender + Unity（Pipeline + MCP） | 场景摆放、代码、可运行工程 |
| 5. 质量闭环 | Astra | 迭代优化与 Bug 修复 |

## 场景构建方法

1. **先定主效果图**，再补俯视、斜侧等视角（室内外都有时尤其重要）
2. **资产分工**：让 AI 标出「适合 Tripo 单独生成」与「适合 Blender/Unity 直接搭」的部分
3. **对照复刻**：把多视角参考图 + Tripo 道具交给 Agent，要求质感对齐，并做多机位盲审后迭代

提示词骨架（可复用）：

- 全面貌参考图：基于已有场景图，生成多视角 + 局部细节的全貌参考
- 资产拆解标注：按 Tripo / Blender 分工分析并在图上标注
- 场景复刻与盲审：完美对照参考图复刻 → 独立审查 agent 多机位找差异 → 循环迭代

## 角色构建方法

整体链路：

**参考图四视图 → Tripo 多视图生成 → Auto-Rig Pro 绑骨 → Mixamo 动作 →（可选）Blender 细修接触动作**

要点：

1. 人物先出 **T-pose 四视图**，保证各视角细节一致
2. Tripo 选面向游戏的拓扑（文中为 Smart Mesh P2.0）；白模合格再 Texture，形状不对用 edit mesh
3. 绑骨后立刻测抬手、弯腿、转身，尽早修穿模与权重
4. Mixamo 解决通用动作产能；**道具接触类动作**不要指望库里有完美剪辑，用接近动作 + IK/手指细节在 Blender 补

与现有笔记的关系：通用人形动作库与导出习惯见 [[30 知识资源/游戏开发/Mixamo - 免费人形角色动画平台|Mixamo]]。

## 工具配置清单

- Blender CLI +（可选付费）Auto-Rig Pro
- Unity + Unity CLI + Pipeline 包 + 官方 Unity MCP（供 Agent 直接改项目）
- Tripo、图像生成模型、PRD/Agent 模型（文中为 Fable / Image 2.5 / Opus / Astra）
- Mixamo（动作 FBX）

## 实践原则

1. **参考图是合同**：后续建模与摆放都以它为验收标准，先定图再铺量生成
2. **生成与搭建分工**：高细节孤品道具走生成式 3D；结构件、布局、灯光可走 Blender/Unity
3. **Agent 需要可操作接口**：Unity MCP / Blender CLI 比「只聊天出代码」更能闭环
4. **动作分两层**：通用位移/表演用 Mixamo；接触与道具交互单独细修预算
5. **开源与复现**：案例工程开源未就绪时，优先沉淀工具链与提示词，而不是等待完整工程包

## 参考来源

- [[50 来源资料/文章/一款3D游戏全靠AI做出来了 - 完整制作流程|头条原文摘录与提示词]]
- Mixamo：https://www.mixamo.com/
- Unity：https://unity.com/

## My takeaways

1. 这条流水线的价值在「阶段清晰 + 每步有工具与验收」，不是某一个神模型。
2. 和 GameFactory-3A 等「生成向 Skill/资产框架」可互补：前者偏端到端案例方法，后者偏可复用工程资产。
3. dsh-blender / Modly 等可替换或增强中间的 Blender/图像转 3D 环节，流水线骨架仍适用。
4. 若要在 unicore / 团结引擎落地，重点验证：Agent 能否稳定改工程、生成网格拓扑是否利于运行时、动作导出格式是否引擎友好。
