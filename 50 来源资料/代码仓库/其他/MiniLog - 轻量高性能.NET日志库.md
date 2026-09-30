---
id: source-20260909-minilog
title: MiniLog - 轻量高性能 .NET 日志库
type: source
status: active
created: 2026-09-09
updated: 2026-09-09
tags:
  - 来源/代码仓库
  - 主题/日志
  - 主题/dotnet
  - 主题/高性能
author: netcasewqs
source_type: repo
source_url: https://gitee.com/netcasewqs/MiniLog
source_author: netcasewqs
source_date: 2026-09-09
summary: 轻量、高性能的 .NET 日志组件库（MIT，零外部依赖）。保留 Logger→Appender→Layout 派发模型，底层用结构体日志条目、零分配渲染与源生成器；目标 .NET 8/10，支持 XML/JSON/INI/Fluent 配置、8 种 Appender、MEL 兼容，NuGet 包名 MiniLog。
related:
  - "[[ZLogger - Cysharp零分配.NET与Unity日志库]]"
---

# MiniLog - 轻量高性能 .NET 日志库

## Source summary

**MiniLog** 是面向 **.NET 8.0 / .NET 10.0** 的轻量高性能日志框架，定位为「现代、低 GC 压力、可替代 MS Logging」的组件库。

- 仓库：https://gitee.com/netcasewqs/MiniLog（Gitee）
- 许可证：**MIT**
- 版本：**1.0.0（GA）**
- 依赖：**零外部依赖**（纯 .NET）
- NuGet：`MiniLog`（核心）、`MiniLog.Extensions.Logging`（MEL 适配）
- 设计借鉴：log4net / NLog 的 **Logger → Appender → Layout** 派发模型
- 底层优化：结构体日志条目、零分配渲染、源生成器（零装箱派发）

## 核心亮点

| 亮点 | 说明 |
|------|------|
| **零分配渲染** | `LogEntry` 值语义 + `ReusableDelegateWriter` + `ValueStringBuilder`（栈缓冲 / ArrayPool），热路径零 GC |
| **零装箱派发** | 源生成器经 `ILogArgumentBag` 泛型参数袋贯穿派发链 |
| **源生成器双后端** | `[Log]` 注入 `ILog` 字段 + `[LoggerMessage]` 强类型方法；一份声明通吃 MiniLog 与 Microsoft.Extensions.Logging |
| **多格式配置** | XML / JSON / INI + Fluent Builder；XML 带 XSD、JSON 带 Schema |
| **多输出目标** | Console / Debug / Trace / File / AsyncFile / Udp / AdoNet / None 共 8 种 Appender |
| **文件滚动** | 11 种文件滚动 × 7 种目录滚动，支持过期清理 |
| **过滤器链** | 8 种 Filter，三态 Deny / Neutral / Accept |
| **MEL 兼容** | `AddMiniLog()` 可作为 MS Logging Provider 接入 |
| **编译期校验** | Roslyn 分析器校验布局 `%token`（LLG0002–0006 等） |

## 快速开始

```bash
dotnet add package MiniLog
# 可选：接入 ASP.NET Core / MEL
dotnet add package MiniLog.Extensions.Logging
```

```csharp
using MiniLog;

LogManager.Initialize();
var log = LogManager.GetLogger<Program>();
log.Info("应用启动");
LogManager.Shutdown();
```

源生成器推荐写法：

```csharp
[Log]
public partial class OrderService
{
    [LoggerMessage(LogLevel.Info, "处理订单 {Id} 金额 {Amount}")]
    public partial void LogOrderProcessed(int id, decimal amount);
}
```

## Appenders 速览

| 类型 | 说明 |
|------|------|
| Console | 可选 ANSI 彩色分级（默认关） |
| Debug / Trace | 调试与 Trace 监听器 |
| File | 同步文件追加 |
| AsyncFile | Channels 异步批写，高并发优先 |
| Udp | 远程 UDP 文本 |
| AdoNet | 参数化 SQL 写库，支持异步批量 |
| None | 丢弃，便于基准 |

## 性能定位（基准快照，.NET 8）

相对主流日志库，MiniLog 在「固定消息 / 异步文件 / 级别关闭」场景下耗时与分配更优（官方 BenchmarkDotNet）：

- 固定消息：MiniLog 约 **26.8µs / 0B**（同条件显著快于 MS / Serilog / NLog / log4net）
- 异步文件：约 **98µs**
- 源生成级别关闭：约 **7.5µs / 0B**

选型提示：

- 要**极致性能与低分配** → MiniLog
- 要**最丰富 Sink/生态** → Serilog
- 要**官方标准抽象** → MS.Extensions.Logging（可用 MiniLog 作 Provider）
- 要**传统成熟生态** → NLog / log4net

## 适用场景

- .NET 8/10 服务、工具、游戏服务端等需要**低 GC 压力**的日志
- 希望保留 log4net/NLog 式配置与 Appender 模型，但要现代性能
- 已用 MEL，想换高性能 Provider（`MiniLog.Extensions.Logging`）
- 需要编译期布局校验与 `[LoggerMessage]` 强类型日志

## 链接

| 资源 | 链接 |
|------|------|
| **Gitee 仓库** | https://gitee.com/netcasewqs/MiniLog |
| **Git Clone (HTTPS)** | https://gitee.com/netcasewqs/MiniLog.git |
| **NuGet（核心）** | https://www.nuget.org/packages/MiniLog |
| **NuGet（MEL）** | https://www.nuget.org/packages/MiniLog.Extensions.Logging |
| **使用手册** | https://gitee.com/netcasewqs/MiniLog/blob/master/docs/MiniLog使用手册.md |
| **配置参考** | https://gitee.com/netcasewqs/MiniLog/blob/master/CONFIGURATION.md |
| **MEL 集成指南** | https://gitee.com/netcasewqs/MiniLog/blob/master/docs/MEL集成指南.md |
| **基准报告** | https://gitee.com/netcasewqs/MiniLog/blob/master/BENCHMARK_REPORT.md |
| **诊断码总表** | https://gitee.com/netcasewqs/MiniLog/blob/master/docs/DIAGNOSTICS.md |
| **变更日志** | https://gitee.com/netcasewqs/MiniLog/blob/master/CHANGELOG.md |
| **许可证** | MIT |

## My takeaways

1. **国产 Gitee 上的正式 GA 日志库**：零依赖 + MIT，可直接 NuGet 引用，适合作为 .NET 高性能日志候选。
2. **模型熟悉、底层激进**：对外像 log4net/NLog，对内用结构体 + 源生成器打零分配，对 Unity 服务端 / 高吞吐服务有参考价值。
3. **生态仍偏早期**：Appender 数量与社区体量不及 NLog/Serilog，选型时要接受「性能优先、插件生态次之」。
4. 与 unicore（Unity 框架）无直接耦合，但是独立的 **.NET 日志基础设施** 资源，需要时从本页跳转即可。

## Related

- [[ZLogger - Cysharp零分配.NET与Unity日志库]]
