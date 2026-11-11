---
id: source-20261111-shipmygame
title: ShipMyGame - Unity游戏上架Agent Skills
type: source
status: active
created: 2026-11-11
updated: 2026-11-11
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/Unity
  - 主题/AI
  - 主题/Agent
  - 主题/Skills
  - 主题/开源资源
  - 主题/游戏发行
  - 主题/Google Play
  - 主题/微信小游戏
  - 主题/TapTap
author: BaronCyrus
source_type: repo
source_url: https://github.com/BaronCyrus/ShipMyGame
source_author: BaronCyrus
source_date: 2026-11-02
summary: 面向 Claude Code / Codex / Cursor 等 Agent 的 Unity 游戏上架 Skills：从注册开发者账号到 Google Play、微信小游戏、TapTap 小游戏提审，步骤来自真实上架踩坑；MIT。
related:
  - "[[40 知识导航/游戏开发|游戏开发]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[50 来源资料/代码仓库/游戏开发/unity-agent-plugin - Unity官方Agent技能插件|unity-agent-plugin]]"
  - "[[50 来源资料/代码仓库/游戏开发/awesome-gamedev-agent-skills - 多引擎游戏开发Agent Skills合集|awesome-gamedev-agent-skills]]"
  - "[[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills]]"
  - "[[50 来源资料/文章/网页小游戏变现 - 11个平台分成门槛提交方式核实版|网页小游戏变现]]"
  - "[[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]"
discovery_source: https://github.com/BaronCyrus/ShipMyGame
---

# ShipMyGame - Unity游戏上架Agent Skills

## Source summary

