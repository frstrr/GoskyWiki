---
id: source-20260909-zlogger
title: ZLogger - Cysharp 零分配 .NET/Unity 日志库
type: source
status: active
created: 2026-09-09
updated: 2026-09-09
tags:
  - 来源/代码仓库
  - 主题/日志
  - 主题/dotnet
  - 主题/Unity
  - 主题/高性能
  - 主题/Cysharp
author: Cysharp
source_type: repo
source_url: https://github.com/Cysharp/ZLogger
source_author: Cysharp
source_date: 2026-09-09
summary: Cysharp 出品的零分配文本/结构化日志库（ZLogger）。直接构建在 Microsoft.Extensions.Logging 之上，用 C# 插值字符串与 Source Generator 从输入到输出全程 UTF8，避免 UTF16 桥接与装箱；支持 Console/File/RollingFile/Stream/InMemory/自定义 Processor，以及 PlainText/JSON/MessagePack。同时支持 .NET 与 Unity（2022.2+）。
related:
  - "[[MiniLog - 轻量高性能.NET日志库]]"
---

# ZLogger - Cysharp 零分配 .NET/Unity 日志库

## Source summary

**ZLogger**（Zero Allocation Text/Structured Logger）是 **Cysharp**（Cygames 旗下）开源的高性能日志库，面向 **.NET** 与 **Unity**。名字里的 Z 即 Zero Allocation。

核心思路：传统日志多基于 UTF16 字符串再编码到 UTF8，存在额外开销；ZLogger 借助 C# 10 插值字符串改进与 .NET 8 的 `IUtf8SpanFormattable`，从输入到输出尽量直接写 UTF8，并避免装箱。它**原生构建在** `Microsoft.Extensions.Logging` 上，省去「自家体系 ↔ MEL」的桥接损耗。

- 仓库：https://github.com/Cysharp/ZLogger
- 组织：Cysharp（https://github.com/Cysharp）
- 许可证：**MIT**
- 语言：C#
- Stars / Forks：约 **1768** / **128**（检索时）
- 最新发行版：**2.5.10**（2025-01-06）
- 默认分支：`master`
- 目标框架：`.NET Standard 2.0/2.1`、`.NET 6/7`、`.NET 8+`（最佳性能在 .NET 8+）
- NuGet：`ZLogger`

## 核心亮点

| 亮点 | 说明 |
|------|------|
| **零分配热路径** | 插值字符串 + UTF8 直出，避免典型 UTF16→UTF8 与装箱成本 |
| **原生 MEL** | 直接作为 `Microsoft.Extensions.Logging` Provider，无第三方桥 |
| **ZLog 插值 API** | `logger.ZLogInformation($"Hello {name}, {age}")` 写法自然且高效 |
| **结构化日志** | 深度整合 `Utf8JsonWriter`；支持 PlainText / JSON / MessagePack / 自定义 Formatter |
| **输出端齐全** | Console、File、RollingFile、Stream、InMemory、自定义 `LogProcessor`（可做 HTTP 批送等） |
| **Source Generator** | `[ZLoggerMessage]` 生成强类型、零分配日志方法 |
| **默认偏快** | 官方强调默认配置即追求高吞吐；对比许多库默认每条 flush 的慢路径 |
| **Unity 支持** | 提供 `ZLogger.Unity` 包与 `AddZLoggerUnityDebug()` |

## Providers 与格式

| 能力 | 说明 |
|------|------|
| Console | 云原生场景重点优化，文本/结构化皆可 |
| File / RollingFile | 按时间间隔或大小滚动；可自定义路径选择器 |
| Stream / InMemory | 任意 `Stream`；内存订阅 `MessageReceived` |
| LogProcessor | 自定义导出（如异步批处理发 HTTP） |
| PlainText / JSON / MessagePack | `UseJsonFormatter()` 等；可配 KeyNameMutator、IncludeProperties |
| Scope / Filter | 继承 MEL 的 Scope、Category 过滤、JSON 配置 LogLevel |

## 快速开始（.NET）

```bash
dotnet add package ZLogger
```

```csharp
using Microsoft.Extensions.Logging;
using ZLogger;

using var factory = LoggerFactory.Create(logging =>
{
    logging.SetMinimumLevel(LogLevel.Trace);
    logging.ClearProviders();
    logging.AddZLoggerConsole();
    // 结构化：logging.AddZLoggerConsole(o => o.UseJsonFormatter());
});

var logger = factory.CreateLogger("Program");
var name = "John";
var age = 33;
logger.ZLogInformation($"Hello my name is {name}, {age} years old.");
```

ASP.NET Core / Generic Host：

