---
id: source-20260831-jit-agent
title: JIT-Agent - 即时生成 Agent Harness
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/Agent框架
author: Guibin Zhang 等（论文作者团队）
source_type: repo
source_url: https://github.com/bingreeky/JIT
source_author: bingreeky
source_date: 2026-08-31
summary: 紧凑型 meta-agent，按任务即时生成可执行的 task-specific agent harness（Model-as-a-Harness），通过 memory/planning/action/capability 四模块结构化生成，测试时持续进化 harness 而 meta 模型本身冻结。
related:
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]"
---

# JIT-Agent - 即时生成 Agent Harness

## Source summary

**JIT-Agent**（Just-in-Time Agent）是一个 **紧凑型 meta-agent**，核心理念是 **Model-as-a-Harness**——不预先编译一个通用 scaffold 再指望它迁移，而是根据任务规格、协议、工具/技能注册表和少量检索到的 prior harness，**即时生成可执行的、任务专属的 agent harness**，包裹任意现成的 agentic LLM。

- 开源项目，GitHub：[bingreeky/JIT](https://github.com/bingreeky/JIT)
- 语言：**Python 3.11**；约 **200 stars**（2026-08-31）
- 论文：[arXiv:2608.25593](https://arxiv.org/abs/2608.25593)
- 模型权重：[Hugging Face - JIT-Agent](https://huggingface.co/JIT-Agent)（含 **JIT-Agent-27B** checkpoint）
- 核心结论：**构建 scaffold 本身是可训练、可迁移的智能维度——与扩大 base model 正交**

## 解决什么问题

传统 Agent 框架通常 **预编译一个通用 harness**（固定的 memory/planning/action 编排），再期望它跨任务迁移。JIT-Agent 认为：

| 方式 | 问题 |
|------|------|
| 固定通用 scaffold | 不同任务结构差异大，一套编排难以最优 |
| 自由形式生成 agent 程序 | 不可控、难评估、难迭代 |
| 只扩 base model | 忽略「如何组织 Agent 运行时」这一独立智能轴 |

JIT-Agent 的思路：**按任务即时合成 harness**，生成的是结构化代码（非自由文本程序），并在测试时根据 trace 和 feedback 修订 harness、更新 archive——**harness 持续进化，meta 生成器本身冻结**。

## 核心能力

| 能力 | 说明 |
|------|------|
| **Just-in-Time Harness 生成** | 输入 task spec + protocol + tool registry + prior harness，输出可执行 harness |
| **四模块结构化** | memory / planning / action / capability orchestration，基于 HarnessFactory 共享接口 |
| **Model-as-a-Harness** | 生成的 harness 包裹任意 off-the-shelf agentic LLM |
| **测试时进化** | trace 和 feedback 回流，修订 harness 并更新 archive |
| **Best-of-N 选择** | 多候选 harness，用 judge 或 logprob 选择最优 |
| **多 benchmark 评估** | xbench、deepsearchqa、agentif、officebench、odyssey、shopping、travel |

## 架构与目录

```
JIT-Agent 流水线：
  Meta Model → 生成 N 个候选 harness → 选择器（judge / logprob）→ 执行模型跑 harness → Judge 评分

Harness 四模块（HarnessFactory 接口）：
  memory → planning → action → capability orchestration
```

| 目录 | 内容 |
|------|------|
| [`jit/`](https://github.com/bingreeky/JIT/tree/main/jit) | meta agent：生成/修复 prompt、best-of-N 选择 |
| [`scripts/`](https://github.com/bingreeky/JIT/tree/main/scripts) | agent kernel、工具、模型、评估引擎、两个 runner |
| [`harness_factory/`](https://github.com/bingreeky/JIT/tree/main/harness_factory) | 手写 harness 实现及设计文档（含 11 种 seed design） |
| [`benchmark/`](https://github.com/bingreeky/JIT/tree/main/benchmark) | 各 benchmark 的 adapter、config、evaluator |
| [`dataset/`](https://github.com/bingreeky/JIT/tree/main/dataset) | benchmark 数据 |

## 三种运行模式

| 目标 | 入口 | Meta model | 选择方式 |
|------|------|------------|----------|
| 测试固定 HarnessFactory 设计 | `scripts.run_seed_harness` | 无 | 无 |
| 用托管 API 作 meta-agent | `scripts.run_jit` | OpenAI 兼容 API | `judge` |
| 评估 JIT checkpoint | `serve_meta_model.sh` + `scripts.run_jit` | 本地 JIT-27B | `logprob` |

## 快速开始

```bash
git clone https://github.com/bingreeky/JIT.git
cd JIT
conda env create -f environment.yml && conda activate jit
cp .env.example .env   # 填写 API keys
python scripts/check_datasets.py
```

**环境变量分组：**

| 分组 | Keys | 用途 |
|------|------|------|
| 执行模型 | `OPENAI_API_BASE`, `OPENAI_API_KEY`, `EXEC_MODEL` | 跑生成的 harness agent loop |
| Judge 模型 | `JUDGE_MODEL`, `JUDGE_API_*` | 评分产出物 |
| Meta 模型 | `META_MODEL`, `META_API_BASE`, `META_API_KEY`, `META_TOKENIZER` | 写 harness（仅 JIT 流水线） |
| 工具 | `SERPER_API_KEY`, `JINA_API_KEY` | web_search / crawl_page |

**示例命令：**

```bash
# 测试固定 harness 设计
python -m scripts.run_seed_harness --bench xbench --list-harnesses
python -m scripts.run_seed_harness --bench xbench --harness plan_and_execute --max-samples 5

# 托管 API 作 meta-agent
python -m scripts.run_jit --bench xbench \
    --meta-model provider-model --meta-base https://api.provider.com/v1 \
    --selector judge --rollouts 3 --max-samples 5

# 本地 JIT-27B checkpoint
MODEL=JIT-Agent/jit-27b SERVED_NAME=jit TP=4 bash scripts/serve_meta_model.sh
python -m scripts.run_jit --bench xbench \
    --meta-model jit --meta-base http://127.0.0.1:8000/v1 \
    --selector logprob --tokenizer JIT-Agent/jit-27b \
    --rollouts 3 --meta-temperature 1.0 --max-samples 5
```

## 支持的 Benchmark

`xbench`, `deepsearchqa`, `agentif`, `officebench`, `odyssey`, `shopping`, `travel`

输出结构：`summary.json`、`generate/`（N 候选）、`select/`（选择结果）、`execute/`（实际运行轨迹）。相同命令可 resume，支持 `--skip-generate` / `--skip-select`。

## 适用场景

- **研究/实验**：探索「harness 智能」作为独立于 base model 的可训练维度
- **多任务 Agent 评估**：需要在 deep research、daily work、planning、workspace 等场景对比不同 harness 设计
- **自定义 harness 工厂**：基于 HarnessFactory 接口扩展 memory/planning/action/capability 模块
- **复现论文结果**：JIT-Agent-27B 在 Hugging Face 可直接拉取

**不太适合**：只想快速集成一个固定 Agent 框架做产品（上手需配置 meta/exec/judge 三模型 + benchmark 数据）。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/bingreeky/JIT |
| **论文 (arXiv)** | https://arxiv.org/abs/2608.25593 |
| **Hugging Face 模型** | https://huggingface.co/JIT-Agent |
| **JIT pipeline 文档** | https://github.com/bingreeky/JIT/blob/main/jit/README.md |
| **HarnessFactory 指南** | https://github.com/bingreeky/JIT/blob/main/harness_factory/README.md |
| **CLI 与运行时** | https://github.com/bingreeky/JIT/blob/main/scripts/README.md |

## My takeaways

1. **新维度**：把「如何组织 Agent 运行时」从 base model 能力中解耦，作为可单独训练和迁移的智能轴——对 Agent 框架设计有启发。
2. **结构化生成**：通过 HarnessFactory 四模块接口约束输出为代码而非自由文本，比纯 prompt 生成 agent 更可控、可评估。
3. **测试时进化**：harness 在运行时根据 feedback 修订，meta 模型冻结——类似 test-time compute 思路在 scaffold 层的应用。
4. **工程完整度较高**：含 11 种 seed harness、7 个 benchmark、完整 generate/select/execute 流水线，适合作为 research codebase 参考。
5. **与 unicore 修复分支的关联**：当前工作区 repair 分支名为 `auto-fix/zhaorun/jit-agent-framework`，可能与 JIT-Agent 的 harness 框架思路相关，值得对照 HarnessFactory 设计。

## Related

- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
- [[50 来源资料/代码仓库/AI/Agent Substrate - AI Agent 大规模运行时|Agent Substrate - AI Agent 大规模运行时]]