**ShipMyGame**（[BaronCyrus/ShipMyGame](https://github.com/BaronCyrus/ShipMyGame)）是一套面向 AI Agent 的开源 **Unity 游戏上架 Skills**：把仓库交给 Agent，用一句话推进到 Google Play / 微信小游戏 / TapTap 小游戏的注册、构建、后台填表与提审。步骤来自作者用 AI 做两款 Unity 6 小游戏并真实上架时踩过的坑（无声、中文方块、微信黑屏、TapTap 准入连挂等）。

核心一句话：**发布手册式 Agent Skills = 工程体检 → 渠道注册 → 打包素材 → 接管后台 → 你确认后再提审。**

- 约 **14 stars / 1 fork**（截至 2026-11-11 GitHub API）
- 许可证：**MIT**；作者：[BaronCyrus](https://github.com/BaronCyrus)
- 默认分支：`main`；仓库创建约 2026-11-02；语言以 **C#** 辅助脚本为主
- 插件元数据版本约 `0.1.0`；Topics：`agent-skills`、`unity`、`google-play`、`wechat-minigame`、`taptap` 等
- 兼容入口：Claude Code、Codex、Cursor 等支持 Agent Skills 的工具
- 发现来源：GitHub 检索 ShipMyGame（Unity 上架向）

解决的痛点：做成游戏之后，各渠道注册、合规、打包、后台、备案、收款文档分散且易踩坑；本库把「实测」路径固化成 Agent 可执行的 Skills，而不是保证自动获批。

## 核心特性

| 特性 | 说明 |
|------|------|
| **总入口路由** | `ship-my-game`：读进度 → 选渠道 → 体检工程 → 准备浏览器 → 交给对应渠道 Skill |
| **多渠道实测** | Google Play、微信小游戏、TapTap 小游戏；Steam 草稿中；App Store / 抖音等在路线图 |
| **工程体检** | 上架前检查无声、中文方块、存档、刘海/胶囊遮挡、测试广告未换等 |
| **人机分工清晰** | Agent 填表/上传/核对；付款、实名、签字、扫码、点「提交」前确认由本人完成 |
| **进度可续** | 审核与备案可能数天到数周，进度写在游戏工程 `publish/STATE.md` |
| **讲清收款** | AdMob、Play 内购、微信流量主、TapTap 广告；银行卡与税务；美元结汇与个税注意点 |
| **密钥本地** | 签名/上传密钥只放本机 `~/.shipmygame/keys/`，不进 git |

## 技能与渠道地图

| Skill | 职责 | 实测进度（README 口径） |
|-------|------|-------------------------|
| `ship-my-game` | 总入口：进度、渠道、体检、浏览器 | — |
| `ship-google-play` | 签名 AAB、Play 注册到封闭测试/正式版申请；Play 服务、AdMob、内购等 | 两款游戏走到封闭测试与更新送审；一款已提交正式版申请 |
| `ship-wechat-minigame` | 个人主体微信小游戏：快适配、备案、提审；流量主与常见黑屏/字体坑 | 已发布上线（单机；Unity 6 黑屏已修） |
| `ship-taptap-minigame` | TapTap 小游戏：上传、准入测试、商店资料、前置备案 | Unity 6 包已通过准入；资料与前置备案进行中 |
| `ship-steam` | Steamworks 注册与收款税务等 | 规划中：注册/收款/税务已实测，建应用之后未走完 |

每步标注 `[实测]`（真实账号跑通）或 `[官方]`（按文档整理、待实测）。

## 适用边界

### 更适合

- 已有（或接近可发）**Unity** 游戏，需要推进 **Google Play / 微信小游戏 / TapTap 小游戏** 上架
- 希望用 Claude Code / Codex / Cursor 等 Agent 执行「上架手册」，而不是从零搜文档
- 需要个人主体路径、备案、测试人数、广告与收款等实操提醒

### 需要注意

- **不是**游戏引擎，也不是一句话生成游戏；构建仍依赖本机 Unity、平台 SDK、浏览器控制与开发者账号
- 安装 Skills **不会**自动补齐 Unity / SDK / 账号权限；某步能否完成取决于工具与权限是否齐备
- 仓库较新（约 2026-11 起），stars 尚少；渠道后台与 SDK 会变，需关注仓库周更巡检与 Issue
- 与 [[50 来源资料/代码仓库/游戏开发/unity-agent-plugin - Unity官方Agent技能插件|unity-agent-plugin]] 互补：官方插件偏**引擎内开发规范**；本库偏**商店上架与发行运维**

## 快速开始

**方式一：直接发链接**（任意能上网的 Agent）

在 Unity 工程目录的 Agent 会话中：

```text
用 https://github.com/BaronCyrus/ShipMyGame 帮我把这个游戏发布到微信小游戏
```

**方式二：装成技能**

```bash
npx skills add BaronCyrus/ShipMyGame
```

**方式三：Claude Code 插件**

```text
/plugin marketplace add BaronCyrus/ShipMyGame
/plugin install ship-my-game@shipmygame
```

**给 Agent 的固定用法（README）**

1. 把仓库 clone 到工具目录，**不要**放进游戏工程：`~/.shipmygame/repo`
2. 读 `skills/ship-my-game/SKILL.md`，从第 1 步照做
3. 当前目录即游戏工程；进度写在 `publish/STATE.md`

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/BaronCyrus/ShipMyGame |
| **README** | https://github.com/BaronCyrus/ShipMyGame/blob/main/README.md |
| **总入口 Skill** | https://github.com/BaronCyrus/ShipMyGame/tree/main/skills/ship-my-game |
| **维护说明** | https://github.com/BaronCyrus/ShipMyGame/blob/main/MAINTAINING.md |
| **评测参考文** | https://blog.mushroom.cv/blog/shipmygame-unity-publishing-agent-skills-review/ |
| **许可证** | MIT |

## My takeaways

1. **定位**：Unity 成品/准成品的 **发行上架 Agent Skills**，不是内容生产管线。
2. **与 unity-agent-plugin**：开发期用官方 Skills；临近上架用 ShipMyGame，可串联使用。
3. **与 awesome-gamedev-agent-skills / threejs-game-skills**：后两者偏多引擎或浏览器**制作**；本库专攻 **Unity → 具体商店渠道**。
4. **检索关键词**：ShipMyGame、ship-my-game、Unity 上架、Google Play Agent Skill、微信小游戏 Unity 6、TapTap 小游戏准入、游戏发行 Agent。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[50 来源资料/代码仓库/游戏开发/unity-agent-plugin - Unity官方Agent技能插件|unity-agent-plugin - Unity官方Agent技能插件]]
- [[50 来源资料/代码仓库/游戏开发/awesome-gamedev-agent-skills - 多引擎游戏开发Agent Skills合集|awesome-gamedev-agent-skills - 多引擎游戏开发Agent Skills合集]]
- [[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills - Three.js浏览器游戏Agent Skills合集]]
- [[50 来源资料/文章/网页小游戏变现 - 11个平台分成门槛提交方式核实版|网页小游戏变现 - 11个平台分成门槛提交方式核实版]]
- [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
