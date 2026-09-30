---
id: source-20260924-ms-cookbook
title: ms-cookbook - 魔搭紫皮书开源模型应用实战指南
type: source
status: active
created: 2026-09-24
updated: 2026-09-24
tags:
  - 来源/代码仓库
  - 主题/开源模型
  - 主题/微调
  - 主题/RAG
  - 主题/Agent
  - 主题/AIGC
  - 主题/ModelScope
  - 主题/实战教程
author: ModelScope / ms-cookbook-team
source_type: repo
source_url: https://github.com/modelscope/ms-cookbook
source_author: modelscope
source_date: 2026-09-24
summary: 魔搭紫皮书（ModelScope Cookbook）：面向开发者的开源模型应用实战指南，覆盖选型、推理、数据、微调、评测、RAG、Agent 与 AIGC；8 部分、35 章，配套 EvalScope / ms-swift / DiffSynth / Ollama 可复现示例。
related:
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层|RAGFlow]]"
  - "[[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/Vibe Coding CN - AI结对编程指南|Vibe Coding CN]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# ms-cookbook - 魔搭紫皮书开源模型应用实战指南

## Source summary

**ModelScope Cookbook（魔搭紫皮书）** 是阿里 ModelScope 开源的 **开源模型应用实战指南**，把模型选型、推理、数据准备、微调、评测与应用开发串成一条可学习、可复现的路径。目标是帮开发者从「跑通第一次推理」走到「能复现、能评测、能改进」的实际应用。

