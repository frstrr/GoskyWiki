---
id: source-20261019-open-knowledge-format
title: open-knowledge-format - 开放知识格式 OKF
type: source
status: active
created: 2026-10-19
updated: 2026-10-19
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/知识库
  - 主题/知识格式
  - 主题/个人知识管理
  - 工具/Obsidian
author: GoogleCloudPlatform
source_type: repo
source_url: https://github.com/GoogleCloudPlatform/open-knowledge-format
source_author: Google Cloud Platform
source_date: 2026-10-19
summary: Google Cloud 发布的厂商中立开放知识格式（OKF v0.2）：用目录内 Markdown + YAML frontmatter 表示可版本控制、可交换的知识包；含出处/信任/生命周期/可验算计算约定，附参考 Agent（BigQuery→OKF）与单文件图谱可视化。Apache-2.0，约 1k Stars。
related:
  - "[[50 来源资料/代码仓库/其他/awesome-okf - 开放知识格式中文资料与工具|awesome-okf]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/Synthadoc - LLM 知识编译引擎|Synthadoc]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace]]"
  - "[[40 知识导航/Obsidian 与 AI|Obsidian 与 AI]]"
  - "[[30 知识资源/其他/Obsidian 个人知识库方法论|Obsidian 个人知识库方法论]]"
---

# open-knowledge-format - 开放知识格式 OKF

## Source summary

**Open Knowledge Format（OKF）** 是 Google Cloud 发布的**通用、厂商中立**知识表示格式：把知识写成「目录里的 Markdown 文件 + YAML frontmatter」，不绑定任何特定 Agent、框架、模型厂商或服务系统。

- GitHub：`GoogleCloudPlatform/open-knowledge-format`（**Apache-2.0**，约 **1018** Stars / **67** Forks）
- 规范版本：**OKF v0.2**（独立仓；2026-08 自 `knowledge-catalog` 拆出）
- 定位一句话：**任何人都能生产、任何人都能消费**的可 git 版本控制知识包（bundle）
- 仓库组成：正式规范 `SPEC.md` + 参考生产 Agent（BigQuery 元数据 → OKF）+ 单文件可视化消费者 + 多套示例 bundle

与「某个知识库产品」不同：OKF 本身**没有运行时、没有必选 SDK**。能 `cat` 就能读；能 `git clone` 就能分发。

## 为什么需要 OKF

相对服务方私有元数据仓，纯文件 + frontmatter 带来：

| 特性 | 说明 |
|------|------|
| **人与 Agent 同读** | 无需 SDK/查询语言；工程师可直接读，LLM 可整文件进上下文 |
| **天然可版本控制** | bundle 进 git：PR、diff、blame、评审与代码一样 |
| **可移植、无锁定** | 一个目录即可 tar/挂载/同步；无专有 API 挡在中间 |
| **结构 + 非结构并存** | frontmatter 管可查询字段；正文管人/模型真正读的说明、schema、示例 |
| **信任与新鲜度一等公民** | v0.2 把出处、可信层级、生命周期写进可查询头信息 |
| **最小约定、可扩展** | 必填很少，额外键与正文段落不破坏消费者 |
| **兼容现有工具** | Obsidian / Notion / MkDocs / Hugo 等已懂 Markdown+YAML |
| **渐进披露** | 自动/约定的 `index.md` 让 Agent 按层导航，而非一次吞整库 |
| **图状关系** | 概念间用普通 Markdown 链接表达，不止目录父子树 |

## 规范要点（v0.2）

### Bundle 结构

```
bundle/
  index.md          # 可选：目录列表，渐进披露
  log.md            # 可选：更新历史
  <concept>.md
  <subdir>/
    index.md
    <concept>.md
```

- **Knowledge Bundle**：可分发的知识包（目录 / git / tar）
- **Concept**：一条知识 = 一个 `.md`；Concept ID = 相对路径去掉 `.md`
- 保留名：`index.md`、`log.md` 不可当概念文档用

### Frontmatter 核心

- **唯一始终必填**：`type`（如 `BigQuery Table`、`Metric`、`Reference`、`Attested Computation`…；类型不中央注册）
- **推荐**：`title`、`description`、`resource`、`tags`
- **出处**：`sources[]`（`resource` / `id` / 可信信号 `author` / `usage_count` / `last_modified`）
- **生成与确认**：`generated`、`verified` → 推导信任层级（未验证 / 机器确认 / 人工审阅）
- **生命周期**：`status`、`stale_after` 等
- **可验算计算（Attested Computation）**：概念可携带「官方计算方式」，消费者用 executor/attester 核对数值是否按约定产出

### 设计非目标

不规定概念类型taxonomy、不规定存储/服务/查询基建、不替代 Avro/Protobuf/OpenAPI 等领域 schema（只引用）、不规定 executor 的打包与调用标准。

## 仓库附带能力（PoC）

仓库贡献的核心是**格式本身**；附带组件只是把生产端与消费端做「看得见」：

### 参考 Agent（producer）

两遍流水线：

1. **BQ pass**：按 BigQuery 元数据为每个概念写一篇 OKF
2. **web pass**：LLM 按 seed URL 抓取权威文档，选择丰富已有概念 / 新建 `references/` / 跳过（有页数上限与同域白名单）

