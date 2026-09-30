---
id: source-20260831-voxcpm
title: VoxCPM - 语音克隆 TTS
type: source
status: active
created: 2026-08-31
updated: 2026-08-31
tags:
  - 来源/代码仓库
  - 主题/语音合成
  - 主题/TTS
  - 主题/AI教育
author: OpenBMB
source_type: repo
source_url: https://github.com/OpenBMB/VoxCPM
source_author: OpenBMB
source_date: 2026-08-31
summary: OpenBMB 开源的 TTS 语音合成与声音克隆模型（VoxCPM2），支持自托管部署和 on-the-fly 自动生成声音。OpenMAIC 集成用于课件 AI 配音，开源版支持用用户自己的声音讲解。
related:
  - "[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenMAIC - 开源多智能体交互课堂|OpenMAIC - 开源多智能体交互课堂]]"
---

# VoxCPM - 语音克隆 TTS

## Source summary

**VoxCPM**（VoxCPM2）是 OpenBMB 开源的 **TTS 语音合成与声音克隆**模型。OpenMAIC 在 v0.2.1 起集成 VoxCPM2，用于课件 AI 配音；开源版还支持**声音克隆**——讲课件可以用用户自己的声音。

- GitHub：`OpenBMB/VoxCPM`
- 集成方：[[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenMAIC - 开源多智能体交互课堂|OpenMAIC]]
- 部署方式：自托管（self-hosted TTS），无需第三方 TTS API

## 核心能力

| 能力 | 说明 |
|------|------|
| **TTS 语音合成** | 将文本转为自然语音，用于课件讲解配音 |
| **声音克隆** | 用少量样本克隆用户声音，课件用「自己的声音」讲解 |
| **On-the-fly 自动生成** | 无需预录，运行时自动生成合适的声音 |
| **自托管** | 数据不出内网，适合学校/企业本地部署 OpenMAIC 的场景 |

## 在 OpenMAIC 中的用途

- 课件每页自动生成讲解旁白（TTS）
- 开源版支持 VoxCPM 声音克隆：用户录一段样本 → 后续课件用克隆声音讲解
- OpenMAIC 文档：https://open.maic.chat/docs/deployment 中有 VoxCPM2 配置说明

## 适用场景

- 部署 OpenMAIC 且希望课件配音不依赖云端 TTS API
- 需要**个性化声音**（教师用自己的声音讲 AI 生成的课件）
- 数据敏感场景，TTS 推理也在本地完成

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/OpenBMB/VoxCPM |
| **OpenMAIC VoxCPM2 文档** | https://open.maic.chat/docs/deployment |
| **OpenMAIC 集成说明** | OpenMAIC README v0.2.1 changelog |

## My takeaways

1. **本地化 TTS 是 OpenMAIC 本地部署的重要拼图**：配合 Ollama/Lemonade 本地 LLM，可实现完全内网 AI 教育链路。
2. **声音克隆提升个性化**：教师用自己声音讲 AI 生成内容，降低「AI 感」，更适合正式教学场景。
3. **OpenBMB 生态**：与 MiniCPM 等模型同团队，TTS 质量有学术团队背书。

## Related

- [[50 来源资料/代码仓库/AI/Agent运行时与编排/OpenMAIC - 开源多智能体交互课堂|OpenMAIC - 开源多智能体交互课堂]]
