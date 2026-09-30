---
id: source-20260901-nowui
title: NowUI - Unity 即时模式 UI 库
type: source
status: active
created: 2026-09-01
updated: 2026-09-01
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/Unity
  - 主题/UI
  - 主题/即时模式UI
author: BlenMiner
source_type: repo
source_url: https://github.com/BlenMiner/NowUI
source_author: BlenMiner
source_date: 2026-09-01
summary: Unity 即时模式（immediate-mode）UI 渲染库。每帧调用绘制 API 后 flush，无需 GameObject 层级或保留式 UI 树；提供矩形批处理、MSDF 文本、类 CSS 渐变、类 flexbox 布局、指针/触摸/手柄交互、主题与 Lottie 矢量动画，并统一支持 Built-in/URP/HDRP、UGUI、UI Toolkit、世界空间、RenderTexture、IMGUI。
related: []
---

# NowUI - Unity 即时模式 UI 库

## Source summary

**Now-UI / NowUI** 是面向 Unity 的即时模式 UI 渲染库：每帧调用绘制 API 并 flush，不依赖 GameObject 层级，也不维护保留式 UI 树。工具箱较完整，覆盖批处理矩形、运行时编译的 MSDF 文本、类 CSS 渐变、类 flexbox 布局、指针/触摸/手柄交互、主题系统与 Lottie 矢量动画。

同一套绘制代码可走 Built-in（`GL`/`Graphics.DrawMeshNow`）、URP、HDRP、UGUI `CanvasRenderer`、UI Toolkit/UXML、世界空间 `MeshRenderer`、`RenderTexture` 或 IMGUI。

- ~35 Stars / 4 Forks
- MIT 协议开源
- 作者：BlenMiner
- 语言：C#
- 默认分支：`main`
- 更新活跃（2026 年仍在持续提交）

## 核心特性

| 特性 | 说明 |
|------|------|
| **即时模式绘制** | 每帧声明式绘制并 flush，无保留式控件树 / GameObject UI 层级 |
| **多宿主渲染** | Built-in / URP / HDRP / UGUI / UI Toolkit / 世界空间 / RenderTexture / IMGUI 共用绘制代码 |
| **矩形与遮罩** | 圆角（可分角）、描边、模糊、padding、纹理/精灵、自定义材质；精确矩形裁剪 + 抗锯齿解析遮罩 |
| **渐变与玻璃** | 线性 / 径向 / 锥形渐变；毛玻璃背景模糊（CommandBuffer / RT / UGUI replay） |
| **文本** | Burst 编译的运行时 MSDF 字体；可选原生插件覆盖 CFF / 彩色 emoji；HarfBuzz shaping（有插件时） |
| **布局** | 流式 `Row`/`Horizontal`、`Column`/`Vertical`，gap / padding / grow / align / justify |
| **控件与主题** | 按钮、开关、滑条、输入框、下拉、滚动视图；ScriptableObject 主题 token |
| **高级能力** | SDF 可组合形状、Docking、Node Graph、Markdown 渲染、Lottie CPU 矢量镶嵌、3D Model Preview |
| **分析器** | 自带 Roslyn Analyzer，编译期提示漏写 `.Draw()` 等误用 |

## 适用边界

### 更适合

- 需要高度自定义、代码驱动的游戏内 HUD / 工具面板 / 编辑器式 UI
- 希望同一套绘制逻辑跨 URP/HDRP/UGUI/世界空间复用
- 需要即时模式交互（悬停、按下、拖拽、点击）且热路径尽量零分配
- 需要 SDF 特效、Docking、节点图、Markdown、Lottie 等增强能力

### 不建议优先使用

- 只想快速用 Unity 默认 UGUI/UI Toolkit 搭标准表单页的项目
- Unity 版本低于 `6000.4`（库明确要求 Unity 6.4+）
- 团队不接受即时模式心智模型（每帧重绘、显式 `StartUI`/`Draw` 生命周期）的项目

## 安装与环境

### UPM 安装（推荐）

Package Manager → `+` → Install package from git URL：

```text
https://github.com/BlenMiner/NowUI.git?path=Assets/NowUI
```

也可克隆仓库后作为 Unity 工程直接打开，用于开发/调试 NowUI 本身。

### 环境要求

| 项 | 要求 |
|----|------|
| Unity | `6000.4` 或更新 |
| 自动依赖 | Burst、Collections、Mathematics |
| 可选输入 | `com.unity.inputsystem`（手柄导航等更可靠；缺省回退 Legacy Input） |
| 可选宿主 | `com.unity.ugui`（`NowGraphic` 等）、`com.unity.modules.uielements`（`NowVisualElement` 等） |

可选包只要出现在 Unity 解析后的依赖图中即可被自动检测，无需手写 scripting define。

## 快速上手心智模型

- 已有明确矩形坐标：用 `Now` 原语直接画
- 希望库自动排布行列：用 `NowLayout`（`Row`/`Column` 等）
- UGUI 宿主：`NowGraphic`（显式放置）或 `NowLayoutGraphic`（测量/绘制由宿主接管）
- 手动宿主（如相机回调）：`Now.StartUI(...)` + 需要时 `NowLayout.RunMeasured(...)`
- 屏幕绘制务必包在 `using (Now.StartUI(...))` 中，dispose 时提交渲染并收尾输入

