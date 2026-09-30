---
id: source-20260924-spacetimedb
title: SpacetimeDB - 数据库即服务器的多人游戏后端
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/多人游戏
  - 主题/游戏后端
  - 主题/实时同步
  - 主题/Unity
  - 主题/数据库
author: Clockwork Labs
source_type: repo
source_url: https://github.com/clockworklabs/SpacetimeDB
source_author: clockworklabs
source_date: 2026-09-24
summary: Clockwork Labs 开源的「关系型数据库 + 服务器」一体后端。业务逻辑以 module（Tables + Reducers）上传进库，客户端直连并自动实时同步状态，省掉传统游戏服务器中间层。支持 Rust/C#/TS/C++ 写模块，C#（Unity）与 C++（Unreal）客户端 SDK；BitCraft Online 整后端即单模块验证。BSL 1.1。
related: []
---

# SpacetimeDB - 数据库即服务器的多人游戏后端

## Source summary

**SpacetimeDB**（[spacetimedb.com](https://spacetimedb.com)）是 Clockwork Labs 开源的实时后端：关系型数据库本身就是服务器。你把 schema 与业务逻辑写成 **module**，编译后跑在数据库内；客户端直连数据库调用逻辑并订阅表，状态变化时由库主动推送增量——传统「客户端 → 游戏服务器 → 数据库」里的中间服务器层可以被删掉。

- GitHub：`clockworklabs/SpacetimeDB`（约 **25.2k Stars**）
- 官网 / 文档：[spacetimedb.com](https://spacetimedb.com) · [docs](https://spacetimedb.com/docs)
- 托管：官方 Maincloud（BSL 下云厂商不可自行提供竞品托管）
- 生产背书：自家 MMORPG [BitCraft Online](https://bitcraftonline.com) 整后端为单个 SpacetimeDB 模块（聊天、物品、地形、玩家位置等），实时同步给数千玩家
- 协议：**BSL 1.1**（数年后续转为 AGPL v3 + linking exception）；linking exception 意味着**你的游戏不必开源**，只需回馈对 SpacetimeDB 本身的修改

## 来源线索

整理自头条文章《做多人游戏，最贵的不是美术是后端！这个开源项目直接把服务器干掉了，25.2k星》（gid `7685935045168677386`）：

- 痛点：独立开发者多人游戏卡在游戏服、与库同步、鉴权、运维（全栈 + DevOps）
- 思路：服务器层大量工作是收请求、改库、推状态 → 让逻辑直接长在数据库里
- 架构对比：`客户端 → 游戏服 → 数据库` 变为 `客户端 → SpacetimeDB（逻辑 + 状态）`
- 适合状态驱动玩法（MMO / SLG / 模拟经营 / 休闲多人）；重物理 / 帧同步场景仍可能需要专门游戏服

## 核心概念

| 概念 | 说明 |
|------|------|
| **Module** | 上传进库的应用单元；内含 Tables 与 Reducers |
| **Tables** | 数据定义（玩家、物品、聊天消息等）；`public` 表可被客户端订阅 |
| **Reducers** | 业务逻辑入口，类比传统 API；带完整 **ACID** 事务 |
| **Subscriptions** | 客户端订阅表后自动收增量，无轮询、无需自写 WebSocket 推送 |
| **内存 + commit log** | 状态常驻内存保吞吐，磁盘 commit log 保持久与崩溃恢复 |
| **鉴权** | 权限写在 module 内，能力上等价于传统服务器层 |

## 语言与引擎支持

### 服务端 Module

| 语言 | 说明 |
|------|------|
| Rust | 官方示例与文档齐全 |
| C# | 与 Unity 生态衔接好 |
| TypeScript | Web / Node 全家桶 |
| C++ | 模块与虚幻侧能力 |

### 客户端 SDK

| SDK | 典型场景 |
|-----|----------|
| C# | 独立应用、**Unity** |
| C++ | **Unreal Engine** |
| TypeScript | React / Next / Vue / Node 等 |
| Rust | 通用客户端 |

官方另有聊天 / Unity 多人 / Unreal 多人教程，以及 Claude Code / Codex 等 Agent 配置指南。

## 适用边界

### 更适合

- 需要实时状态同步的 MMO、SLG、模拟经营、休闲多人
- 想降低「自建游戏服 + 同步 + 运维」成本的独立团队
- 已用 Unity / Unreal / Web，希望模块语言与引擎同栈
- 需要 ACID 事务的交易、物品、存档类玩法

### 需谨慎 / 不太适合当银弹

- 逻辑极重计算（大规模物理、严格帧同步）——仍可能要专用游戏服
- 必须由第三方云厂商自托管竞品服务：BSL 明确限制，深度使用会更依赖官方 Maincloud
- 生态相对年轻，开放 Issue 多，复杂生产需自行踩坑与评估
- 商业上线前建议法务过一遍 BSL → AGPL 切换条款

## 快速开始

```bash
# macOS / Linux
curl -sSf https://install.spacetimedb.com | sh

# Windows (PowerShell)
iwr https://windows.spacetimedb.com -useb | iex

spacetime login
spacetime dev --template chat-react-ts
```

Docker 本地：

```bash
docker run --rm --pull always -p 3000:3000 clockworklabs/spacetime start
```

源码构建见仓库 README（`spacetimedb-standalone` / `cli` / `update`）。

## 最小示例（概念）

服务端定义表 + reducer：

```rust
#[spacetimedb::table(accessor = messages, public)]
pub struct Message {
    #[primary_key]
    #[auto_inc]
    id: u64,
    sender: Identity,
    text: String,
}

#[spacetimedb::reducer]
pub fn send_message(ctx: &ReducerContext, text: String) {
    ctx.db.messages().insert(Message {
        id: 0,
        sender: ctx.sender,
        text,
    });
}
```

客户端订阅（TS）：

```typescript
const [messages] = useTable(tables.message);
// 服务端状态变化时自动更新，无轮询
```

## 相关链接

| 类型 | URL |
|------|-----|
| 仓库 | https://github.com/clockworklabs/SpacetimeDB |
| 官网 | https://spacetimedb.com |
| 文档 | https://spacetimedb.com/docs |
| 定价 / Maincloud | https://spacetimedb.com/pricing |
| BitCraft | https://bitcraftonline.com |
| 示例 Blackholio（Unity 等） | 仓库内 `demo/Blackholio`；独立仓库曾归档 |
| 来源文章（头条） | https://www.toutiao.com/article/7685935045168677386/ |

## 检索关键词

`SpacetimeDB` · `Clockwork Labs` · `BitCraft` · `Reducer` · `数据库即服务器` · `Unity 多人` · `Unreal 多人` · `实时同步` · `BSL`