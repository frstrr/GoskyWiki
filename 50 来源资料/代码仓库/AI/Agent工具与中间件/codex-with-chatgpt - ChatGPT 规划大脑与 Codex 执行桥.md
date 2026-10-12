---
id: source-20261012-codex-with-chatgpt
title: codex-with-chatgpt - ChatGPT 规划大脑与 Codex 执行桥
type: source
status: active
created: 2026-10-12
updated: 2026-10-12
tags:
  - 来源/代码仓库
  - 主题/AI
  - 主题/Agent
  - 主题/MCP
  - 主题/CLI
  - 主题/AI编程Agent
  - 主题/开源资源
  - 主题/ChatGPT
  - 主题/Codex
author: XiaoDuoYa
source_type: repo
source_url: https://github.com/XiaoDuoYa/codex-with-chatgpt
source_author: XiaoDuoYa
source_date: 2026-10-12
summary: 把 ChatGPT 网页版变成 Codex 的规划与审查大脑：控制面用 Computer Use 传极小状态消息，数据面经 OAuth 保护的只读 MCP 按需读工作区；执行权仍在 Codex，不用 API Key、不做逆向代理。
related:
  - "[[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill - AI Agent 登录态浏览器操控]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]"
  - "[[40 知识导航/AI 编程 Agent|AI 编程 Agent]]"
  - "[[40 知识导航/AI 工具使用|AI 工具使用]]"
---

# codex-with-chatgpt - ChatGPT 规划大脑与 Codex 执行桥

## Source summary

