---
id: source-20260907-teamai-cli
title: Tencent/teamai-cli - 团队 AI Harness 统一管理
type: source
status: active
created: 2026-09-07
updated: 2026-09-07
tags:
  - 来源/代码仓库
  - 主题/AI编程Agent
  - 主题/团队协作
  - 主题/Skills
  - 主题/MCP
author: Tencent
source_type: repo
source_url: https://github.com/Tencent/teamai-cli
source_author: Tencent
source_date: 2026-09-07
summary: 腾讯开源的 TeamAI CLI：用共享 Git 仓库统一分发 Skills、Rules、MCP、Hooks 与团队知识，驾驭 Claude Code、Codex、Cursor、CodeBuddy、WorkBuddy 等多款 AI Agent，覆盖 Team Execution / Context / Improvement 三层能力。
related:
  - "[[50 来源资料/代码仓库/AI/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]"
  - "[[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
---

# Tencent/teamai-cli - 团队 AI Harness 统一管理

## Source summary

**TeamAI**（`teamai-cli`）是腾讯开源的团队级 AI Agent 管理工具，口号是 *Make Every Team AI Native*。它把团队的 Skills、Rules、MCP、Hooks、文档与知识沉淀到共享 Git 仓库，再通过 `push → 评审合并 → pull` 同步到每位成员本地的各类 AI 工具。

- **仓库**：https://github.com/Tencent/teamai-cli
- **npm 包**：https://www.npmjs.com/package/teamai-cli
- **许可证**：MIT
- **定位**：团队 Harness（执行规范 + 上下文知识 + 持续改进），而非单人技能包
- **支持的 Agent**：Claude Code、Codex、CodeBuddy、WorkBuddy、OpenCode、Cursor、Qoder、OpenClaw、Hermes、DeepSeek Harness 等（各工具能力覆盖度不同）
- **Git 托管**：GitHub、GitLab、GitCode、CNB、TGit 及私有 Git 服务

## 快速开始

```bash
npm install -g teamai-cli
```

管理员在 Git 托管平台创建共享经验仓库（需给成员写权限），成员执行：

```bash
# 项目级（默认，资源装到项目目录）
teamai init https://github.com/yourorg/yourrepo

# 或用户级（资源装到 ~/）
teamai init https://github.com/yourorg/yourrepo --scope user
```

