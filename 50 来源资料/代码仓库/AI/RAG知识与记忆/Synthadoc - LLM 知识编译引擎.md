---
id: source-20260909-synthadoc
title: Synthadoc - LLM 知识编译引擎
type: source
status: active
created: 2026-09-09
updated: 2026-09-09
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Wiki
  - 主题/知识库
  - 主题/个人知识管理
  - 主题/RAG
  - 工具/Obsidian
author: axoviq-ai
source_type: repo
source_url: https://github.com/axoviq-ai/synthadoc
source_author: axoviq-ai
source_date: 2026-09-09
summary: 开源 LLM 知识编译引擎：在摄入时把 PDF/Office/网页/音视频等原始材料合成为可互链、可审计的本地 Markdown Wiki（Obsidian 友好），以 ingest-time 编译对抗传统 query-time RAG；含矛盾检测、引用溯源、生命周期、MCP/CLI/Web/Obsidian 四入口。AGPL-3.0，Community Edition v1.3.2。
related:
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层|RAGFlow]]"
  - "[[50 来源资料/代码仓库/其他/awesome-okf - 开放知识格式中文资料与工具|awesome-okf]]"
  - "[[40 知识导航/Obsidian 与 AI|Obsidian 与 AI]]"
---

# Synthadoc - LLM 知识编译引擎

## Source summary

