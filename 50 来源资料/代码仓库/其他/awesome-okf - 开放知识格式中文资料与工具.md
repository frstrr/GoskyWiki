---
id: source-20260909-awesome-okf
title: awesome-okf - 开放知识格式中文资料与工具
type: source
status: active
created: 2026-09-09
updated: 2026-09-09
tags:
  - 来源/代码仓库
  - 主题/个人知识管理
  - 主题/开源资源
  - 主题/知识格式
  - 工具/Obsidian
author: yzfly
source_type: repo
source_url: https://github.com/yzfly/awesome-okf
source_author: 云中江树 / yzfly
source_date: 2026-09-09
summary: 中文世界 OKF（开放知识格式）落点仓库：规范中文翻译、7 个 producer 插件（飞书/Obsidian/Notion/GitHub/awesome/HTML→OKF）、7 个 Claude Code skill、上游扩展提案，以及符合 OKF v0.2 的自托管 bundle 范例；MIT 许可，约 66 Stars。
related:
  - "[[30 知识资源/其他/Obsidian 个人知识库方法论|Obsidian 个人知识库方法论]]"
  - "[[30 知识资源/其他/Obsidian 知识库自动沉淀工作流|Obsidian 知识库自动沉淀工作流]]"
  - "[[40 知识导航/Obsidian 与 AI|Obsidian 与 AI]]"
  - "[[50 来源资料/代码仓库/其他/Memos - 自托管轻量笔记工具|Memos - 自托管轻量笔记工具]]"
  - "[[50 来源资料/代码仓库/AI/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]"
  - "[[50 来源资料/代码仓库/AI/WeKnora - LLM 知识管理框架|WeKnora - LLM 知识管理框架]]"
---

# awesome-okf - 开放知识格式中文资料与工具

## Source summary

**awesome-okf** 是中文世界面向 **OKF（Open Knowledge Format / 开放知识格式）** 的资料与工具枢纽：规范译文、转换插件、Claude Code skill、上游扩展提案，以及一份「活的」合规范例（仓库自身即 OKF v0.2 bundle）。

- GitHub：`yzfly/awesome-okf`（**MIT** 许可，约 **66** Stars / **12** Forks）
- 作者/维护：云中江树（yzfly）；微信公众号同名
- 定位：中文 OKF 落点——规范翻译 + 工具链 + 提案 + 范例
- 技术栈：以 **Python** 为主（plugins / skills）；零第三方依赖的标准库实现
- Topics：`knowledge-base`、`markdown`、`okf`、`open-knowledge-format`
- 创建于 2026-06；规范侧已对齐 **OKF v0.2**

### OKF 是什么