```bash
python3.13 -m venv .venv
.venv/bin/pip install --index-url https://pypi.org/simple/ -e ".[dev]"

.venv/bin/python -m reference_agent enrich \
    --source bq \
    --dataset <project>.<dataset> \
    --web-seed-file <path/to/seeds.txt> \
    --out ./bundles/<name>
```

凭证：BigQuery 用 `gcloud auth application-default login`；Gemini 用 `GEMINI_API_KEY` 或 Vertex AI 环境变量。

### 可视化（consumer）

`visualize` 把任意 OKF bundle 渲成**单文件交互 HTML**（无后端）：力导向图谱、详情面板、反向链接、搜索与类型过滤。

```bash
.venv/bin/python -m reference_agent visualize --bundle ./bundles/<name>
```

### 示例 bundle（可直接打开）

| Bundle | 内容 |
|--------|------|
| [`bundles/ga4/`](https://github.com/GoogleCloudPlatform/open-knowledge-format/tree/main/bundles/ga4) | GA4 电商数据集 + viz |
| [`bundles/stackoverflow/`](https://github.com/GoogleCloudPlatform/open-knowledge-format/tree/main/bundles/stackoverflow) | Stack Overflow 公开数据集 |
| [`bundles/crypto_bitcoin/`](https://github.com/GoogleCloudPlatform/open-knowledge-format/tree/main/bundles/crypto_bitcoin) | Bitcoin 区块/交易 |
| [`bundles/acme_retail/`](https://github.com/GoogleCloudPlatform/open-knowledge-format/tree/main/bundles/acme_retail) | Acme Retail 示例 |

各示例另有 `samples/<name>/` recipe（seed URL + 精确 enrich 命令）便于复现。

## 适用场景

- 要给 **Agent / 人 / 多工具** 共享同一份可 diff 的知识制品，而不是只塞进私有向量库
- 数据目录、指标说明、运维 playbook、API/表元数据需要 **出处 + 信任 + 过期** 可查询
- 希望知识包能被 Obsidian / MkDocs / 静态站 / LLM 上下文 **直接消费**
- 从 BigQuery（或类似目录）**自动导出**再叠加网页文档富化
- 选型 OKF 生态前，先读官方 SPEC 与示例 bundle（中文入口见 awesome-okf）

## 与本库及相关资源

| 资源 | 关系 |
|------|------|
| **本库（Obsidian 个人笔记）** | 同为 Markdown + frontmatter；字段约定不同。需要跨工具/Agent 标准交换时再导出为 OKF |
| [[50 来源资料/代码仓库/其他/awesome-okf - 开放知识格式中文资料与工具\|awesome-okf]] | 中文规范译文、转换插件、Skills、生态索引；推荐中文侧入口 |
| [[50 来源资料/代码仓库/AI/RAG知识与记忆/Synthadoc - LLM 知识编译引擎\|Synthadoc]] | ingest-time 编译本地 Wiki；可导出 OKF（注意版本对齐） |
| 历史仓 `knowledge-catalog` | 规范早期落点；正式 SPEC 已迁到本仓 |

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/GoogleCloudPlatform/open-knowledge-format |
| **规范 SPEC v0.2** | https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/SPEC.md |
| **示例 bundles** | https://github.com/GoogleCloudPlatform/open-knowledge-format/tree/main/bundles |
| **复现 recipes** | https://github.com/GoogleCloudPlatform/open-knowledge-format/tree/main/samples |
| **历史/总入口仓** | https://github.com/GoogleCloudPlatform/knowledge-catalog |
| **中文资料与工具** | https://github.com/yzfly/awesome-okf |
| **许可证** | https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/LICENSE |

## My takeaways

1. **先认清贡献物是格式，不是产品**：选型时问「要不要可交换的 Markdown 知识包」；Agent/可视化只是示范两端。
2. **与本库互补**：日常仍用 Obsidian 自有元数据；OKF 适合「要交给外部 Agent / 跨组织交换 / 进 git 评审」的那一层。
3. **v0.2 的价值在信任语义**：`sources` / `generated` / `verified` / `stale_after` / Attested Computation，比「纯 Markdown 文件夹」更适合 Agent 持续维护的语料。
4. **落地路径**：读 SPEC → 打开 `bundles/*/viz.html` 感受消费端 → 需要中文工具链再走 [[50 来源资料/代码仓库/其他/awesome-okf - 开放知识格式中文资料与工具|awesome-okf]]。

## Related

- [[50 来源资料/代码仓库/其他/awesome-okf - 开放知识格式中文资料与工具|awesome-okf - 开放知识格式中文资料与工具]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/Synthadoc - LLM 知识编译引擎|Synthadoc - LLM 知识编译引擎]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora - LLM 知识管理框架]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/MemPalace - 本地优先 AI 记忆系统|MemPalace - 本地优先 AI 记忆系统]]
- [[40 知识导航/Obsidian 与 AI|Obsidian 与 AI]]
- [[30 知识资源/其他/Obsidian 个人知识库方法论|Obsidian 个人知识库方法论]]
- [[30 知识资源/其他/Obsidian 知识库自动沉淀工作流|Obsidian 知识库自动沉淀工作流]]
