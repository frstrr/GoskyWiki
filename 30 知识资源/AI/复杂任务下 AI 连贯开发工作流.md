---
id: evergreen-ai-continuous-dev-workflow
title: 复杂任务下 AI 连贯开发工作流
type: evergreen
status: evergreen
created: 2026-04-10
updated: 2026-06-18
tags:
  - 知识/人工智能
  - 主题/AI编程Agent
  - 主题/工作流
aliases:
  - AI 连续开发工作流
  - AI 复杂任务 handoff
summary: 复杂任务可以持续交给 AI 推进，但不应依赖超长聊天历史维持连续性，而应通过阶段划分、状态摘要、脚本化验证和 handoff note，把连贯性沉淀到代码与文档中。
moc:
  - [[40 知识导航/AI 工具使用]]
  - [[40 知识导航/工作流]]
source:
  - [[30 知识资源/AI/Claude API Prompt Caching 失效根因与优化策略|Claude API Prompt Caching 失效根因与优化策略]]
related:
  - [[30 知识资源/AI/AI编程Agent Skills方法论|AI编程Agent Skills方法论]]
  - [[30 知识资源/其他/Obsidian 知识库自动沉淀工作流|Obsidian 知识库自动沉淀工作流]]
---

# 复杂任务下 AI 连贯开发工作流

## TL;DR

复杂任务可以连贯开发，但不要让 AI 持续背负完整原始对话历史。应该把原始过程压缩成阶段性状态，让连续性来自代码、测试和 handoff note，而不是聊天记录本身。

## Key points

- 连贯性和上下文体积要分开管理。
- 每个阶段结束后，保留状态摘要，不保留大段原始工具输出。
- 高频验证应脚本化，避免在对话里插入大量动态内容。
- 新会话应从 handoff note 或阶段摘要恢复，而不是依赖完整历史。

## Notes

### 为什么复杂任务仍然可以连贯推进

真正可靠的连续性来源是：

- 当前代码。
- 测试结果。
- 设计文档。
- 阶段性状态摘要。
- handoff note。

聊天历史适合推动当前一步，但不适合作为长期状态存储。

### 推荐工作流

1. 开始一个阶段前，明确当前目标、范围和约束。
2. 让 AI 修改代码或补文档。
3. 运行统一的验证脚本。
4. 把验证结果压缩成 5 到 10 行状态摘要。
5. 将摘要写入 handoff note。
6. 下一会话从 handoff note 接着做。

### 阶段划分方式

复杂任务建议至少拆成以下阶段：

- 问题定位和方案设计。
- 主逻辑实现。
- 测试补齐和边角修复。
- 回归验证和收尾。

每完成一个阶段后开新会话，可以保留目标连续性，同时避免完整历史不断膨胀。

### 状态摘要应该写什么

建议只保留高价值状态：

- 已完成内容。
- 已修改文件。
- 当前验证结论。
- 未解决问题。
- 下一步动作。

示例：

```md
当前状态：
- 已完成：重构 `InventoryService` 的状态同步逻辑
- 已修改文件：
  - `src/inventory/service.ts`
  - `src/inventory/service.test.ts`
- 当前验证结果：
  - 单测通过
  - 集成测试 1 个失败：`should sync after reconnect`
- 怀疑原因：reconnect 后缓存未清理
- 下一步：检查 reconnect handler 和 cache invalidation
```

### 工具输出如何压缩

不要把完整 `git diff`、编译日志、测试 JSON 长期带入后续会话，只保留：

- 哪些文件变化了。
- 哪个测试失败了。
- 失败的错误类型是什么。
- 当前结论是什么。

示例：

```md
验证结论：
- `npm test`：23 通过，1 失败
- 失败用例：`sync after reconnect`
- 错误：`expected cache size 0, received 1`
```

### 脚本化验证

把高频操作合并成一个命令，例如：

```bash
npm run verify:feature-x
```

这样 AI 不需要在一轮对话里插入大量零散命令输出，只需要读取最终结论：

- compile: pass
- unit test: pass
- integration test: fail
- fail case: xxx

### handoff note 约定

推荐在仓库中维护一个简短状态文件，例如：

- `docs/dev-notes/feature-x.md`
- `tmp/ai-handoff.md`

建议固定包含：

- 背景。
- 当前实现。
- 最近一次验证结果。
- 已知问题。
- 下一步计划。

## Failure modes

- 依赖超长聊天记录维持上下文。
- 每轮都塞入完整编译日志和测试原文。
- 没有阶段结束摘要，导致新会话需要重新爬梳全部历史。
- 验证步骤分散，工具输出过于碎片化。

## Related

- [[30 知识资源/AI/Claude API Prompt Caching 失效根因与优化策略|Claude API Prompt Caching 失效根因与优化策略]]
- [[30 知识资源/其他/Obsidian 知识库自动沉淀工作流|Obsidian 知识库自动沉淀工作流]]
- [[99 系统/元数据规范|元数据规范]]
