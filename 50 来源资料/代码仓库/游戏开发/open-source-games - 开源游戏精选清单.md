---
id: source-20261103-open-source-games
title: open-source-games - 开源游戏精选清单
type: source
status: active
created: 2026-11-03
updated: 2026-11-03
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/开源游戏
  - 主题/awesome-list
  - 主题/游戏清单
author: bobeff
source_type: repo
source_url: https://github.com/bobeff/open-source-games
source_author: bobeff
source_date: 2026-11-03
summary: bobeff 维护的开源游戏与商业游戏开源重实现 Awesome List。按动作、经营、RTS、Roguelike 等十余类索引；条目常同时给出官网与源码。约 17k Stars，CC0-1.0。适合找可玩开源游戏、学习引擎重实现，以及带着具体问题读源码。
related:
  - "[[40 知识导航/游戏开发|游戏开发]]"
  - "[[50 来源资料/代码仓库/游戏开发/Mindustry - 自动化塔防RTS开源游戏|Mindustry - 自动化塔防RTS开源游戏]]"
---

# open-source-games - 开源游戏精选清单

## Source summary

**open-source-games**（仓库 [bobeff/open-source-games](https://github.com/bobeff/open-source-games)）是一份按玩法分类的开源游戏目录，同时收录「完整开源游戏」与「商业游戏的开源重实现 / 移植 / 逆向工程」项目。作者 **bobeff** 维护；仓库本身是 Markdown 清单，不是可运行的游戏引擎。

- ~**17,393** Stars / ~**1,565** Forks（2026-11-03 GitHub API）
- 协议：**CC0-1.0**（清单内容可自由复用）
- 语言：清单为主；仓库检测语言标记为 **Python**（辅助脚本）
- Topics：`awesome-list` / `games` / `open-source`
- 默认分支：`main`
- 创建于：2021-09-15

## 来源线索

整理自头条文章《GitHub 上这份游戏清单，让我又想折腾电脑了》（gid `7700285412165616138`，媒体「执游乐享欢愉」）：

- 定位：周末入口——今晚想找点什么玩，或想知道一款游戏究竟怎么做出来
- 使用建议：先按目录选一种玩法；安装说明去各项目官网 / README，清单上的一句介绍不能替代系统要求与素材要求
- 重要提醒：清单里混有引擎重实现、源码移植、逆向工程；以及运行工具（如 ScummVM）。「有源码链接」≠「下载 ZIP 就能完整免费玩」
- 文章推荐的 5 个入门方向：OpenTTD、SuperTuxKart、Mindustry、Endless Sky、OpenRCT2

## 核心特性

| 特性 | 说明 |
|------|------|
| **按玩法分类** | 动作、冒险、经营/大亨、城建、FPS、平台、解谜、赛车、即时战略、Roguelike、RPG、沙盒、射击、体育、第三人称、塔防、回合制等 |
| **双入口** | 多数条目同时给出项目主页与 `[[source]]` 源码链接 |
| **含重实现** | 收录 OpenRCT2、OpenTTD、OpenXcom、VCMI、fheroes2 等「开源重实现 / 引擎替代」 |
| **相关清单外链** | Other lists 指向 awesome-game-remakes、osgameclones、Libre Game Wiki、Wikipedia 开源游戏列表等 |
| **清单协议宽松** | CC0-1.0，便于二次整理到个人知识库 |

## 分类速览（目录）

- Action / Adventure / Business and Tycoon / City-Building
- First-Person / Platformers / Puzzle / Racing
- Real-Time strategies / Roguelikes / Role-Playing / Sandbox
- Shoot 'em up / Sport / Third-Person / Tower Defence / Turn-Based strategies
- Other lists（外部开源游戏清单集合）

## 文章推荐的 5 个入口

| 项目 | 定位 | 入口 |
|------|------|------|
| **OpenTTD** | 《运输大亨豪华版》开源模拟经营；修路铺轨建运输网 | https://www.openttd.org · [源码](https://github.com/OpenTTD/OpenTTD) |
| **SuperTuxKart** | 开源卡丁车；适合先玩再读源码 | 官网安装包 · 清单 Racing 分类 |
| **Mindustry** | 自动化塔防即时战略；Java；有构建说明 | [[50 来源资料/代码仓库/游戏开发/Mindustry - 自动化塔防RTS开源游戏\|Mindustry]] · https://github.com/Anuken/Mindustry |
| **Endless Sky** | 二维太空贸易与战斗；Win/macOS/Linux 免费开源 | https://endless-sky.github.io · [源码](https://github.com/endless-sky/endless-sky) |
| **OpenRCT2** | 《过山车大亨 2》开源重实现；**运行需原版 RCT2 游戏数据** | https://openrct2.org · [安装文档](https://docs.openrct2.io/) · [源码](https://github.com/OpenRCT2/OpenRCT2) |

## 适用边界

### 更适合

- 按玩法发现可玩 / 可学的开源游戏项目
- 对比「完整开源游戏」与「需原版素材的引擎重实现」
- 带着具体问题（寻路、存档、关卡编辑）选一款反复玩的游戏去读源码
- 为自己的 awesome / 知识库做二次索引（协议 CC0）

### 使用时要注意

- **不是发行平台**：清单不负责安装包、系统要求与素材授权
- **重实现常需原版数据**：如 OpenRCT2、部分 Build 引擎移植、经典游戏源码移植
- **工具 ≠ 游戏**：ScummVM 等是运行器，本身不附送所支持的商业游戏
- **GitHub Download ZIP** 多半是源码；只想玩应优先官网 Download 或 Releases
- **许可要分开看**：各游戏代码 / 美术 / 音乐许可可能不同，清单 CC0 不覆盖被收录项目

## 相关外部清单（Other lists）

| 资源 | 链接 |
|------|------|
| Awesome Game Remakes | https://github.com/radek-sprta/awesome-game-remakes |
| Awesome Open Source Games | https://github.com/michelpereira/awesome-open-source-games |
| Games on GitHub | https://github.com/leereilly/games |
| Libre Game Wiki | https://libregamewiki.org |
| List of Open Source Games | https://trilarion.github.io/opensourcegames |
| Open Source Game Clones | https://osgameclones.com |
| Wikipedia: List of open-source video games | https://en.wikipedia.org/wiki/List_of_open-source_video_games |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/bobeff/open-source-games |
| **作者** | https://github.com/bobeff |
| **Issues** | https://github.com/bobeff/open-source-games/issues |
| **源头报道（头条）** | https://www.toutiao.com/article/7700285412165616138/ |

## My takeaways

1. **开源地址已确认**：主仓 `bobeff/open-source-games`，Awesome List，约 17k Stars，CC0-1.0。
2. **定位是地图不是商店**：按玩法发现项目；下载与素材要求必须回各项目主页核实。
3. **三类对象要分清**：完整可玩开源游戏 / 需原版数据的重实现或移植 / 仅运行工具。
4. **学代码的用法**：先选一款愿意反复玩的，带着具体问题读实现，比漫无目的 clone 更有效。
5. **与本库已有笔记**：清单中的 Mindustry 已单独沉淀；OpenTTD / SuperTuxKart / Endless Sky / OpenRCT2 可按需再拆笔记。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[50 来源资料/代码仓库/游戏开发/Mindustry - 自动化塔防RTS开源游戏|Mindustry - 自动化塔防RTS开源游戏]]
- [[50 来源资料/代码仓库/游戏开发/OpenFrontIO - 开源浏览器RTS游戏|OpenFrontIO - 开源浏览器RTS游戏]]