**Synthadoc** 是开源的 **LLM 知识编译（knowledge compilation）引擎**：读取原始文档，在**摄入时**用 LLM 合成为持久、结构化、可互链的本地 Wiki，而不是像传统 RAG 那样在查询时临时拼块摘要。产物是纯 Markdown + YAML frontmatter，可直接在 [Obsidian](https://obsidian.md) 或其他 Wiki 生态中阅读、编辑与备份。

- GitHub：`axoviq-ai/synthadoc`（约 **1.1k+ Stars**；协议 **AGPL-3.0**）
- 当前公开版：**Community Edition v1.3.2**（Python **3.11+**）
- 安装：`pip install synthadoc`
- 思想来源：Andrej Karpathy 的 [LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)——「LLM 应能替你维护一个 Wiki」
- 定位一句话：**ingest-time 编译 Wiki**，相对 **query-time RAG** 的透明、人可读、可自管自优替代方案

核心对比：RAG 检索 chunk；Synthadoc 把来源**编译进知识库**，自动建 `[[wikilinks]]`、发现矛盾与孤儿页、每条论断带出处，且 Wiki 离线仍可读。

## 核心能力

### 知识质量

| 能力 | 说明 |
|------|------|
| **摄入时合成** | 来源在 ingest 时写入 Wiki，查询不再重摘要整库 |
| **矛盾检测与解决** | 冲突页标 `status: contradicted`；可自动解决或人工复核 |
| **对抗性论断审查** | 第二 LLM 挑过强表述/无依据超级词；可配置门控降级 |
| **论断级出处** | `^[file:L-L]` 行级引用；Obsidian Source Viewer；断引用 lint |
| **五态生命周期** | `draft → active → contradicted / stale → archived`；lint 自动迁移；不可变事件日志 |
| **摄入前消毒** | 剥离零宽字符、双向覆盖、隐藏 HTML、指令覆盖短语 |

### 知识结构

| 能力 | 说明 |
|------|------|
| **加权知识图谱** | wikilink + 共来源边；Web UI / Obsidian Canvas 可视化；Louvain 聚类 |
| **孤儿页检测** | lint 列出无引用页并给出可粘贴索引条目 |
| **ROUTING.md** | 按分支路由查询；新页自动归槽 |
| **Candidates 暂存** | 先入暂存区，审阅后再晋升到正式 Wiki |
| **Scaffold** | 按现状重生成 index / AGENTS.md / purpose.md，不覆盖受保护页 |
| **领域模板** | 约 30 套（金融/技术/医疗/法律等）开箱结构 |

### 检索与问答

| 能力 | 说明 |
|------|------|
| **问题分解 + 缺口提示** | 复合问并行 BM25；结果过薄时给知识缺口与建议搜索 |
| **BM25 + 可选语义重排** | 小语料 TF 回退；可选 `BAAI/bge-small-en-v1.5` |
| **Web / YouTube 摄入** | Tavily 分解搜索；YouTube 字幕（可无 API Key） |
| **CJK 友好** | 中日韩查询不易误报知识缺口 |
| **流式输出 + 查询缓存** | 缓存键 = 问题 + Wiki 版本；摄入/生命周期变更自动失效 |

### 接口与集成

| 能力 | 说明 |
|------|------|
| **Obsidian 插件** | 摄入、流式问答、lint、生命周期、出处、图谱、后台 vault 监控 |
| **Web Chat UI** | `synthadoc web`：多轮会话、缺口提示、图谱页 |
| **MCP Server** | 约 12 工具；stdio / SSE / HTTP，接 Claude Desktop / Code、n8n、LangGraph |
| **Context Pack** | 目标 → 子问题 → 按 token 预算打包证据，可贴进任意 LLM |
| **导出** | `llms.txt` / `llms-full.txt` / GraphML / JSON / **OKF v0.1** bundle |
| **Agent 指引文件** | `AGENTS.md` / `CLAUDE.md` / `GEMINI.md`，scaffold 可再生 |

### 运维与信任

| 能力 | 说明 |
|------|------|
| **Local-first** | 源文件默认不出本机；Wiki 纯 Markdown，无服务也可读 |
| **备份/恢复** | 单 zip（页面 + audit/lifecycle DB + 配置）；可改写端口/域名，无需重摄入 |
| **快照与回滚** | 生命周期变更与 vault 保存可快照；`lifecycle rollback` 可撤销 |
| **成本守卫 + 审计** | 按任务记 token/费用；软警告/硬门控；`audit.db` 不可变日志 |
| **可恢复任务队列** | ingest/lint 持久化状态，崩溃可续跑 |
| **Hooks + 自定义 Skill** | `on_ingest_complete` / `on_lint_complete`；可挂 git 自动提交等 |
| **敏感信息撤回** | 扫描 API Key/邮箱/证件等并 `[REDACTED]`；增量扫描 |
| **Agentic 维护工作流** | 过期重摄入、断链修复、矛盾/孤儿/断引用解决等（写前确认） |

## 支持的内容源与模型

**内容源：** PDF、DOCX、PPTX、XLSX/CSV、Markdown、TXT、图片（视觉）、网页 URL、YouTube 字幕、AI 会话转录（`.jsonl`）等。

**LLM 后端：** Gemini（默认免费档 Flash）、Groq、Qwen（DashScope）、MiniMax、DeepSeek、Anthropic、OpenAI、本地 Ollama；也可走 **Claude Code / Opencode** CLI（无需单独 API Key）。可选 `TAVILY_API_KEY` 做联网搜索摄入。

## 快速开始

```bash
pip install synthadoc
synthadoc --version

# 安装演示 Wiki（可先不配 Key 浏览）
synthadoc install history-of-computing --target %USERPROFILE%\wikis --demo

# 启动引擎（默认 localhost:7070）
synthadoc serve -w history-of-computing
```

正式使用需至少一个 LLM Key（或配置 coding-tool provider）。Wiki 根目录 `.synthadoc/config.toml` 的 `[agents]` 可切换模型；Obsidian 插件随包捆绑，新 Wiki `install` 时自动装入，升级可用 `synthadoc plugin upgrade`。

完整走读见仓库 [docs/user-quick-start-guide.md](https://github.com/axoviq-ai/synthadoc/blob/main/docs/user-quick-start-guide.md)；架构见 [docs/design.md](https://github.com/axoviq-ai/synthadoc/blob/main/docs/design.md)。

## 适用场景

- 个人/小团队要把散落 PDF、纪要、网页沉淀成**可浏览、可互链、可审计**的本地知识库
- 想要 **Wiki 制品本身**（人可读 Markdown），而不是只依赖向量库里的不可见 chunk
- 需要矛盾发现、论断出处、生命周期与成本审计（合规/尽调/研究笔记）
- 已用 Obsidian，希望 CLI / Web / MCP / 插件四入口统一管知识
- 对 local-first、可 git 备份、可迁机（zip restore）有强需求

## 与同类资源对比（简要）

| 项目 | 侧重点 |
|------|--------|
| **Synthadoc** | ingest-time 编译本地 Markdown Wiki；Obsidian 一等公民；矛盾/出处/生命周期强 |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架\|WeKnora]] | 腾讯企业知识平台：RAG + ReAct + Wiki + 国内 IM/飞书语雀；产品化与 RBAC |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层\|RAGFlow]] | 产品化 query-time RAG 引擎（DeepDoc、分块、多源同步） |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统\|MemPalace]] | Agent 对话长期记忆（逐字存储 + 语义召回），非文档 Wiki 编译 |
| [[50 来源资料/代码仓库/其他/awesome-okf - 开放知识格式中文资料与工具\|awesome-okf]] | OKF 规范/工具索引；Synthadoc 导出兼容 OKF v0.1 |

