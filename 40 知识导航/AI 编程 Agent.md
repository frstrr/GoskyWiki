---
id: moc-ai-coding-agent
title: AI 编程 Agent
type: moc
status: active
created: 2026-06-18
updated: 2026-09-24
tags:
  - 导航/人工智能
  - 主题/AI编程Agent
aliases:
  - Coding Agent
  - AI Agent 编程工具
summary: AI 编程 Agent 主题导航，用于组织 Agent 框架对比、OpenCode 原理、编排机制、上下文管理和工具扩展相关笔记。
---

# AI 编程 Agent

## 主题说明

这个主题用于沉淀 AI 编程 Agent 的框架、编排机制、上下文管理、工具系统、权限控制和长期使用经验。

## 入门路径

- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]：先建立 OpenCode、Codex、Claude Code 的整体对比视角。
- [[30 知识资源/AI/OpenCode Agent 编排核心原理|OpenCode Agent 编排核心原理]]：再深入理解 OpenCode 的 Agent 循环、工具调用、Prompt 分层和上下文管理。

## 核心笔记

- [[30 知识资源/AI/主流 AI 编程 Agent 框架对比|主流 AI 编程 Agent 框架对比]]
- [[30 知识资源/AI/OpenCode Agent 编排核心原理|OpenCode Agent 编排核心原理]]
- [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
- [[30 知识资源/AI/复杂任务下 AI 连贯开发工作流|复杂任务下 AI 连贯开发工作流]]

## 来源资料

- [[50 来源资料/代码仓库/AI/mattpocock skills - AI编程Agent Skills|mattpocock skills - AI编程Agent Skills]]

- [[50 来源资料/代码仓库/AI/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]：obra 开源的 Agentic Skills 框架与完整 SDLC 方法论，279k+ Stars，支持 Cursor/Claude Code/Codex 等 15+ 平台

## 后续可扩展方向

- Agent 记忆系统
- Subagent 编排模式
- Prompt Caching 与上下文压缩
- 工具权限与安全策略
- OpenCode、Claude Code、Codex 的实际使用复盘


- [[50 来源资料/代码仓库/AI/Agent Substrate - AI Agent 大规模运行时|Agent Substrate - AI Agent 大规模运行时]]：Google 开源 Agent 大规模运行时，K8s 之上 30 倍超售、亚秒级挂起恢复

- [[50 来源资料/代码仓库/AI/JIT-Agent - 即时生成 Agent Harness|JIT-Agent - 即时生成 Agent Harness]]：按任务即时生成 agent harness（Model-as-a-Harness），四模块结构化 + 测试时进化


- [[50 来源资料/代码仓库/AI/Luvus - AI Agent 任务控制中心|Luvus - AI Agent 任务控制中心]]：跨平台 Rust 终端多路复用器，AI 编码 Agent 任务控制中心，持久 session + worktree 编排 + UHP 1.0


- [[50 来源资料/代码仓库/AI/PAXM - Coding Agent 中立记忆适配器|PAXM - Coding Agent 中立记忆适配器]]：本地优先、provider 中立的跨 Agent 记忆适配器（SQLite 默认；Codex/Claude/OpenCode/Cursor/MCP）