```csharp
builder.Logging.ClearProviders();
builder.Logging.AddZLoggerConsole();
// 文件滚动示例见仓库 README
```

Source Generator：

```csharp
public static partial class LogExtensions
{
    [ZLoggerMessage(LogLevel.Debug, "Hello, {name}")]
    public static partial void Hello(this ILogger logger, string name);
}
```

可用 `BannedApiAnalyzers` 禁止普通 `LoggerExtensions`/`Console`，引导团队统一走 `ZLog*`。

## Unity 接入要点

| 项 | 要求 |
|----|------|
| Unity 版本 | **2022.2+** 可用标准功能；**2022.3.12f1+** 可用 Source Generator（C# 11 preview） |
| 依赖工具 | [NuGetForUnity](https://github.com/GlitchEnzo/NuGetForUnity)、[CsprojModifier](https://github.com/Cysharp/CsprojModifier) |
| UPM 包 | `https://github.com/Cysharp/ZLogger.git?path=src/ZLogger.Unity/Assets/ZLogger.Unity` |
| 编译器 | `Assets`（或 asmdef 旁）放 `csc.rsp`：`-langVersion:10 -nullable`；IDE 侧用 LangVersion.props + CsprojModifier |
| NuGet | 经 NuGetForUnity 安装 `ZLogger` |

Unity 基本用法：

```csharp
var loggerFactory = LoggerFactory.Create(logging =>
{
    logging.SetMinimumLevel(LogLevel.Trace);
    logging.AddZLoggerUnityDebug(); // 输出到 Unity Console
});
var logger = loggerFactory.CreateLogger<YourClass>();
logger.ZLogInformation($"Hello, {name}!");
```

亦支持 File Provider + JSON 结构化输出。

## 适用场景

- .NET 8+ 服务 / ASP.NET Core / Generic Host，需要**低分配、高吞吐**日志
- 已统一在 `Microsoft.Extensions.Logging`，想换掉慢桥接 Provider
- Unity 2022.2+ 项目，希望用 Cysharp 生态做结构化/文件日志，而不只靠 `Debug.Log`
- 云原生控制台日志、JSON 采集、RollingFile 落盘

### 选型对照（与 wiki 内 MiniLog）

| 维度 | ZLogger | MiniLog |
|------|---------|---------|
| 出处 | Cysharp（国际/Unity 生态强） | 国产 Gitee，GA 较新 |
| 抽象层 | 原生 MEL | 自有 Logger→Appender，另提供 MEL 适配 |
| Unity | 官方 Unity 包与文档 | 未作为 Unity 一等公民 |
| 生态体量 | Star 更多、Cysharp 工具链协同 | 更轻、零外部依赖 |
| 配置风格 | MEL + ZLoggerOptions / Formatter | XML/JSON/INI + Fluent Appender |

需要 **Unity 官方路径 / Cysharp 生态 / MEL 原生** → 优先看 ZLogger；要 **零依赖 Appender 模型 / 国产可维护 fork** → 对照 MiniLog。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/Cysharp/ZLogger |
| **Git Clone** | https://github.com/Cysharp/ZLogger.git |
| **Releases** | https://github.com/Cysharp/ZLogger/releases |
| **最新版 v2.5.10** | https://github.com/Cysharp/ZLogger/releases/tag/2.5.10 |
| **NuGet** | https://www.nuget.org/packages/ZLogger |
| **Unity UPM** | `https://github.com/Cysharp/ZLogger.git?path=src/ZLogger.Unity/Assets/ZLogger.Unity` |
| **Cysharp 组织** | https://github.com/Cysharp |
| **Issues** | https://github.com/Cysharp/ZLogger/issues |
| **许可证** | MIT |

## My takeaways

1. **「cy 的 zlog」即 Cysharp/ZLogger**：不是 `github.com/cy/zlog`，而是 Cysharp 的零分配日志库，Unity/.NET 圈常用简称。
2. **对 unicore 有直接参考价值**：Unity 2022.2+ 可经 NuGetForUnity + UPM 接入；适合替代或增强 `Debug.Log`，做文件/JSON/结构化日志。
3. **性能叙事清晰**：默认追求快，强调 UTF8 直出与去掉 MEL 桥；适合高帧率或高吞吐场景评估。
4. **接入成本高于「丢进 Plugins」**：Unity 需配 `csc.rsp`、CsprojModifier、NuGetForUnity，团队要接受这套前置。
5. **与 MiniLog 互补**：同主题日志候选；ZLogger 偏 MEL+Cysharp+Unity，MiniLog 偏自有 Appender + 零依赖。

## Related

- [[MiniLog - 轻量高性能.NET日志库]]
