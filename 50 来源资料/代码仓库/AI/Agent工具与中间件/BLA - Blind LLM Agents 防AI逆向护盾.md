---
id: source-20261110-bla
title: BLA - Blind LLM Agents 防AI逆向护盾
type: source
status: active
created: 2026-11-10
updated: 2026-11-10
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/MCP
  - 主题/CLI
  - 主题/安全工具
  - 主题/逆向工程
  - 主题/软件保护
  - 主题/开源资源
author: qybaihe
source_type: repo
source_url: https://github.com/qybaihe/bla
source_author: qybaihe
source_date: 2026-11-10
summary: "开源 Counter-REA 蓝队防御 MCP/CLI（npm 包 bla-agents）：用 Poison→Exhaust→Hallucinate 加固二进制、Electron、JS、WASM、Python 等 19+ 种产物，抬高 AI 逆向 Agent 的 Token/成本，并保持运行时等价。仅限保护自有软件。"
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA - Agent 逆向分析 MCP]]"
  - "[[50 来源资料/代码仓库/其他/awesome-reverse-engineering - 逆向工程工具与学习资源精选|awesome-reverse-engineering]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# BLA - Blind LLM Agents 防AI逆向护盾

## Source summary

**BLA**（Blind LLM Agents，仓库 [qybaihe/bla](https://github.com/qybaihe/bla)）是开源的 **Counter-REA 蓝队防御**能力层：通过 **MCP + CLI**（npm 包 [`bla-agents`](https://www.npmjs.com/package/bla-agents)）保护软件发布产物，对抗 `morluto/rea` 一类 AI 逆向代理与 LLM 侦察流水线，同时强调 **100% 运行时等价**。

- 约 **4408 Stars** / **537 Forks**（2026-11-10 GitHub API）；许可证 **MIT**；主语言 **TypeScript**；npm 包版本约 **3.2.2**
- 官网：[bla.tools](https://bla.tools/) · [防御指南](https://bla.tools/guides/) · [基准大盘](https://bla.tools/showcase/) · [CrackMe 竞技场](https://bla.tools/crackme/)
- 口号：**Poison → Exhaust → Hallucinate**（投毒 → 耗尽预算 → 诱导幻觉/误判）
- 与 [[50 来源资料/代码仓库/AI/Agent工具与中间件/REA - Agent 逆向分析 MCP|REA]] 对偶：REA 是红队/分析侧，BLA 是蓝队/防护侧

来源线索：今日头条文章 [中大本科生反AI逆向项目火了！让黑客Token账单原地暴涨300倍](https://www.toutiao.com/article/7706296357258084906/)（侧重热度与「Token 暴涨」叙事；能力边界与用法以官方 README 为准）。

## 解决什么问题

| 痛点 | BLA 方案 |
|------|----------|
| AI 逆向 Agent 可低成本解包 ASAR、还原 SourceMap、抽 WASM/SQLite/权重 | `bla harden` / MCP 闭环对产物注入护盾，抬高分析配额与 Token 成本 |
| 传统混淆对 LLM「控制流归纳」几乎无效 | 不只改名：蜜罐诱饵 + 预算爆破 + 跨产物纠缠 + 运行时 Truman 护盾 |
| 商业虚拟化壳重、易破跨平台构建 | 格式原生护盾内置于 `bla-agents`，无外部加壳器依赖 |
| 加固后怕行为漂移 | 不透明谓词/惰性段承载诱饵；基准目标宣称 14/14 输入输出等价 |

## 核心能力（四大支柱）

| 支柱 | 机制 | 对 AI 逆向的效果 |
|------|------|------------------|
| **Poison 认知投毒** | 格式合法蜜罐凭据、硬件飞地清单、VLQ v3 诱饵 SourceMap、影子状态机 | 迫使模型在冲突假说间分流，追合成线索 |
| **Exhaust 预算爆破** | 精确触发分析器硬配额（ASAR 头、WASM Wabt、SQLite 虚拟列、模型嵌套层等） | 在触达真逻辑前将结果打成 `LIMITED` / 容量错误 |
| **Entangle 反剥离缠绕** | `--self-seal` / sidecar 哈希绑定；`bla_bind_scope` 多点调用域折叠 | 阻碍裁剪诱饵、单点 NOP、离线当解密 Oracle |
| **Runtime 楚门世界** | Electron Fuse 加固；检测 CDP / Inspector / Frida / WebDriver | 动态调试时返回合成遥测，正常用户路径零感知 |

## Agent 闭环（四阶段）

1. **侦察规划**：`bla_audit` / `bla_lint` → `bla_weave_plan`
2. **源码手术**：`bla_encrypt_strings` → `bla_bind_scope` → `bla_verify_weave`
3. **产物护盾**：`bla_harden` / `bla_defend`（19+ 格式 + Self-Seal）
4. **对抗门禁**：`bla_verify`（预算触发验证 + 运行等价检查）

`npx bla-agents setup` 后可对 Agent 说：「使用 BLA 对当前项目进行防逆向闭环加固」。

## 支持的产物格式（摘要）

内置 **19+** 种护盾（无需 Hopper/Ghidra/WABT 等外部加壳器）：

| 类别 | 示例 |
|------|------|
| 前端/桌面 | JavaScript/TypeScript、Electron `.asar`、Electron Fuses |
| 二进制与字节码 | WASM、Python/pyc、DEX、.NET、JVM、Erlang BEAM、EVM、Native ELF |
| 数据与模型 | SQLite、Safetensors、GGUF |
| 其他 | PCAP、Flutter/Dart 常量池、NVIDIA PTX、ZIP/bplist 等 |

真机 vs 真实 REA 的格式级结果见仓库 README / [empirical-rea-validation](https://github.com/qybaihe/bla/blob/main/docs/redteam/empirical-rea-validation.md)。基准宣称平均 Token 放大约 **~639.5×**、逆向成本约 **~304.4×**（估算，以官方表格为准）。

## 快速开始

**环境**：推荐 Node.js `^22.23.4` 或 `>=24.0.0`。

```bash
# 一键加固当前项目/构建目录
npx bla-agents harden .

# 注册到本机编程 Agent（Claude Code / Cursor / Codex / Windsurf / Gemini CLI / Copilot CLI / VS Code 等）
npx bla-agents setup

# 复现 14 目标基准
npx bla-agents benchmark
```

构建插件示例（Vite）：

```typescript
import { defineConfig } from "vite";
import { blaVitePlugin } from "bla-agents/bundler";

export default defineConfig({
  plugins: [blaVitePlugin({ intensity: "bankrupt", enableHoneySourceMaps: true })],
});
```

## 相关链接

| 类型 | 链接 |
|------|------|
| GitHub | https://github.com/qybaihe/bla |
| Issues | https://github.com/qybaihe/bla/issues |
| npm | https://www.npmjs.com/package/bla-agents |
| 官网 | https://bla.tools/ |
| 中文 README | https://github.com/qybaihe/bla/blob/main/docs/readme/README_zh.md |
| CLI/MCP 手册 | https://github.com/qybaihe/bla/blob/main/docs/cli-and-mcp.md |
| 免责声明 | https://github.com/qybaihe/bla/blob/main/DISCLAIMER.md |
| 对偶项目 REA | https://github.com/morluto/rea |
| 来源文章 | https://www.toutiao.com/article/7706296357258084906/ |

## 合规与风险（必读）

- **仅限防御性使用**：保护**自有**软件 IP 与发布产物；官方强调非攻击工具。
- 从官方仓库或 npm `bla-agents` 安装；加固与验证宣称**本地离线**完成（不上传源码/二进制；项目内 `.blarc` 随机种子）。
- 对抗效果针对特定 AI 逆向流水线（如 REA）的预算/容量路径做了实测；**不等于**对所有人工逆向工具链（Ghidra/Frida/IDA 等）普遍「不可逆向」。
- 使用前阅读 [DISCLAIMER.md](https://github.com/qybaihe/bla/blob/main/DISCLAIMER.md)，并遵守当地法律与软件许可。

## 适用场景

- 需要降低 Electron / JS 产物被 Agent「一键克隆」风险的独立开发者或小团队
- 已在用 Cursor / Claude Code 等 Agent，希望把防逆向织入发布流水线
- 研究 AI 红蓝对抗、软件保护与 LLM 安全预算攻击面时，作为 Counter-REA 参考实现