## 演示与媒体

| 资源 | 链接 |
|------|------|
| From Documents to Wiki | https://www.youtube.com/watch?v=rIGO6zi9XQE |
| Four Interfaces（CLI / Obsidian / Web / MCP） | https://youtu.be/ue_kHhG0iog |
| Agentic Maintenance Workflow | https://www.youtube.com/watch?v=ojBtNlXVHQk |
| 端到端示例 AquaFlow（并购尽调） | 仓库 `docs/example/aquaflow/` |
| Blogs & Media 索引 | 仓库 `docs/media/README.md` |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/axoviq-ai/synthadoc |
| **PyPI** | https://pypi.org/project/synthadoc/ |
| **快速开始** | https://github.com/axoviq-ai/synthadoc/blob/main/docs/user-quick-start-guide.md |
| **设计/架构** | https://github.com/axoviq-ai/synthadoc/blob/main/docs/design.md |
| **定制化** | https://github.com/axoviq-ai/synthadoc/blob/main/docs/design.md#customization |
| **领域模板** | https://github.com/axoviq-ai/synthadoc/tree/main/synthadoc/templates |
| **Obsidian 插件源码** | https://github.com/axoviq-ai/synthadoc/tree/main/obsidian-plugin |
| **Karpathy LLM Wiki gist** | https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f |
| **许可证** | https://github.com/axoviq-ai/synthadoc/blob/main/LICENSE |
| **Issues** | https://github.com/axoviq-ai/synthadoc/issues |
| **贡献指南** | https://github.com/axoviq-ai/synthadoc/blob/main/CONTRIBUTING.md |

## My takeaways

1. **范式差异是选型关键**：要「可维护的本地 Wiki 制品」优先 Synthadoc；要「企业级查询时 RAG 产品」看 RAGFlow / WeKnora。
2. **与本库（Obsidian 个人笔记）高度同构**：产出就是 Markdown vault + wikilink + frontmatter，插件与图谱可直接叠在 Obsidian 工作流上。
3. **信任机制完整**：行级引用、对抗审查、生命周期、audit.db、敏感撤回——比「只聊天总结文档」更适合长期知识资产。
4. **协议注意**：AGPL-3.0，商用闭源二次分发需评估合规；个人/内网自用通常压力较小。
5. **成本可控路径清晰**：默认 Gemini Flash 免费档 + 三层缓存 + 成本门控；也可本地 Ollama / coding-tool CLI 少碰云 API Key。

## Related

- [[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora - LLM 知识管理框架]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层|RAGFlow - 开源 RAG 引擎与 Agent 上下文层]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]
- [[50 来源资料/代码仓库/其他/awesome-okf - 开放知识格式中文资料与工具|awesome-okf - 开放知识格式中文资料与工具]]
- [[40 知识导航/Obsidian 与 AI|Obsidian 与 AI]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
