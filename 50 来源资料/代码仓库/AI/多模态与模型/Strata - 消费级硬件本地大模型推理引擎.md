---
id: source-20261103-strata
title: Strata - 消费级硬件本地大模型推理引擎
type: source
status: active
created: 2026-11-03
updated: 2026-11-03
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/本地推理
  - 主题/大语言模型
  - 主题/DeepSeek
  - 主题/Qwen
  - 主题/MoE
  - 主题/开源资源
author: Niko1221
source_type: repo
source_url: https://github.com/Niko1221/Strata
source_author: Niko1221
source_date: 2026-09-24
summary: 开源本地 AI 推理引擎 Strata：一键安装桌面端/终端，在消费级硬件上跑 Qwen3.8-Flash-Next、DeepSeek V4.1-Flash 等大模型；热门专家缓存 + SSD 流式加载，提供本机 OpenAI/Anthropic 兼容 API，可对接 Claude Code、Codex、OpenCode 等工具。
related:
  - "[[50 来源资料/代码仓库/AI/多模态与模型/MiniCPM - 端侧大语言模型|MiniCPM - 端侧大语言模型]]"
  - "[[50 来源资料/代码仓库/AI/多模态与模型/ms-cookbook - 魔搭紫皮书开源模型应用实战指南|ms-cookbook - 魔搭紫皮书开源模型应用实战指南]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
aliases:
  - Niko1221/Strata
  - Strata inference engine
  - strataengine
---

# Strata - 消费级硬件本地大模型推理引擎

## Source summary

