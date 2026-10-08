---
id: source-20261008-mindustry
title: Mindustry - 自动化塔防RTS开源游戏
type: source
status: active
created: 2026-10-08
updated: 2026-10-08
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/开源游戏
  - 主题/RTS
  - 主题/塔防
  - 主题/工厂建造
  - 主题/Java
author: Anuken
source_type: repo
source_url: https://github.com/Anuken/Mindustry
source_author: Anuken
source_date: 2026-10-08
summary: 单人维护九年的工厂+塔防+RTS 开源游戏。Java/libGDX（自研 Arc），GPL-3.0；手机与 itch 免费无广告，Steam 买断约 $9.99。约 29k GitHub Stars，适合学跨平台游戏工程与开源商业化样本。
related:
  - "[[40 知识导航/游戏开发|游戏开发]]"
---

# Mindustry - 自动化塔防RTS开源游戏

## Source summary

**Mindustry**（仓库 [Anuken/Mindustry](https://github.com/Anuken/Mindustry)）是一款「自动化塔防 RTS」：采矿建厂、拉供应链、抵御波次、反推敌方基地，并把《异星工厂》式物流与塔防/RTS 拼在一起。作者 **Anuken** 自 2017 年游戏 jam 原型起独立维护至今。

- ~**29,267** Stars / ~**3,823** Forks（2026-10-08 GitHub API）
- 协议：**GPL-3.0**
- 语言：**Java**（JDK 17；Gradle 多模块）
- 引擎：基于 **libGDX**，作者自研 **[Arc](https://github.com/Anuken/Arc)** 层
- 官网 / Wiki：https://mindustrygame.github.io 、https://mindustrygame.github.io/wiki
- 默认分支：`master`

## 来源线索

整理自头条文章《一个人写了9年：免费开源的它，Steam好评95%，还赚了近400万美元》（gid `7691860426237968936`）：

- 文章写作时约 **29.1k Star**、**2 万+ 提交**；Steam 约 9,000 条评测 **95% 好评**（近期约 97%）
- 第三方统计总收入约 **390 万美元**；Steam 约卖出 **67 万份**（文章数据，非官方财报）
- 商业结构：itch / 手机（含 F-Droid）**免费无广告无内购**；Steam **$9.99 买断** + 装饰性 DLC；源码可编译自用
- 时间线摘要：2017-04 jam 冠军原型 → itch / Google Play → 2019-09 Steam → v6 逻辑处理器 → v7 第二星球 Erekir

## 核心特性

| 特性 | 说明 |
|------|------|
| **工厂物流** | 钻头采矿、传送带、冶炼、液体管道、电力网络；炮塔弹药依赖供应链 |
| **塔防波次** | 敌人进攻核心；防线与物流必须同时成立 |
| **RTS 反推** | 攒机械化部队平推敌方基地；战役跨两颗星球 |
| **多模式** | 生存/进攻战役、程序生成关卡、沙盒、PvP、多人合作 |
| **可编程 mlog** | 内置逻辑语言，可用处理器脚本控制建筑与单位 |
| **跨平台** | Desktop（Win/macOS/Linux）、Android、iOS、独立多人服务器，共享核心代码 |

## 适用边界

### 更适合参考

- 工厂建造 / 塔防 / 轻量 RTS 玩法与「物流即防线」设计
- Java + libGDX（或自研引擎层）跨平台中型游戏工程
- GPL 开源 + 免费端 + 付费便利层（Steam/创意工坊/云存档）的商业化样本
- 单人长期维护 + 社区 PR / Suggestions 仓库的协作流程

### 不太适合直接拿来用

- 需要闭源商用 fork：协议为 **GPL-3.0**，衍生分发义务重，需法务评估
- Unity / C# 引擎选型（本仓是 Java 游戏本体，不是可嵌入中间件）
- 追求极低学习曲线的休闲向产品（资源链与策略深度偏陡）

## 技术与本地运行

### 环境

| 项 | 要求 |
|----|------|
| JDK | **必须 JDK 17**（其他版本不可用） |
| 构建 | Gradle Wrapper（`gradlew`） |
| 桌面运行 | `gradlew desktop:run` |
| 桌面打包 | `gradlew desktop:dist` → `desktop/build/libs/Mindustry.jar` |
| 服务器 | `gradlew server:dist` |
| Android | 配置 `ANDROID_HOME` 后 `gradlew android:assembleDebug` |

说明：`mindustry.gen` 等包在**构建时生成**（注解/`@Remote`/实体组件等），仓库里没有对应手写源码。

### 相关仓库与渠道

| 资源 | 链接 |
|------|------|
| 功能建议仓 | https://github.com/Anuken/Mindustry-Suggestions |
| 每日构建 | https://github.com/Anuken/MindustryBuilds/releases |
| Arc 引擎层 | https://github.com/Anuken/Arc |
| Discord | https://discord.gg/mindustry |
| Trello 规划 | https://trello.com/b/aE2tcUwF/mindustry-40-plans |
| Javadoc | https://mindustrygame.github.io/docs/ |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/Anuken/Mindustry |
| **官网** | https://mindustrygame.github.io |
| **Wiki** | https://mindustrygame.github.io/wiki |
| **itch.io** | https://anuke.itch.io/mindustry |
| **Google Play** | https://play.google.com/store/apps/details?id=io.anuke.mindustry |
| **F-Droid** | https://f-droid.org/packages/io.anuke.mindustry |
| **Flathub** | https://flathub.org/apps/details/com.github.Anuken.Mindustry |
| **Steam**（商店页可搜 Mindustry） | https://store.steampowered.com/ |
| **源头报道（头条）** | https://www.toutiao.com/article/7691860426237968936/ |

## My takeaways

1. **开源地址已确认**：主仓 `Anuken/Mindustry`，GPL-3.0，活跃维护中。
2. **玩法定位**：工厂物流 + 塔防波次 + RTS 反推；内置 mlog 可编程层是差异点。
3. **商业启发**：开放源码与收费不冲突——收费买的是分发便利与支持，不是「藏代码」。
4. **工程价值**：真实中型 Java 跨平台项目；构建时代码生成、Gradle 多模块、自研 Arc 层都可翻。
5. **风险点**：学习曲线陡；方向长期系于单一维护者；借鉴代码需遵守 GPL。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[50 来源资料/代码仓库/游戏开发/OpenFrontIO - 开源浏览器RTS游戏|OpenFrontIO - 开源浏览器RTS游戏]]
