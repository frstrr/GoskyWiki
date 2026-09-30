---
id: source-20260831-abilitykit
title: AbilityKit - 模块化游戏战斗工具集
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/游戏开发
  - 主题/Unity
  - 主题/技能系统
  - 主题/战斗框架
author: HOBOBO
source_type: repo
source_url: https://github.com/HOBOBO/AbilityKit
source_author: HOBOBO
source_date: 2026-08-31
summary: 面向中大型战斗项目的可组合游戏战斗工具集，纯 C# Runtime、逻辑与表现分离，提供技能管线、事件触发、战斗原子能力、帧同步/状态同步、回放与网络等模块化机制。Unity UPM + .NET 双栈，可按需裁剪组合。
related: []
---

# AbilityKit - 模块化游戏战斗工具集

## Source summary

**Ability-Kit** 是一个面向复杂战斗项目的可组合工具集，以 Unity UPM Package 组织源码，同时提供大量 .NET 工程用于脱离 Unity 的编译、测试、Console 宿主和 Orleans 服务端接入。

核心定位：**不是**替项目预制一套固定 MOBA/ARPG/Shooter 应用层，而是提供技能编排、规则触发、战斗原子能力、逻辑世界、同步、回放、网络和表现解耦等可复用机制。游戏规则、房间流程、账号接入、技能配置规范仍由具体项目决定。

- ~220 Stars
- MIT 协议开源
- 作者：HOBOBO
- 当前状态：**开发期**（API、目录结构和依赖仍在收敛）

## 核心特性

| 特性 | 说明 |
|------|------|
| **逻辑与表现分离** | 纯 C# 逻辑层可在服务器、客户端、编辑器环境下运行，通过事件与表现层解耦 |
| **同步与恢复机制** | 帧同步、快照、状态同步、预测校正、记录回放和恢复相关组件 |
| **数据驱动链路** | Trigger Plan、Action Schema、Timeline 与项目配置工具把规则落到强类型运行时 |
| **高度可扩展** | 模块化设计，支持 Hook/Feature/Blueprint 扩展机制，按需裁剪 |
| **性能基础设施** | 索引、对象池、空间分桶、批处理和低分配 API |

## 适用边界

### 更适合

- MOBA、ARPG、MMO、RTS、多人动作、带复杂技能的 Shooter
- 需要服务端/客户端复用纯 C# 战斗逻辑
- 需要长期维护大量配置化技能、Buff、触发器和投射物
- 需要战斗日志、回放、自动化测试、预测回滚或状态同步

### 不建议优先使用

- 技能数量少、生命周期短、同步要求低的小型项目
- 所有逻辑都可以安全写在 Unity 场景脚本里的项目
- 以一次性硬编码交付为主、缺少配置和测试治理的项目
- 不需要解释战斗来源、也不需要网络同步的项目

## 架构概览

```text
可读配置 / Trigger Plan
  -> 强类型 Action Schema
  -> ExecCtx / World Service 上下文解析
  -> 技能、投射物、召唤等参数 Modifier
  -> 技能 Runtime / Origin / Trace 血缘
  -> 投射物、区域、Buff、Continuous 生命周期
  -> 业务事件处理与 Snapshot 输出
  -> 纯 C# 验收、回放、同步或 Unity 表现消费
```

### 三层责任边界

| 层次 | 稳定内容 | 不应由这一层决定 |
|------|----------|------------------|
| 框架机制层 | Phase/Trigger/Action 契约，World/Host 生命周期，ECS 适配，同步/快照/记录/网络基础设施 | 英雄规则、房间阶段、账号登录、UI 流程、具体配置表结构 |
| 项目应用层 | 组合根、会话、配置发布、权威模型、系统顺序、失败补偿、表现投影 | 把本项目策略包装成所有游戏必须使用的框架默认 |
| 示例宿主层 | MOBA、Shooter、Console、ET、Unity Starter 和 Orleans 参考实现 | 证明另一种宿主、同步模式或项目规则也已自动适用 |

## 核心模块

### 技能与战斗层

| 模块 | 说明 |
|------|------|
| `com.abilitykit.pipeline` | 技能流程编排：Phase 图模型，Sequence/Parallel/Conditional/Repeat/Delay/WaitUntil/Timeline |
| `com.abilitykit.triggering` | 事件触发与规则执行引擎：EventBus、TriggerRunner、TriggerPlan、强类型 Action Schema |
| `com.abilitykit.ability` | 技能聚合运行时：Ability、Effect、Triggering、配置加载、热重载 |
| `com.abilitykit.combat.*` | 伤害、投射物、移动、碰撞、导航、目标选择等战斗原子能力 |
| `com.abilitykit.continuous` | 持续效果运行时：条件驱动的激活、阻止、暂停、恢复与移除 |
| `com.abilitykit.behavior` | 行为运行时，可嵌入 Pipeline 行为阶段 |

### 世界管理与同步

| 模块 | 说明 |
|------|------|
| `com.abilitykit.world.framesync` | 帧同步：FrameSync、Rollback、ClientPrediction、输入历史 |
| `com.abilitykit.world.statesync` | 状态同步与客户端预测：Rollback、StateHash |
| `com.abilitykit.world.snapshot` | 快照路由：按 opCode 解码并分发到处理器 |
| `com.abilitykit.record` | 录像回放：Session、Container、Track，支持输入录制、状态哈希采样 |
| `com.abilitykit.world.ecs` | 轻量级 ECS 框架 |

