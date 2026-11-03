---
id: source-20261103-gdevelop-examples
title: GDevelop-examples - GDevelop开源示例与主题槽位模板库
type: source
status: active
created: 2026-11-03
updated: 2026-11-03
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/开源资源
  - 主题/无代码
  - 主题/GDevelop
  - 主题/3D
  - 主题/AI生成
  - 主题/示例模板
author: GDevelopApp
source_type: repo
source_url: https://github.com/GDevelopApp/GDevelop-examples
source_author: GDevelopApp
source_date: 2026-11-03
summary: GDevelop 官方免费示例与起步模板库。社区 PR 合并后自动部署；3D 起步模板用「主题槽位」解耦美术与关卡，主题只填槽、模板只留槽，便于 AI 生成场景后一键换整套美术。
related:
  - "[[40 知识导航/游戏开发|游戏开发]]"
  - "[[50 来源资料/代码仓库/游戏开发/open-source-games - 开源游戏精选清单|open-source-games]]"
discovery_source: https://ainewsdetector.lukezystudio.cn/?from=app&v=2#item/b7993f28-58a3-4814-b6de-3f6f62494f1e
---

# GDevelop-examples - GDevelop开源示例与主题槽位模板库

## Source summary

**GDevelop-examples**（[GDevelopApp/GDevelop-examples](https://github.com/GDevelopApp/GDevelop-examples)）是开源无代码引擎 **GDevelop** 的官方**免费示例 / 起步模板**仓库：示例可在编辑器内搜索下载；社区提交 PR，合并进 `main` 后自动部署到主站。

核心一句话：**模板只管玩法与槽位，主题只管按槽填美术；两边互不引用，换主题 = 换整套占位资产。**

- 约 **181 stars / 35 forks**（2026-11-03 GitHub API）
- 语言：**JavaScript**（示例工程与生成脚本）；许可证：仓库未标 SPDX（以仓库与示例内说明为准）
- 默认分支：`main`；主页：https://gdevelop.io
- Topics：game-development、game-engine、gdevelop、open-source、hacktoberfest
- 配套引擎主仓：[4ian/GDevelop](https://github.com/4ian/GDevelop)（约 28k+ stars，跨平台 2D/3D/多人）
- 发现来源：GameAI 资讯条 [item/b7993f28…](https://ainewsdetector.lukezystudio.cn/?from=app&v=2#item/b7993f28-58a3-4814-b6de-3f6f62494f1e)

解决的痛点：无代码 / AI 出关卡相对容易，但**成套且一致的美术**难；槽位把「长什么样」从模板里剥出去，交给主题复用。

## 核心特性

| 特性 | 说明 |
|------|------|
| **官方示例库** | 免费示例工程；编辑器内可搜可下；进首页须在 `scripts/generate-database.js` 登记 |
| **社区贡献流** | Issue 附 zip 或直接 PR；质量门槛较高，需按提交指南打磨 |
| **自动部署** | 推到 `main` 后 CircleCI 部署示例与数据库 |
| **主题槽位解耦** | 根目录与各 3D starter 的 `theme-slots.json`：角色 `character.player`、地面 `env.ground` 等 |
| **主题副本** | 构建时为每个 3D starter 生成 `*.theme-*.json` 换皮副本；AI 选主题时直接打开已换皮工程 |
| **Staging 联调** | 非 main 分支 `--staging` 部署，可与 assets / AI prompts 仓库联调未合并主题 |
| **Gameplay tests** | 工程内 `tests` 数组；CI 用最新 Linux GDevelop 跑按键/逐帧脚本验收 |

## 主题槽位（重点机制）

设计原则：**主题与模板互不引用，只共同指向槽位表。**

- 根目录 `theme-slots.json`：全库共享的槽位词汇表
- `examples/<starter>/theme-slots.json`：该起步模板里「哪个对象扮演哪个槽」
- 对象引用形如：`scene:<场景>/<对象>`、`global/<对象>`、`object:<扩展>::<自定义对象>/<子对象>`
- 天空盒：`effect:<场景>/<层>/<效果>` → 一个天空盒槽（主题六面图替换）
- 3D 立方体：可整体映射 1 个贴图槽，或六个面各占一槽
- 自定义对象（如 3D 地形）：贴图 → texture 槽；模型 → model 槽；颜色 → color 槽；地形层贴图+法线 → material 槽
- **新增 3D starter 必须带 `theme-slots.json`**；未映射且未进 `ignoredObjects` 的 3D 模型/方块会让构建失败并点名对象
- 能复用已有槽就复用（新 starter 主角仍用 `character.player`）；只有真正新角色才往根表加槽（未填主题前保留占位美术）

## 适用边界

### 更适合

- 用 GDevelop 做 2D/3D/多人，需要官方起步模板与可换皮主题
- 关注 **AI 生成游戏**：模型出关卡后，靠主题槽位快速对齐用户设定的美术风格
- 想给社区贡献示例，或研究「模板 / 主题 / 资产仓」三仓协作与 CI 换皮流水线
- 查阅无代码引擎如何用事件表 + 可复用行为拼逻辑

### 需要注意

- 这是 **示例与模板仓**，不是引擎本体；做游戏请下 [GDevelop](https://gdevelop.io)，改引擎看 [4ian/GDevelop](https://github.com/4ian/GDevelop)
- 「高质量示例」依赖人工评审，合并节奏与审稿人力绑定
- 槽位命名一旦被社区主题沿用就近似公共接口，改名成本高
- 非标准玩法可能槽位覆盖不全；无合适槽应进 `ignoredObjects`，勿硬套错语义槽
- Stars 不高（示例仓常态）；热度与价值主要看引擎生态与模板可用性

## 快速开始

**玩示例 / 做游戏**

1. 打开 https://gdevelop.io 下载 GDevelop
2. 在编辑器中搜索并下载示例（无需先 clone 本仓）

**贡献示例（摘要）**

1. 按 [提交示例指南](http://wiki.compilgames.net/doku.php/gdevelop5/community/guide-for-submitting-an-example) 达标
2. 根目录准备 `preview.png`（16:9）、可选 `thumbnail.png` / `square-icon.png`
3. 写简短 `README.md`，导出工程 zip
4. 开 Issue 附 zip，或 clone 后放入 `examples/` 再提 PR

**本地相关脚本（开发者）**

```bash
npm install
# 跑全部含 gameplay tests 的工程
node scripts/run-gameplay-tests.js
# 只跑相对 main 有改动的
node scripts/run-gameplay-tests.js --only-changed
# 指定工程
node scripts/run-gameplay-tests.js --projects=examples/starting-platformer/starting-platformer.json
```

进首页示例须在 `scripts/generate-database.js` 登记。

## 链接

| 资源 | 链接 |
|------|------|
| **本仓库（示例库）** | https://github.com/GDevelopApp/GDevelop-examples |
| **引擎主仓 GDevelop** | https://github.com/4ian/GDevelop |
| **官网 / 下载** | https://gdevelop.io |
| **组织 GDevelopApp** | https://github.com/GDevelopApp |
| **提交示例指南** | http://wiki.compilgames.net/doku.php/gdevelop5/community/guide-for-submitting-an-example |
| **发现条目（GameAI）** | https://ainewsdetector.lukezystudio.cn/?from=app&v=2#item/b7993f28-58a3-4814-b6de-3f6f62494f1e |

## My takeaways

1. **定位**：GDevelop 官方示例与 3D 起步模板仓；真正「能玩能改」的入口在编辑器，不在 clone 本仓。
2. **槽位解耦**：主题与模板靠共享槽名对齐——这是对接 AI 出场景、再统一换皮的关键设计。
3. **工程启发**：根槽位表 + starter 映射 + CI 生成主题副本 + staging 三仓联调，是可抄的「内容工厂」流水线。
4. **风险**：槽名即接口；人工审稿决定示例质量上限；与引擎/assets/prompts 仓强耦合。
5. **检索关键词**：GDevelop、GDevelop-examples、theme-slots、无代码游戏引擎、3D starter、AI 换皮主题。

## Related

- [[40 知识导航/游戏开发|游戏开发]]
- [[50 来源资料/代码仓库/游戏开发/open-source-games - 开源游戏精选清单|open-source-games - 开源游戏精选清单]]
- [[50 来源资料/代码仓库/游戏开发/GameFactory-3A - 开源 3A 游戏生成 Skill 与资产框架|GameFactory-3A]]
- [[50 来源资料/代码仓库/游戏开发/threejs-game-skills - Three.js浏览器游戏Agent Skills合集|threejs-game-skills]]