[OKF](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) 由 Google Cloud 发布：把知识定义为**目录内的 Markdown 文件 + YAML frontmatter + 一小套约定**。没有运行时、没有 SDK。官方规范独立仓见 [open-knowledge-format](https://github.com/GoogleCloudPlatform/open-knowledge-format)（2026-08 自 knowledge-catalog 拆出，SPEC v0.2）。

v0.2 要点：出处（`sources`）、信任（`generated`/`verified`）、生命周期（`status`/`stale_after`）、可验算计算（`Attested Computation`）；破坏性变更含 `timestamp` → `generated.at`、正文 Citations → 头信息 `sources`。

## 核心能力

| 能力 | 说明 |
|------|------|
| **规范中文译本** | `docs/okf-spec-zh.md` 全文翻译，标注硬要求与留白；含 v0.1→v0.2 差异说明 |
| **Producer 插件（7）** | 飞书 / Obsidian / Notion / GitHub / awesome 列表 / HTML → OKF；`myokf-cli` 统一入口 |
| **Claude Code Skills（7）** | 创建库、导入 awesome、书/代码转 OKF、发布 VitePress / 单文件网页（含图谱） |
| **上游扩展提案** | i18n（`lang`+`canonical`）、代码支持（符号/行号锚点）、HTML 一等公民——向后兼容、不动 MUST |
| **生态索引** | README 维护 OKF 相关热门仓库表（官方、LangChain openwiki、iwe、MCP 记忆、校验器、RAG 等） |
| **Dogfooding** | 仓库自身是合规范例：`.md`+frontmatter+`type`，根目录 `index.md` / `log.md`，可本地校验 |

## 工具链速览（plugins）

| 工具 | 输入 → OKF |
|------|------------|
| feishu-to-okf | 飞书知识空间 / 文档 |
| obsidian-to-okf | Obsidian vault（wikilink → OKF 链接） |
| notion-to-okf | Notion Markdown 导出 |
| github-to-okf | GitHub 仓库（提取代码符号） |
| awesome-to-okf | GitHub awesome-xx 列表 |
| html-to-okf | HTML 文件 |
| myokf-cli | 以上工具的统一 CLI |

```bash
pip install myokf-cli
myokf from-github yzfly/awesome-okf -o ./kb
myokf validate ./kb
myokf to-web ./kb -o kb.html
```

## Skills 速览（Claude Code）

| Skill | 用途 |
|-------|------|
| okf-creator | 从零创建高质量 OKF 知识库 |
| awesome-to-okf | 导入 awesome 列表并富化 |
| book-to-okf | 书 / 长文拆成互链概念库 |
| code-to-okf | 代码库转 OKF |
| github-to-okf | 仓库 → OKF 富化工作流 |
| okf-to-book | OKF 发布为 VitePress 文档站 |
| okf-to-web | OKF 打包成单文件网页（含图谱） |

## 文档与提案

| 资源 | 说明 |
|------|------|
| OKF 规范中文版 | 全文翻译与硬要求标注 |
| 发布博客中文版 | 官方博客译文 |
| Karpathy LLM Wiki | 思想来源说明 |
| 代码/PDF/图片支持度调研 | 规范能力边界 |
| 全网资料汇总 | 外部资源索引 |
| dogfooding 说明 | 本仓如何做成 OKF bundle |
| i18n / 代码支持 / HTML 一等公民提案 | 向上游的兼容扩展草案 |

## 生态中值得先看的仓库（摘录）

| 仓库 | 形态要点 |
|------|----------|
| GoogleCloudPlatform/open-knowledge-format | 官方 SPEC v0.2 + PoC + 可视化 + 示例 bundle |
| GoogleCloudPlatform/knowledge-catalog | 规范与示例总入口（历史仓） |
| langchain-ai/openwiki | 为代码库写维护 agent 文档，产出 OKF v0.2 |
| iwe-org/iwe | Markdown 知识图谱：LSP + CLI + MCP，`iwe init --okf` |
| fellowgeek/mcp-memory | 以 OKF 为底的 Agent 长期记忆 MCP |
| killop/okf-rag | 本地优先 OKF 检索 / RAG |

完整列表以仓库 README「OKF 热门仓库」为准（持续更新）。

## 与现有知识库的关系

| 维度 | Obsidian 个人笔记（本库） | OKF bundle |
|------|---------------------------|------------|
| 形态 | 本地 vault + wikilink + 自有 frontmatter | 目录 Markdown + OKF 约定字段 |
| 目标读者 | 人 + 本库 AI 工作流 | 人 + 多 Agent / 跨工具互通 |
| 桥接 | `obsidian-to-okf` 可把 vault 导出为 OKF | 需要跨工具交换或 Agent 消费时再转 |

本库方法论笔记偏「如何在 Obsidian 里沉淀」；awesome-okf 偏「行业开放格式与互操作工具」。二者互补，不互相替代。

## 适用场景

- 想了解 **OKF 规范** 与中文资料，避免只啃英文 SPEC
- 需要把 **飞书 / Obsidian / Notion / GitHub / awesome 列表** 转成可校验的 OKF bundle
- 用 Claude Code 做「文档/仓库 → 知识包 → 网页/文档站」流水线
- 做 Agent 知识层、MCP 记忆、本地 RAG 时，需要一份 **生态索引** 快速选型
- 向 OKF 上游提 i18n / 代码锚点 / HTML 等扩展时，参考本仓提案写法

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/yzfly/awesome-okf |
| **英文 README** | https://github.com/yzfly/awesome-okf/blob/main/README.en.md |
| **OKF 规范中文版** | https://github.com/yzfly/awesome-okf/blob/main/docs/okf-spec-zh.md |
| **官方 SPEC（knowledge-catalog）** | https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md |
| **官方规范独立仓** | https://github.com/GoogleCloudPlatform/open-knowledge-format |
| **贡献说明** | https://github.com/yzfly/awesome-okf/blob/main/CONTRIBUTING.md |
| **许可证** | https://github.com/yzfly/awesome-okf/blob/main/LICENSE |
| **作者** | https://github.com/yzfly |

## My takeaways

1. **中文 OKF 入口仓**：价值 = 规范译文 + 可跑工具链 + 生态地图；选型「怎么把现有笔记/文档交给 Agent」时可从这里起步。
2. **与本库工作流对接点**：已有 Obsidian 沉淀体系时，不必整体迁到 OKF；需要跨工具交换或 Agent 标准消费时，用 `obsidian-to-okf` / `myokf-cli` 导出即可。
3. **生态热度在涨**：LangChain openwiki、iwe、各类 MCP/校验器/RAG 说明 OKF 正从「规范文档」走向「可交换知识包」；本仓的热门表适合当活索引。
4. **局限**：Stars 仍少、中文社区早期；规范演进（v0.2 破坏性字段）要注意工具与文档是否已迁移——本仓已声明完成迁移。

## Related

- [[30 知识资源/其他/Obsidian 个人知识库方法论|Obsidian 个人知识库方法论]]
- [[30 知识资源/其他/Obsidian 知识库自动沉淀工作流|Obsidian 知识库自动沉淀工作流]]
- [[40 知识导航/Obsidian 与 AI|Obsidian 与 AI]]
- [[50 来源资料/代码仓库/其他/Memos - 自托管轻量笔记工具|Memos - 自托管轻量笔记工具]]
- [[50 来源资料/代码仓库/AI/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]
- [[50 来源资料/代码仓库/AI/WeKnora - LLM 知识管理框架|WeKnora - LLM 知识管理框架]]
- [[50 来源资料/代码仓库/其他/awesome-selfhosted - 自托管软件精选列表|awesome-selfhosted - 自托管软件精选列表]]