**Strata**（仓库 [Niko1221/Strata](https://github.com/Niko1221/Strata)）是面向消费级电脑的 **开源本地大模型推理引擎**：提供桌面 App 与终端 CLI，一键安装后即可在本机聊天、写代码、处理图片，并通过 **OpenAI / Anthropic 兼容 API**（默认 `http://127.0.0.1:8080`）对接各类 Agent / IDE 工具。无需账号；MIT 许可。

- 约 **38.5k+ stars / 4.2k+ forks**（截至 2026-11-03 GitHub API）；主语言 **C++**；许可证 **MIT**
- 最新发布：**v0.2.10**（2026-11-02）
- 官网发行说明：[stratat.com releases](https://www.stratat.com/en/releases/v0.2.4)
- 中文 README：[README.zh-CN.md](https://github.com/Niko1221/Strata/blob/main/README.zh-CN.md)
- 来源线索：今日头条文章《12GB显卡本地跑DeepSeek V4.1-Flash-1017，Strata 0.2.4彻底封神？》（[文章链接](https://www.toutiao.com/article/7700409766661440009/)）

核心一句话：**热门专家常驻内存/显存 + 其余权重从 SSD 流式加载，让 12GB 显卡也能“够得着”500B 级 MoE。**

## 核心特性

| 特性 | 说明 |
|------|------|
| **一键安装** | 桌面安装包（Win/Linux/Mac）或终端一行脚本；首次启动按显卡下载对应引擎 |
| **双入口** | 桌面 GUI 聊天（含文件与实时代码预览）+ `strata launch` / `strata setup` 终端向导 |
| **专家缓存 + SSD 流式** | MoE 高频专家驻留内存（显存允许时进 GPU），其余按需从高速 NVMe 读取 |
| **本机兼容 API** | OpenAI 风格 `http://127.0.0.1:8080/v1`；Anthropic 风格基址通常为 `http://127.0.0.1:8080` |
| **工具对接** | `strata connect` 输出 Claude Code、Codex CLI、OpenCode、Hermes Agent、OpenClaw 等接入说明 |
| **可选图像输入** | README 标明支持本地图像相关工作流（具体能力因模型/版本而异） |
| **无遥测、开源** | 免费 MIT；强调本地运行、自有数据 |

## 支持的模型（摘录）

| 模型族 | 要点 |
|--------|------|
| **Qwen3.8-Flash-Next** | 主力推荐；ISTA-DASLab / Unsloth / Gyro 等多量化档；有 Coder 专用包 |
| **Qwen3.6-35B-A3B** | Unsloth UD-Q4_K_XL / UD-Q6_K_XL / Q8_0 等 |
| **DeepSeek V4.1-Flash-1017** | 约 552B 总参数 MoE；ISTA-DAS 1.75/2.00/2.25 bpw（约 239～274 GB）；内存放不下的部分读 SSD |

DeepSeek 快速启动示例：

```sh
strata launch --family deepseek41 --model ist-das-2.00bpw --plan
```

不确定选哪个？官方有按内存选型指南：[docs/MODELS.md](https://github.com/Niko1221/Strata/blob/main/docs/MODELS.md)

## 硬件门槛（DeepSeek 场景）

文章与官方 release 对齐的大致门槛：

| 项 | 建议 |
|----|------|
| 显存 | **≥ 12GB**（RTX 30/40/50 等） |
| 内存 | 实测可用约 **46GB**，推荐 **64GB+** |
| 磁盘 | 至少约 **239～280GB** 空闲，须 **高速 NVMe SSD**（机械盘基本不可用） |
| 其他平台 | AMD Radeon AI PRO R9700、Ryzen AI Max / Strix Halo、Apple Silicon（DeepSeek 有单独 Mac 文档）等，见官方 Hardware 表 |

官方参考速度（IQ2_XXS / 2.00 bpw 量级，随版本微调）：RTX 3060 12GB + 46GB RAM 约 **8～11 tok/s** 输出；多卡 R9700 或大显存工作站可到数十～上百 tok/s。**12GB 消费级配置更适合验证与轻量试用**；连续 Agent 工作流通常需要更高显存驻留专家比例。

## 安装

**桌面：** 从 [Releases](https://github.com/Niko1221/Strata/releases/latest) 下载对应系统安装包。

**终端：**

```sh
# Linux / Mac
curl -fsSL https://github.com/Niko1221/Strata/releases/latest/download/install.sh -o install.sh && sh install.sh

# Windows PowerShell
irm https://github.com/Niko1221/Strata/releases/latest/download/install.ps1 | iex
```

安装后执行 `strata launch`，按向导下载模型与引擎。另有源码构建、Docker 文档。

## 关键链接

| 资源 | URL |
|------|-----|
| GitHub 仓库 | https://github.com/Niko1221/Strata |
| 最新 Release | https://github.com/Niko1221/Strata/releases/latest |
| 发行说明站 | https://www.stratat.com/en/releases/v0.2.4 |
| 中文 README | https://github.com/Niko1221/Strata/blob/main/README.zh-CN.md |
| DeepSeek 文档 | https://github.com/Niko1221/Strata/blob/main/docs/DEEPSEEK41.md |
| 工作原理 | https://github.com/Niko1221/Strata/blob/main/docs/HOW_IT_WORKS.md |
| 故障排查 | https://github.com/Niko1221/Strata/blob/main/docs/TROUBLESHOOTING.md |
| Agent 技术参考 | https://github.com/Niko1221/Strata/blob/main/AGENTS.md |
| Buy Me a Coffee | https://buymeacoffee.com/strataengine |
| 头条来源文 | https://www.toutiao.com/article/7700409766661440009/ |

## 适用场景

- 想在 **消费级 GPU** 上本地跑 Qwen / DeepSeek 等较大模型，并暴露兼容 API 给编码 Agent
- 需要 **一键安装 + 桌面聊天**，而不是手搓 llama.cpp / vLLM 全套环境
- 关注 **MoE + SSD 流式** 工程路线，评估本地千亿级模型的可行性与成本结构

## 限制与注意

- DeepSeek 体量极大：磁盘与内存成本往往高于「12GB 显卡」本身；消费级 8～11 tok/s 多半只够聊天/验证
- 文章所述 0.2.4 时期 DeepSeek 曾 **仅终端可用、桌面仍停在 0.2.3**；后续版本（当前已至 0.2.10）README 已写明桌面+终端均支持，落地前以最新 Release / README 为准
- 速度高度依赖 **内存带宽、专家驻留量、NVMe 随机读**；同显存不同内存/SSD 可相差一个数量级
- 模型权重与引擎包体积巨大，需可信来源下载并核对签名/哈希（官方 release 提供校验）

## 文章要点备忘（v0.2.4 语境）

- 2026-10-24 发布 0.2.4 / 终端包 0.2.4.1，正式加入 DeepSeek V4.1-Flash-1017
- 三个 GSQ-RCO 量化档：IQ1_M / IQ2_XXS / IQ2_XS；官方更推荐有磁盘余量时用更高质量档做编程与 agent
- 工程意义：把「显存够不够」改写为「内存 + SSD + CPU 调度」共同决定能否跑、跑多快