尚无团队仓库时，可从 [teamai-hub](https://github.com/teamai-hub) 模板起步（内置 skills / rules / review agents），再用 `teamai init` 关联。

初始化后，开启 AI 会话时会自动拉取管理员发布的 Harness 更新。完整指南见仓库 `docs/usage-guide.zh-CN.md`。

## 产品架构（三层）

| 层 | 要解决的问题 | CLI 中的体现 |
|----|--------------|--------------|
| **Team Execution** | 让每个 Agent 按团队方式工作 | `init` / `pull` / `push`；skills、rules、agents、hooks、MCP、env |
| **Team Context** | 让每个 Agent 理解整个团队 | recall、learnings、代码知识图谱、teamwiki |
| **Team Improvement** | 让每次执行变成团队能力积累 | 摩擦信号经验分享、sessions、digest、dashboard |

### 分发策略

| 能力 | 命令 | 作用 |
|------|------|------|
| **Roles** | `teamai roles` | 角色 → 命名空间映射，成员只同步匹配的 skills |
| **Tags** | `teamai tags` | 给 skills/rules 打标签，按需订阅 |
| **Sources** | `teamai source` | 订阅其他团队或本团队公共 skill 仓库，pull 时自动同步 |

## 核心能力

### Team Execution — One Team. One Harness. Every Agent.

- 共享 Git 仓库统一存放 skills / rules / docs / hooks
- 流程：`teamai push` 开分支 + MR → 评审合并 → SessionStart hook 触发 `teamai pull` 同步到本地各工具目录（如 `~/.claude/skills/`、`~/.cursor/skills/` 等）
- 同一资源若已有未合并 PR，再次 `push` 会就地更新该 PR，避免重复开单
- **团队 Hooks**：`hooks/hooks.yaml` 声明一次，pull 分发到各工具；支持 `teamai hooks list|inject|remove`
- **团队 MCP**：`mcp/mcp.yaml` 声明，密钥用 `${VAR}`；支持 `teamai mcp list|inject|remove`
- **Skill 订阅源**：`teamai source add/list/browse/remove`
- **团队包**：`teamai install` 共享并恢复 npm 包与 Claude Code 插件

### Team Context — 团队知识可召回

- **摩擦信号经验沉淀**：Session 结束时 Stop hook 按打断/拒绝/重试等摩擦评分；达标后提示运行 `/teamai-share-learnings`，自动总结并推送到团队仓库（顺畅长会话不触发）
- **团队知识检索（默认关闭）**：`teamai recall enable|disable|status`；开启后部署 `teamai-recall` 子 agent，任务前自动检索；也可手动 `teamai recall "关键词"`（BM25 + 图谱增强）
- **代码知识图谱**：`teamai import --from-repo|--from-org` 解析为 `teamwiki/` 结构化图谱；AST 轨（TS/JS/Python/Go，tree-sitter WASM）+ 启发式轨（全语言）；可设 `TEAMAI_SKIP_AST=1` 强制启发式

### Team Improvement — 洞察与闭环

| 能力 | 命令 | 内容 |
|------|------|------|
| **Usage** | `teamai digest` | 团队周报：token、会话量、干预率 |
| **Sessions** | `teamai session save` | 脱敏单会话摘要，喂给周报 Highlights |
| **Dashboard** | `teamai dashboard` | Web 看板：会话状态、干预、token；含 KB Health（覆盖率、高频/沉默条目、趋势、贡献） |

## 常用命令一览

| 命令 | 说明 |
|------|------|
| `teamai init` | OAuth 登录、关联仓库、注册成员、注入 hooks |
| `teamai pull` / `push` | 拉取注入本地 / 推送并开 MR |
| `teamai install [target]` | 安装团队 npm 包与 Claude 插件 |
| `teamai status` / `doctor` | 差异状态 / 配置诊断 |
| `teamai recall` / `promote` / `maintenance` | 检索、晋升 learning、维护知识库 |
| `teamai import` / `codebase --lint` | 导入知识 / 图谱健康检查 |
| `teamai roles` / `tags` / `source` | 角色、标签、订阅源 |
| `teamai digest` / `dashboard` / `session save` | 周报、看板、会话摘要 |
| `teamai uninstall` | 移除所有 teamai 资源与 hooks |

## 适用场景

- 需要在团队内统一 Skills / Rules / MCP / Hooks，避免每人各配一套
- 希望 AI 会话自动同步团队最新 Harness，并支持 MR 评审后再分发
- 想把「踩坑摩擦」沉淀为可召回的团队知识，而非散落在个人聊天记录
- 使用 Cursor / Claude Code / Codex / CodeBuddy / WorkBuddy 等多工具，需要一套跨 Agent 的团队配置

## 相关链接

| 类型 | 链接 |
|------|------|
| GitHub 仓库 | https://github.com/Tencent/teamai-cli |
| 中文 README | https://github.com/Tencent/teamai-cli/blob/main/README.zh-CN.md |
| 英文 README | https://github.com/Tencent/teamai-cli/blob/main/README.md |
| 使用指南（中文） | https://github.com/Tencent/teamai-cli/blob/main/docs/usage-guide.zh-CN.md |
| npm 包 | https://www.npmjs.com/package/teamai-cli |
| 团队模板 org | https://github.com/teamai-hub |
| 贡献指南 | https://github.com/Tencent/teamai-cli/blob/main/.github/CONTRIBUTING.md |
| 许可证 | MIT |

## My takeaways

1. **团队 Harness 而非个人 Skills 包**：与 Superpowers / mattpocock skills 偏「个人代理方法论」不同，TeamAI 强调 Git 协作流（push/MR/pull）和跨成员分发。
2. **摩擦驱动沉淀很实用**：只对「较劲过」的 session 提示分享，避免噪音知识库。
3. **三层闭环完整**：Execution（规范分发）→ Context（召回/图谱）→ Improvement（digest/dashboard）形成可运营的团队 AI 能力体系。
4. **与现有生态可叠加**：本地仍可用 Superpowers 等方法论 Skills；TeamAI 负责团队级同步与知识召回。
5. **OpenClaw / WorkBuddy 同属其兼容矩阵**：若已在用腾讯系或 Claw 生态，可一并纳入团队 Harness 管理。

## Related

- [[50 来源资料/代码仓库/AI/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
- [[50 来源资料/代码仓库/AI/OpenClaw - 多平台 AI 助手|OpenClaw - 多平台 AI 助手]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]