### 流程与状态机

| 模块 | 说明 |
|------|------|
| `com.abilitykit.flow` | 流程编排引擎：IFlowNode 节点树，WAKE/PUMP 事件驱动 |
| `com.abilitykit.hfsm` | 分层状态机：基于 UnityHFSM，ITriggerable 事件转换 |

### 基础设施

| 模块 | 说明 |
|------|------|
| `com.abilitykit.core` | 数学库、对象池、日志、事件系统、序列化 |
| `com.abilitykit.gameplaytags` | Gameplay Tag 状态标识系统 |
| `com.abilitykit.modifiers` | 通用参数/属性修正器（Buff、装备、天赋动态改写参数） |
| `com.abilitykit.trace` | 溯源树运行时，追踪技能/效果/Action 来源上下文 |
| `com.abilitykit.diagnostics` | 开发期诊断与性能分析工具 |

## 按需组合建议

| 目标 | 起始组合 | 项目仍需实现 |
|------|----------|--------------|
| 技能流程与规则 | `core`、`pipeline`、`triggering`，按需加入 `ability`、`actionschema` | 技能输入、配置发布、Action 服务和玩法生命周期 |
| 战斗原子能力 | Targeting、Damage、Projectile、Motion、Collision、Navigation 等 `combat.*` 包 | 实体存储、World 服务注册、系统顺序和表现反馈 |
| 逻辑世界与宿主 | `world.di`、所选 ECS、`host`/`host.extension` | World 创建策略、模块组合、Tick 与 teardown owner |
| 联机同步 | FrameSync/Snapshot/StateSync/Record 与所需 `network.*` 包 | 权威模型、Room 能力、连接恢复、协议版本和场景验收 |

## 示例与宿主

| 示例/宿主 | 主要展示能力 |
|-----------|--------------|
| `demo.moba.*` | 技能输入、Pipeline、Trigger Plan、Buff/Continuous、投射物、区域、伤害、BT AI、表现 Cue |
| `demo.shooter.*` | 权威插值、预测校正、快照投影、可靠事件、重连恢复、Svelto ECS |
| MOBA Console | 纯 .NET 进程中组合 World、Host、同步适配、输入、表现投影和回放 |
| ET Demo | 把 ET Scene、Component/System 接到 MOBA runtime |
| Orleans Server | Gateway、Room、Battle Host、协议路由、状态存储边界 |

## 环境要求

- Unity `2022.3.62f1`（Package 基线 Unity 2022.3）
- .NET SDK `10.0.300`（根目录 `global.json` 固定）
- Windows/PowerShell 是仓库现有构建脚本覆盖最完整的开发环境

## 快速体验

```powershell
# Console Demo（纯 C# 验证）
dotnet run --project src/AbilityKit.Demo.Moba.Console/AbilityKit.Demo.Moba.Console.csproj
```

Unity 入口：
- `Unity/Assets/Scenes/StarterScene.unity`：统一 Starter，可选 MOBA/Shooter 与 Local/Multiplayer Profile
- `Unity/Packages/com.abilitykit.demo.moba.view.runtime/Scenes/MobaDemoGameplayScene.unity`
- `Unity/Packages/com.abilitykit.demo.shooter.view.runtime/Scenes/ShooterDemoGameplayScene.unity`

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/HOBOBO/AbilityKit |
| **设计文档总索引** | https://github.com/HOBOBO/AbilityKit/blob/master/Docs/design/00-index.md |
| **玩法能力地图** | https://github.com/HOBOBO/AbilityKit/blob/master/Docs/design/08-GameplayModules/00-GameplayCapabilityMap.md |
| **同步能力地图** | https://github.com/HOBOBO/AbilityKit/blob/master/Docs/design/07-NetworkSynchronization/00-SynchronizationCapabilityMap.md |
| **MOBA 参考实现** | https://github.com/HOBOBO/AbilityKit/blob/master/Docs/design/09-ImplementationExamples/MOBA/00-Overview.md |
| **技术选型文档** | https://github.com/HOBOBO/AbilityKit/blob/master/Unity/Packages/技术选型文档.md |
| **Issues** | https://github.com/HOBOBO/AbilityKit/issues |

## My takeaways

1. **定位清晰**：工具集而非应用模板，按需裁剪 `com.abilitykit.*` 包，不要全量引入。
2. **Pipeline + Triggering 是核心主线**：技能流程编排与事件触发规则执行是框架最高价值点，适合复杂技能/Buff/被动联动场景。
3. **纯 C# 双栈设计**：同一套源码可在 Unity、Console、Orleans 服务端运行，适合需要逻辑复用和自动化测试的中大型项目。
4. **Trace 血缘追踪**：技能释放产生的投射物、区域、Buff 和后续伤害保留 root/parent/owner context，对战斗调试和回放很有价值。
5. **仍在开发期**：接入前需评估 API 稳定性，以设计文档和当前源码为准，不要假设所有模块都已生产就绪。
6. **与 unicore 的关联**：若 unicore 项目需要技能系统、Buff、投射物、多人同步等能力，可作为开源参考或按需引入相关模块。

## Related

- （待补充：若后续建立 `30 知识资源/游戏开发/` 或 `40 知识导航/游戏开发` 主题页，可在此链接）
