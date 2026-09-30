---
id: source-20260924-headroom
title: Headroom - LLM 上下文压缩中间件
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Token优化
  - 主题/上下文工程
  - 主题/AI编程Agent
  - 主题/MCP
author: chopratejas / Headroom Labs
source_type: repo
source_url: https://github.com/headroomlabs-ai/headroom
source_author: headroomlabs-ai / chopratejas
source_date: 2026-09-24
summary: 本地运行的 LLM 上下文压缩中间件：在 Agent 与模型之间压缩工具输出、日志、代码与 RAG 内容；支持 Library / Proxy / Wrap / MCP；可逆 CCR；Apache-2.0。头条文称结构化场景最高约省 92% Token，精度基本不掉。
related:
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/codebase-memory-mcp - 代码库知识图谱 MCP|codebase-memory-mcp]]"
  - "[[50 来源资料/代码仓库/AI/多模态与模型/OpenMontage - Agent 化视频制作系统|OpenMontage]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# Headroom - LLM 上下文压缩中间件

## Source summary

**Headroom** 是开源、本地运行的 **LLM 上下文压缩层**：把要喂给模型的工具输出、日志、文件、RAG chunk、对话历史等先压缩再送入 LLM，目标是「同样答案、更少 Token」。压缩在本机完成，内容不会发到远端做压缩。

- 开源项目，GitHub：[headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom)
- 主要贡献者：[@chopratejas](https://github.com/chopratejas) 等（Headroom Labs）
- 语言：Python / TypeScript / Rust / C 等混合
- 约 **72k+ stars / 5.5k+ forks**（截至 2026-09-24，随时间变化）
- 许可证：**Apache-2.0**
- 文档站：[docs.headroomlabs.ai](https://docs.headroomlabs.ai/docs)
- 口号方向：*Compress everything your agent reads — same answers, fewer tokens*

来源线索：今日头条文章《省92%Token还不降精度？GitHub热榜工具太狠了》（作者：AI探索家，2026-09-14）将其列为热榜「省 Token」代表项目之一。

## 解决什么问题

| 痛点 | Headroom 方案 |
|------|----------------|
| Agent 工具输出 / 日志 / 搜索结果 Token 爆炸 | ContentRouter 按类型分流到专用压缩器 |
| 粗暴截断怕丢关键信息 | **CCR**（压缩-缓存-取回）：原文本地缓存，模型可 `headroom_retrieve` 按需还原 |
| 不想改 Agent 代码 | Proxy / `headroom wrap` 一行接入 |
| 还要付模型「啰嗦输出」的钱 | 可选 Output Shaper（verbosity / effort routing） |

## 核心能力

| 能力 | 说明 |
|------|------|
| **Library** | Python/TS：`compress(messages)` 内联调用 |
| **Proxy** | `headroom proxy --port 8787`，零改代码 |
| **Agent wrap** | `headroom wrap claude\|codex\|cursor\|opencode\|…`；`unwrap` 还原 |
| **MCP** | `headroom_compress` / `headroom_retrieve` / `headroom_stats` |
| **ContentRouter** | 识别内容类型后选压缩器（Magika 等，文章称约 5ms、准确率很高） |
| **SmartCrusher** | JSON：统计找拐点；错误条目高优先级保留 |
| **CodeCompressor** | 基于 AST：偏保留签名、砍冗余函数体，输出合法语法 |
| **日志压缩** | 模式匹配，ERROR/FAIL/WARN 等关键行优先保留 |
| **CCR** | 原文进本地 SQLite + hash；模型取回延迟极低 |
| **Cross-agent memory** | Claude / Codex / Gemini / Grok 等共享去重记忆 |
| **headroom learn** | 从失败会话挖修正，写入 `CLAUDE.md` / `AGENTS.md` 等 |

## 官方/文中 benchmark（注意口径）

头条文与早期宣传常用的「高结构化」数字（示例）：

| 场景 | 压缩前 | 压缩后 | 节省 |
|------|--------|--------|------|
| 代码搜索 100 条 | 17,765 | 1,408 | ~92% |
| 生产 FATAL 日志 100 条 | 10,144 | 1,260 | ~87.6% |
| SRE 故障排查 | 65,694 | 5,118 | ~92% |

仓库 README（2026-09 附近）用同一套离线 proof 表给出的 **默认 compress() 口径**更保守，例如代码搜索约 21%、SRE 约 57%——**重复 JSON/日志可到 90%+，散文与高密度短代码几乎压不动**。选型时以 `headroom savings` / 自有流量为准。

精度侧（官方 eval 口径示例）：GSM8K 持平；TruthfulQA 小幅波动在置信区间内；SQuAD / BFCL 在一定压缩比下仍约 97%。

## 使用注意（文中 + README）

1. **grep 结果、短源码**等密度极高内容：压缩率可能接近 0。
2. **激进压缩务必开 CCR**，关掉易掉精度。
3. **动态 system prompt**（时间戳、UUID）会打穿 KV cache；易变内容放到尾部。
4. 压缩本身延迟通常远低于模型推理（亚毫秒～数毫秒级）。

## 安装与快速上手

```bash
uv tool install --python 3.13 "headroom-ai[all]"
# 或: pip install "headroom-ai[all]"
# npm 包 headroom-ai 仅为 TypeScript SDK，不含 CLI

headroom deploy
headroom wrap claude          # 或其他 agent
headroom proxy --port 8787
headroom doctor
headroom dashboard             # 需 proxy 在跑
```

## 适用场景

- 跑 Coding Agent / 工具调用密集、日志与 JSON 冗余高的流水线
- 想 **零改业务代码** 先试 Proxy / Wrap 看账单
- 需要与 [[50 来源资料/代码仓库/AI/RAG知识与记忆/codebase-memory-mcp - 代码库知识图谱 MCP|codebase-memory-mcp]] 搭配：一个管「记住结构」，一个管「少喂 Token」

**不太适合**：已经极短的上下文；不能接受本地 sidecar/代理；合规要求「绝对不能改写任何工具输出」且又不想开 CCR 取回。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/headroomlabs-ai/headroom |
| **文档** | https://docs.headroomlabs.ai/docs |
| **llms.txt** | 仓库根目录 /llms.txt |
| **介绍文章（头条）** | https://www.toutiao.com/article/7685280926175117876/ |
| **相关镜像文（腾讯云）** | https://cloud.tencent.com/developer/article/2703769 |

## My takeaways

1. **定位**：Agent 与 LLM 之间的「传输层压缩」，不是又一个 RAG 框架。
2. **可逆 CCR** 是相对粗暴截断的关键差异；生产建议默认打开。
3. **宣传 92%** 多出现在高冗余结构化场景；日常 coding agent 可能更接近 20%–60%，应用前先测。
4. 与 codebase-memory-mcp 互补：结构记忆降「瞎读文件」，Headroom 降「已读内容的体积」。

## Related

- [[50 来源资料/代码仓库/AI/RAG知识与记忆/codebase-memory-mcp - 代码库知识图谱 MCP|codebase-memory-mcp - 代码库知识图谱 MCP]]
- [[50 来源资料/代码仓库/AI/多模态与模型/OpenMontage - Agent 化视频制作系统|OpenMontage - Agent 化视频制作系统]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