- GitHub：`modelscope/ms-cookbook`（Apache-2.0，约 **394 Stars**）
- 在线阅读：[ModelScope Studio · ms-cookbook](https://modelscope.cn/studios/ms-cookbook-team/ms-cookbook)（免安装，支持全文搜索、导读路径、章节导航、代码复制、公式渲染）
- 体量：**8 部分 · 35 章**（约 34 章可直接阅读）
- 配套工具链：**EvalScope**、**ms-swift**、**DiffSynth**、**Ollama**，以及 RAG / Agent 工作流

## 核心定位

| 维度 | 说明 |
|------|------|
| **定位** | 面向开发者的实战「紫皮书」，不是单一代码库，而是结构化学习资源 + 可运行示例 |
| **覆盖面** | 选型 → 推理 → 数据 → 微调/对齐 → 评测 → 业务应用 → AIGC → Agent |
| **阅读方式** | 优先在线 Studio；也可 clone 后 `python3 -m http.server` 本地阅读（无需项目依赖/构建） |
| **受众** | 想上手开源模型应用的开发者/学生；做选型、资源规划、微调、评测的应用工程师；做图像定制与生成的 AIGC 实践者 |

## 学习路径（官方推荐）

| 目标 | 推荐章节 | 练习重点 |
|------|----------|----------|
| **入门跑通** | 01 → 05 → 07 | 理解模型、定义任务、完成第一次推理 |
| **适配业务模型** | 12 → 13 → 15 | 训练数据准备、ms-swift 轻量微调、与基线对比评测 |
| **搭建应用** | 19 → 27 → 28 | 知识检索（RAG）、外部工具（MCP）、可复用 Skill |
| **生成式 AI** | 21 → 22 → 23 → 25 | AIGC case、DiffSynth 图像 LoRA、商品图、理论基础 |

## 全书结构（摘要）

| 部分 | 主题 | 代表章节 |
|------|------|----------|
| **Part 1** | 认识开源模型 | 开放与许可、下载与模型卡、数据基石、免费算力资源 |
| **Part 2** | 从业务到模型任务 | 任务定义、用 EvalScope 形成选型基线报告 |
| **Part 3** | 跑通第一个模型 | 30 分钟首推、服务器选型、Ollama 本机、云 Notebook、量化 |
| **Part 4** | 微调与评测 | 业务素材→训练数据、ms-swift 微调、偏好对齐、效果对比 |
| **Part 5** | 应用系统 | 健身教练、客服质检、语音助手、企业知识问答（RAG） |
| **Part 6** | 生成式 AI | AIGC case、DiffSynth LoRA、商品营销图、AI 视频、理论基础 |
| **Part 7** | Agents | Agent 概念、MCP、Skill、Claude Code / PI / DeepSeek Harness、产线巡检 Agent |
| **Part 8** | 补充基础 | 大模型基础知识、主流 LLM 评测共建 |

## 应用示例（可对照章节）

| 示例 | 工作流 | 章节 |
|------|--------|------|
| AI 健身教练 | 姿态关键点对比与动作纠偏 | 16 |
| 智能客服质检 | 通话转写与质检分析 | 17 |
| 语音助手 | ASR → 模型问答 → TTS | 18 |
| 企业知识问答 | 检索增强生成（RAG） | 19 |
| 商品营销图 | 生成与编辑 | 23 |
| 产线巡检 Agent | Penguin Harness 开发与评测优化 | 33 |

## 本地阅读（轻量）

```bash
git clone https://github.com/modelscope/ms-cookbook.git
cd ms-cookbook
python3 -m http.server 4173 --bind 127.0.0.1
```

浏览器打开 `http://127.0.0.1:4173/#home`。仅需 Python 3 + 现代浏览器；章节练习本身可能另需模型下载、密钥或算力。

## 适用场景

- 需要 **系统化学习开源模型落地**（选型/推理/微调/评测/应用），而不是零散博客
- 想用 **ModelScope 生态工具**（EvalScope、ms-swift、DiffSynth）按章节复现
- 计划做 **企业知识问答、客服质检、语音助手、AIGC 商品图、产线巡检 Agent** 等场景原型
- 希望有 **在线可读 + 本地可镜像** 的中文实战教材，方便团队内部分享

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/modelscope/ms-cookbook |
| **在线阅读（ModelScope Studio）** | https://modelscope.cn/studios/ms-cookbook-team/ms-cookbook |
| **Issues** | https://github.com/modelscope/ms-cookbook/issues |
| **Pull Requests** | https://github.com/modelscope/ms-cookbook/pulls |
| **贡献说明** | https://github.com/modelscope/ms-cookbook/blob/main/CONTRIBUTING.md |
| **ModelScope 开发者实践（投稿话题 #魔搭紫皮书）** | https://modelscope.cn/spotlight |
| **贡献者列表** | https://github.com/modelscope/ms-cookbook/graphs/contributors |
| **README 结构参考：Hello-Agents** | https://github.com/datawhalechina/hello-agents |
| **License** | Apache License 2.0 |

## My takeaways

1. **定位是「开源模型应用教科书」**：把选型、推理、微调、评测、RAG、Agent、AIGC 串成连续路径，适合作为团队内部 onboarding / 选型手册入口。
2. **与纯代码框架互补**：本身以章节教程为主，落地时再对接 EvalScope / ms-swift / DiffSynth / Ollama 等工具；做企业知识问答时可对照 RAGFlow、WeKnora 做产品化选型。
3. **Agent 部分已跟进 MCP / Skill / 多种 Harness**：对要搭工具调用与可复用能力封装的实践者有直接参考价值。
4. **阅读门槛低、复现门槛按章递增**：在线/本地都能读；真正跑通章节示例仍要按环境要求准备模型与算力。
5. **内容以中文章节为主、GitHub 为源**：改内容按仓库说明应改 content/source-html/ 再构建；引用模型/数据集/工具各自许可证需单独核对。

## Related

- [[50 来源资料/代码仓库/AI/RAG知识与记忆/RAGFlow - 开源RAG引擎与Agent上下文层|RAGFlow - 开源 RAG 引擎与 Agent 上下文层]]
- [[50 来源资料/代码仓库/AI/RAG知识与记忆/WeKnora - LLM 知识管理框架|WeKnora - LLM 知识管理框架]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/Vibe Coding CN - AI结对编程指南|Vibe Coding CN - AI 结对编程指南]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