**Codex with ChatGPT**（仓库 [XiaoDuoYa/codex-with-chatgpt](https://github.com/XiaoDuoYa/codex-with-chatgpt)）把已付费的 **ChatGPT 网页版**接到 **Codex** 编码会话：ChatGPT 负责推理 / 规划 / 审查，Codex 保留完整执行权（编辑、Shell、测试、修复）。仓库不会被整包上传——ChatGPT 经 **只读 MCP**、按需读取它真正需要的那几行代码。

- 约 **7,270 stars** / **681 forks**（2026-10-12 GitHub API）；许可证 **MIT**；主语言 **TypeScript**
- Topics：`ai-agents` · `chatgpt` · `codex` · `mcp` · `model-context-protocol` · `oauth`
- 一句话定位：**ChatGPT 负责思考，Codex 负责干活**（官方网页 + 只读 MCP 桥，不用 API Key、不做逆向代理）
- 交付形态：本地 **C2C Bridge**（`c2c` CLI）+ **Codex Skill**（`skill/SKILL.md`）+ Cloudflare 隧道 + ChatGPT 连接器配对
- 非官方社区项目，与 OpenAI 无关联、未获背书；状态称 V1，已端到端验证 Bridge / OAuth / 隧道 / 连接器配置

## 解决什么问题

| 痛点 | 本项目方案 |
|------|------------|
| ChatGPT Plus/Pro 网页额度闲置，Codex 却烧 API/额度做规划与 Review | 思考与审查交给网页版 ChatGPT；Codex 只执行 |
| 想用 ChatGPT 能力又不愿暴露整仓 / 写 API Key / 搞逆向代理 | 官方网页 UI + OAuth 保护的只读 MCP，按需读行 |
| Agent「说测过就信」审查不独立 | 执行后 ChatGPT 经 MCP 自查 git diff / 测试记录 |
| 小白不会配 MCP / Tunnel / OAuth | 一段话交给 Codex 自动安装；Skill 可每日自检更新 |

## 核心能力

| 能力 | 说明 |
|------|------|
| **双平面协作** | 控制面：Computer Use 交换极小 `[C2C]` 状态（`INIT → PLAN → EXECUTED → REVIEW → DONE`），不粘贴 diff/日志/文件体；数据面：ChatGPT 经 MCP 自拉所需上下文 |
| **只读 MCP 工具（10）** | `workspace_info`、`list_directory`、`read_file`、`search_workspace`、`git_status`、`git_diff`、`test_status`、`execution_summary`、`execution_output`、`read_image` |
| **独立审查闭环** | Codex 执行后，ChatGPT 亲自看真实 diff 与测试记录，不盲信「全绿」口头报告 |
| **安全边界** | 服务端无写/删/Shell/提交工具；令牌绑单工作区；路径 realpath 收敛；`.env*`/密钥等默认拒绝；公网端点强制 OAuth 2.1（PKCE）；浏览器侧仅一次性配对码 |
| **Codex Skill UX** | 安装 `skill/SKILL.md` 后用自然语言完成首次配置与日常「用 C2C 实现 XXX」；可说「更新 Codex with ChatGPT」 |
| **可选固定域名** | 默认 Cloudflare 临时隧道；有 Cloudflare 账号+域名时可配稳定主机名，减少重启后重配连接器 |
| **媒体交接** | 连接器只读；ChatGPT 页面下载图片/视频后，本地用 `c2c asset import` 校验导入（签名/大小/拒活动 SVG/不覆盖） |

## 工作原理（简图）

```text
ChatGPT 网页版（推理/规划/审查）
   │ MCP 数据面          ▲ Computer Use 控制面（消息 < 1KB）
   ▼                     │
C2C Bridge（本机回环 HTTP + OAuth + 配对 + Cloudflare Tunnel）
   │ 只读
   ▼
本地工作区  ◀──编辑/git──  Codex Harness（Shell / 测试 / 修复）
```

## 快速开始

**环境**：Node.js ≥ 20、git；公网连接需 `cloudflared`（Skill 可代装）。QUIC 被拦时可设 `C2C_TUNNEL_PROTOCOL=http2` 后重启 Bridge。

### 一段话交给 Codex（推荐）

把仓库 README 中的「一段话安装」原文贴给 Codex，让其自检环境、克隆构建、安装 Skill、跑 `c2c setup`（内置浏览器配 ChatGPT 连接器与配对码）。仅登录 / 验证码 / 2FA 时打断用户。

### 手动三步（摘要）

1. 克隆并构建仓库；把 `skill/` 装到 `$CODEX_HOME/skills/codex-with-chatgpt/`（未设则 `~/.codex`，Windows 为 `%USERPROFILE%\\.codex`）。
2. 对 Codex 说：**「使用 Codex with ChatGPT 完成首次配置。」**
3. 日常：**「使用 Codex with ChatGPT，帮我实现 XXX。」**

成功时可见清单：项目识别 → Bridge 启动 → 安全连接 → ChatGPT 已连 → 文件读取测试通过。

**开发者命令（摘要）**

```bash
pnpm install && pnpm build   # 产出 dist/，暴露 c2c
pnpm test                    # vitest（路径安全、OAuth、配对、MCP 等）
c2c setup                    # Bridge + 隧道 + 配对码
c2c sandbox-allow            # 设置目录加入 Codex 沙箱白名单
c2c status / doctor / pair / unpair / logs / stop
```

## 安全要点（选用前必读）

- **构造只读**：写文件/删除/Shell/提交工具不存在，提示注入无法「打开」它们。
- **工作区边界**：一令牌一工作区；symlink、`../`、绝对路径逃逸有测试覆盖拦截。
- **敏感文件默认不出**：`.env*`、SSH、凭据等拒绝（`.env.example` 可放行）；可用 `.c2cignore`。
- **知道 URL ≠ 有权限**：无 OAuth 令牌 401；跨工作区令牌 403；配对码短 TTL、限次、用后即毁。
- **非官方**：依赖 ChatGPT 网页与连接器能力，产品侧变更可能导致需更新 Skill/桥。

完整威胁模型见仓库 [docs/security.md](https://github.com/XiaoDuoYa/codex-with-chatgpt/blob/main/docs/security.md)。

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/XiaoDuoYa/codex-with-chatgpt |
| **中文 README** | https://github.com/XiaoDuoYa/codex-with-chatgpt/blob/main/README.zh-CN.md |
| **英文 README** | https://github.com/XiaoDuoYa/codex-with-chatgpt/blob/main/README.md |
| **架构** | https://github.com/XiaoDuoYa/codex-with-chatgpt/blob/main/docs/architecture.md |
| **协议** | https://github.com/XiaoDuoYa/codex-with-chatgpt/blob/main/docs/protocol.md |
| **安全** | https://github.com/XiaoDuoYa/codex-with-chatgpt/blob/main/docs/security.md |
| **故障排查** | https://github.com/XiaoDuoYa/codex-with-chatgpt/blob/main/docs/troubleshooting.md |
| **Issues** | https://github.com/XiaoDuoYa/codex-with-chatgpt/issues |
| **许可证** | MIT |

## My takeaways

1. **省的是「规划/审查」侧额度与 API 消耗**，不是替代 Codex 执行；适合已有 ChatGPT 订阅、又常用 Codex 的人。
2. **只读 MCP + 小消息控制面** 是设计核心：既让网页 ChatGPT 看见必要上下文，又避免整仓粘贴与写权限外泄。
3. **和 BrowserSkill 互补**：BrowserSkill 给 Agent 操控已登录浏览器；本项目让 ChatGPT 网页反过来当 Codex 的规划/审查大脑。
4. **Skill 自动安装路径降低门槛**，但首配仍依赖 ChatGPT 连接器、隧道与（可选）Cloudflare 账号体验。
5. **选用时盯安全模型与非官方声明**：路径/敏感文件策略够认真，但长期可用性取决于 OpenAI 网页侧产品变化。

## Related

- [[50 来源资料/代码仓库/AI/Agent工具与中间件/BrowserSkill - AI Agent 登录态浏览器操控|BrowserSkill - AI Agent 登录态浏览器操控]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
- [[40 知识导航/AI 编程 Agent|AI 编程 Agent]]
- [[40 知识导航/AI 工具使用|AI 工具使用]]