## 功能地图（摘录）

| 能力 | 文档入口（仓库内） |
|------|-------------------|
| 矩形 / 特性总览 | `Assets/NowUI/Documentation~/Features.md` |
| 遮罩 | `Documentation~/Masks.md` |
| 渐变 | `Documentation~/Gradients.md` |
| 玻璃效果 | `Documentation~/Glass.md` |
| 线条 / 形状 | `Documentation~/Lines.md` / `Shapes.md` |
| 特效 / 顶点变形 | `Documentation~/Effects.md` |
| 3D 模型预览 | `Documentation~/ModelPreviews.md` |
| 世界空间 UI | `Documentation~/WorldSpace.md` |
| 文本预处理 / 本地化钩子 | `Documentation~/TextPreprocessor.md` |
| 布局 | `Documentation~/Layout.md` |
| 控件 | `Documentation~/Controls.md` |
| 主题 | `Documentation~/StylesAndThemes.md` |
| 身份 ID | `Documentation~/Identity.md` |
| Lottie | `Documentation~/Lottie.md` |
| Markdown | `Documentation~/Markdown.md` |
| Docking | `Documentation~/Docking.md` |
| SDF 形状 | `Documentation~/SDF.md` |
| 渲染管线宿主 | `Documentation~/RenderPipelines.md` |
| 移动端 | `Documentation~/Mobile.md` |
| API / 分配约定 | `Documentation~/API.md` |
| AI Agent 指南 | `Documentation~/AI_GUIDE.md` |

## 平台与原生插件

原生插件（`nowui-msdf` 字体编译、`nowui-vg` Lottie 镶嵌）预编译提交在 `Assets/NowUI/Plugins`：

| 平台 | 字体编译 | Lottie |
|------|----------|--------|
| Windows / Linux / macOS x64·arm64 | native | native |
| Android arm64 / iOS / WebGL | native（iOS/WebGL 静态链接） | native |
| 其他（主机等） | Burst 托管回退 | 托管回退 |

原生插件是性能增强而非硬依赖；无二进制平台仍可运行。托管字体回退覆盖 TrueType（glyf）；CFF OpenType 与彩色 emoji 仍需原生编译器。

## 仓库结构

| 路径 | 说明 |
|------|------|
| `Assets/NowUI` | UPM 包本体（运行时、编辑器、示例、原生插件） |
| `Assets/NowUI/Runtime` | 绘制 API、布局、输入、文本、Lottie、主题与各宿主集成 |
| `Assets/NowUI/Editor` | 字体编译菜单、`.lottie` 导入器 |
| `Assets/NowUI/Example` | 示例（邮件客户端 mockup、落地页、世界空间标签等） |
| `Assets/NowUI/Documentation~` | 与版本匹配的公开文档 |
| `Assets/NowUITests` / `Assets/NowUIHarness` | 仓库专用测试与视觉/性能测试（不进包导出） |
| `Docs` | 维护者向设计 / 基准 / 发布说明 |

## AI 编程支持

UPM 包内含版本匹配文档、作用域 `AGENTS.md`，以及可安装的 `nowui` skill：

- Unity 菜单：`Tools > NowUI > AI > Install Agent Skill`
- 或从 `Assets/NowUI/Documentation~/AI_GUIDE.md` 开始
- 另有 `Copy Project AGENTS.md Snippet` 便于写入项目级 Agent 说明

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/BlenMiner/NowUI |
| **UPM Git URL** | `https://github.com/BlenMiner/NowUI.git?path=Assets/NowUI` |
| **README** | https://github.com/BlenMiner/NowUI/blob/main/README.md |
| **AI 指南** | https://github.com/BlenMiner/NowUI/blob/main/Assets/NowUI/Documentation~/AI_GUIDE.md |
| **Issues** | https://github.com/BlenMiner/NowUI/issues |
| **许可证** | MIT（[LICENSE.md](https://github.com/BlenMiner/NowUI/blob/main/LICENSE.md)） |
| **第三方许可** | https://github.com/BlenMiner/NowUI/blob/main/Assets/NowUI/THIRD_PARTY_LICENSES.md |

## My takeaways

1. **定位明确**：即时模式 UI，不是 UGUI 替代控件库；适合代码驱动、跨宿主复用的自定义界面。
2. **渲染宿主覆盖广**：同一套 API 打通 Built-in/URP/HDRP/UGUI/UI Toolkit/世界空间，对工具型面板和游戏内 HUD 都很有参考价值。
3. **功能面偏「完整工具箱」**：布局、控件、主题、SDF、Docking、节点图、Markdown、Lottie 都有，不只是画矩形。
4. **版本门槛偏高**：要求 Unity `6000.4+`，老项目无法直接引入，可先作技术参考。
5. **对 Agent 友好**：自带 `AGENTS.md` 与 skill，后续用 AI 接入成本相对低。
6. **与 unicore 的关联**：若 unicore 需要自定义即时 UI、跨管线 HUD、世界空间名牌或编辑器式 Docking/节点图，可作为开源候选或实现参考；需先确认项目 Unity 版本是否满足。

## Related

- （待补充：若后续建立 `30 知识资源/游戏开发/` 或 Unity UI 选型导航页，可在此链接）
