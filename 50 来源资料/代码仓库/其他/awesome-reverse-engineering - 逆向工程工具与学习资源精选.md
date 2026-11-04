---
id: source-20261104-awesome-reverse-engineering
title: awesome-reverse-engineering - 逆向工程工具与学习资源精选
type: source
status: active
created: 2026-11-04
updated: 2026-11-04
tags:
  - 来源/代码仓库
  - 主题/开源资源
  - 主题/逆向工程
  - 主题/安全工具
  - 主题/二进制分析
  - 主题/MCP
author: morluto
source_type: repo
source_url: https://github.com/morluto/awesome-reverse-engineering
source_author: morluto
source_date: 2026-11-04
summary: 按任务场景组织的逆向工程 Awesome 列表：覆盖学习、文件甄别、原生分析、调试、动态插桩、去混淆、恶意软件、安卓/.NET、iOS/macOS、语言运行时、Web/协议、固件、文件格式、游戏、二进制 diff、符号执行、指令提升与 AI Agent 辅助分析；同作者另推 REA；CC0-1.0，约 1767 Stars。
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA - Agent 逆向分析 MCP]]"
---

# awesome-reverse-engineering - 逆向工程工具与学习资源精选

## Source summary

**awesome-reverse-engineering**（[morluto/awesome-reverse-engineering](https://github.com/morluto/awesome-reverse-engineering)）是一份面向实践的**逆向工程工具与学习资源精选列表**：按「你要完成什么任务」组织条目，覆盖二进制、应用、固件、文件格式与协议。

- GitHub：`morluto/awesome-reverse-engineering`（许可证 **CC0-1.0**；约 **1767** Stars / **395** Forks，2026-11-04 API）
- 作者/维护：morluto（与 [[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA]] 同作者）
- 定位：按任务导航的 RE 起点目录；商业工具条目会标注 **commercial**
- 主语言标注为 Python（仓库含维护脚本）；内容主体是 Markdown 列表
- Topics：`awesome-list`、`reverse-engineering`、`ghidra`、`frida`、`malware-analysis`、`ai-agents`、`mcp`、`rea` 等
- 创建于 2026-10-09；链接检查由 GitHub Actions 在 PR / 推送 / 每周运行

列表顶部 **Featured**：[REA](https://github.com/morluto/rea) — 面向 Agent 的逆向分析工具链（MCP/CLI），与本列表互补：本仓是选型地图，REA 是可接到 Agent 的分析能力层。

## 核心能力

| 能力 | 说明 |
|------|------|
| **按任务找工具** | README「Start here」表：从「学汇编」「看 APK」「拆固件」「做 binary diff」等任务直达对应章节 |
| **场景覆盖全** | 学习、甄别、原生分析、调试器、动态插桩、去混淆/反分析、恶意软件、安卓与托管代码、iOS/macOS、语言运行时、Web/协议、固件硬件、文件格式、游戏、diff/补丁、符号执行、指令提升、AI 辅助 |
| **条目写清用途** | 每条说明适用时机；商业产品标 **commercial**；部分条目注明维护状态/平台限制 |
| **维护与证据** | CONTRIBUTING + 链接检查 Actions；另有 catalog / priority-candidate 等源评审文档，区分「文档宣称」与「可复现示例」 |
| **关联列表导航** | Related lists 指向 AI RE、嵌入式安全、游戏格式、Web reversing 等专题 Awesome，避免单仓装下所有细分领域 |

## 分类速览

| 章节 | 解决什么 | 代表性资源（摘） |
|------|----------|------------------|
| Learning / Books | 入门汇编与逆向 | beginners.re、pwn.college、OST2、Practical Malware Analysis |
| File identification and triage | 先认清样本与能力 | capa、DIE、FLOSS、LIEF、PE-bear |
| Native analysis | 反汇编/反编译 | Ghidra、IDA、Binary Ninja、radare2/Rizin、Cutter |
| Debuggers | 单步与运行时观察 | x64dbg、GDB+GEF/pwndbg、WinDbg、rr、LLDB |
| Dynamic instrumentation | 插桩与追踪 | Frida、Pin、DynamoRIO、bpftrace、Sysinternals |
| Deobfuscation / anti-analysis | 脱壳、重建导入、反反调试 | PE-sieve、Scylla、ScyllaHide、de4dot |
| Malware analysis | 沙箱、内存、规则 | CAPE、Volatility 3、YARA/YARA-X、FLARE-VM、REMnux |
| Android and managed code | APK / .NET / Java | JADX、Apktool、dnSpyEx、ILSpy、MobSF |
| iOS and macOS | Apple 生态 | ipsw、Hopper、objection、pymobiledevice3 |
| Language runtimes | PyInstaller/Go/Flutter/Hermes 等 | pyinstxtractor、GoReSym、Blutter、hermes-dec |
| Web and protocols | 流量与协议重建 | Wireshark、mitmproxy、Scapy、Burp、CyberChef（格式章也常用） |
| Firmware and hardware | 固件提取与硬件侧 | Binwalk、unblob、OFRAK、QEMU、OpenOCD、UEFITool |
| File formats | 结构化二进制 | ImHex、Kaitai、010 Editor、Construct |
| Game RE | 引擎资产与内存结构 | Il2CppDumper、Cpp2IL、Cheat Engine、AssetRipper、FModel |
| Binary diffing | 版本对比与补丁审计 | BinDiff、Diaphora、ghidriff |
| Symbolic / formal | 约束求解与语义探索 | angr、Triton、Unicorn、Qiling、Z3 |
| Instruction lifting | 反汇编库与提升 | Capstone、Keystone、Zydis、Remill、BAP |
| AI-assisted analysis | Agent / MCP 接分析器 | IDA/Ghidra/radare2/JADX/WinDbg 等 MCP；仓库 Featured 的 REA |
| Practice | 练习与社区 | crackmes.one、FLARE-On、picoCTF、r/ReverseEngineering |

完整条目与描述以仓库 README 为准。

## 与同类资源 / 本库笔记的关系

| 资源 | 侧重 | 关系 |
|------|------|------|
| **本列表** | 通用 RE 工具与学习总索引 | 选型起点 |
| [[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP\|REA]] | Agent 逆向 MCP/CLI | 同作者 Featured；列表选型 → REA 落地分析 |
| Awesome AI Reverse Engineering 等 Related lists | AI 集成 / 嵌入式 / 游戏格式等专题 | README 已链出，需要细分时再下钻 |
| Reversing.ID Awesome-Reversing 等 | 另一套软件/硬件/格式目录 | 可交叉对照，非替代 |

## 适用场景

- 做安全研究、CTF、恶意软件分析或互操作分析时，需要**按任务快速缩小工具候选**
- 搭建分析环境（Windows FLARE-VM / Linux REMnux 等）前做工具地图
- 给 AI Agent 选逆向相关 MCP / 技能前，先看「AI-assisted analysis」章与 Featured REA
- 查某细分领域（固件、Flutter、游戏引擎）时，从本列表进门，再跟 Related lists 或单点仓库笔记

## 合规提示

- 列表是**工具目录**，不是未授权破解教程；使用任一条目须遵守授权与当地法律。
- 恶意软件相关章节强调隔离环境与组织处置流程；笔记只作资源索引，不展开攻击利用步骤。
- 商业工具以厂商许可为准；标 commercial 的条目通常另有免费/社区版限制。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/morluto/awesome-reverse-engineering |
| **Featured：REA** | https://github.com/morluto/rea |
| **贡献说明** | https://github.com/morluto/awesome-reverse-engineering/blob/main/CONTRIBUTING.md |
| **列表结构评审** | https://github.com/morluto/awesome-reverse-engineering/blob/main/docs/list-review.md |
| **目录扩展开源评审** | https://github.com/morluto/awesome-reverse-engineering/blob/main/docs/catalog-expansion-review.md |
| **优先候选源评审** | https://github.com/morluto/awesome-reverse-engineering/blob/main/docs/priority-candidates.md |
| **链接检查 Actions** | https://github.com/morluto/awesome-reverse-engineering/blob/main/.github/workflows/links.yml |
| **许可证** | https://github.com/morluto/awesome-reverse-engineering/blob/main/LICENSE（CC0-1.0） |
| **作者** | https://github.com/morluto |

## My takeaways

1. **价值在「按任务导航」**：比单纯堆工具名更适合「我现在卡在某一步该用什么」的检索方式。
2. **与 REA 成对记忆**：本仓 = 开源/商业 RE 生态地图；[[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA]] = 把分析能力接到 Agent 的落地层。
3. **维护意识较强**：死链检查 + 源评审文档，比「只 curated 不管时效」的 awesome 更值得当长期入口。
4. **局限**：条目多、英文为主；深度对比与版本兼容仍要回各项目文档；Stars 中等（相对 REA 热度），以内容结构而非热度为选型依据。

## Related

- [[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA - Agent 逆向分析 MCP]]
