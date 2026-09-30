---
id: source-20260924-openfrontio
title: OpenFrontIO - 开源浏览器RTS游戏
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/浏览器游戏
  - 主题/RTS
  - 主题/SLG
  - 主题/开源游戏
author: openfrontio
source_type: repo
source_url: https://github.com/openfrontio/OpenFrontIO
source_author: openfrontio
source_date: 2026-09-24
summary: 在线浏览器即时战略（领土控制 + 结盟）开源游戏。TypeScript 全栈，网页版永久免费，Steam 抢先体验与网页端共享服务器；社区驱动，AGPL-3.0。适合研究轻量网页 SLG/RTS、浏览器大战略传播与开源游戏社区协作。
related: []
---

# OpenFrontIO - 开源浏览器RTS游戏

## Source summary

**OpenFront / OpenFrontIO**（官网 [openfront.io](https://openfront.io/)）是一款在线实时战略游戏，核心玩法是领土扩张、建造与结盟。地图基于真实地理区划，玩家在浏览器中即可开局，无需下载甚至不必注册。

技术上是 **WarFront.io** 的 fork/重写（致谢：https://github.com/WarFrontIO）。核心团队极小（公开资料约 3 人：创始人/维护者 Evan Pellegrini，技术 Josh Harris，运营 Lewis Malton），但 GitHub 社区贡献活跃。

- ~2720 Stars / ~1400 Forks（与文章描述一致）
- 协议：源码 **AGPL-3.0**；美术资源 **CC BY-SA 4.0**
- 语言：TypeScript（npm / webpack / Express）
- 默认分支：`main`
- 官网：https://openfront.io/
- Steam：https://store.steampowered.com/app/3560670/OpenFront/（抢先体验；网页版仍永久免费并共享玩家池）

## 来源线索

整理自头条文章《每天20多万人游玩，这款浏览器SLG终于上Steam了》（gid `7688611606078423552`）：

- 上线 Steam 前网页版已有超 100 万月活；官方称连续数月日活 20 万+；广告合作方数据约日活 20–25 万
- 定位为「轻量化 SLG / 在线 RTS」：降低传统大战略学习成本，强调 PVP、外交翻脸与领土争夺
- 产品谱系参考：Risk → OGame/Travian 等浏览器策略 → Territorial.io → WarFront → OpenFront
- 传播路径偏社区/主播，而非传统买量；网址本身即试玩入口

## 核心特性

| 特性 | 说明 |
|------|------|
| **实时领土争夺** | 扩张地盘、交战、资源与防御设施建设 |
| **结盟系统** | 与其他玩家结盟互助，亦可随时翻脸 |
| **多地图** | 欧洲、亚洲、非洲等多区域真实地理地图 |
| **跨平台网页** | 现代浏览器即可玩；Steam 客户端提供离线/外观等增值体验 |
| **社区共建** | PR 审核 + 贡献者持续提交内容与功能；支持 Crowdin 本地化 |

## 适用边界

### 更适合参考

- 浏览器 / 网页多人 RTS、io 游戏、轻量 SLG 玩法设计
- 「网页即客户端 + 试玩页」的获客与留存路径
- 小团队 + 开源社区驱动的游戏项目组织方式
- TypeScript 全栈网页游戏（客户端 webpack + 服务端 Express）架构参考

### 不太适合直接当引擎用

- 需要传统 RTS 微操、编队、复杂兵种克制的项目（本作刻意弱化微操）
- 希望闭源商用 fork：协议为 **AGPL-3.0**，网络服务衍生版本义务较重，需法务评估
- Unity / 原生客户端引擎选型（本仓库是浏览器 TS 栈，不是引擎库）

## 技术与本地运行

### 环境

| 项 | 要求 |
|----|------|
| Node / npm | npm ≥ 10.9.2 |
| 浏览器 | Chrome / Firefox / Edge 等现代浏览器 |
| 安装命令 | 必须用 `npm run inst`（内部为更安全的 `npm ci --ignore-scripts`），不要用 `npm install` / `npm i` |

### 常用命令

```bash
git clone https://github.com/openfrontio/OpenFrontIO.git
cd OpenFrontIO
npm run inst
npm run dev          # 客户端 + 服务端开发模式（热重载）
npm run start:client
npm run start:server-dev
npm run dev:staging  # 连预发 API
npm run dev:prod     # 连生产 API（回放等场景）
npm test
```

贡献与翻译见仓库 `CONTRIBUTING.md`。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/openfrontio/OpenFrontIO |
| **官网 / 网页版** | https://openfront.io/ |
| **Steam 商店页** | https://store.steampowered.com/app/3560670/OpenFront/ |
| **前身 WarFront** | https://github.com/WarFrontIO |
| **Crowdin 本地化** | https://crowdin.com/project/openfront-mls |
| **Issues** | https://github.com/openfrontio/OpenFrontIO/issues |
| **源头报道（头条）** | https://www.toutiao.com/article/7688611606078423552/ |

## My takeaways

1. **开源地址已确认**：官方主仓为 `openfrontio/OpenFrontIO`，不是商业闭源客户端。
2. **产品形态值得记**：网页免费获客 → Steam 付费/增值；同一玩家池，降低「下载门槛」对传播的杀伤。
3. **玩法定位**：轻量化领土 SLG + 强 PVP/外交，而非传统 RTS 微操；与 Territorial.io / WarFront 一脉。
4. **协议注意**：AGPL-3.0 + 资源 CC BY-SA，若借鉴代码或对外提供网络服务需合规。
5. **与 unicore 的关联**：unicore 是 Unity 框架；本项目主要作「网页多人策略 / 开源游戏社区 / 轻量 SLG 设计」参考，不宜直接当 Unity 依赖引入。

## Related

- （可后续补充：Territorial.io、WarFront、浏览器策略游戏选型导航）