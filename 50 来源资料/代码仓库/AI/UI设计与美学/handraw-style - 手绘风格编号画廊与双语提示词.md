---
id: source-20260907-handraw-style
title: yang0/handraw-style - 手绘风格编号画廊与双语提示词 Skill
type: source
status: active
created: 2026-09-07
updated: 2026-09-07
tags:
  - 来源/代码仓库
  - 主题/提示词工程
  - 主题/AI
  - 主题/多模态
  - 主题/开源资源
  - 主题/Agent
author: yang0
source_type: repo
source_url: https://github.com/yang0/handraw-style
source_author: yang0
source_date: 2026-09-05
summary: 手绘风格编号画廊 + Agent Skill：整理 001–216 种手绘画风，按「编号+主题」生成中英双语生图提示词；可选按模型能力决定是否附带编号参考图。适合内容创作者与 AI 生图工作流。
related:
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/awesome-astra-prompts - GPT-6 Astra 提示词精选|awesome-astra-prompts - GPT-6 Astra 提示词精选]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]"
  - "[[50 来源资料/代码仓库/AI/Skills与方法论/Vibe Coding CN - AI结对编程指南|Vibe Coding CN - AI结对编程指南]]"
---

# yang0/handraw-style - 手绘风格编号画廊与双语提示词 Skill

## Source summary

**handraw-style** 是一套「手绘风格编号画廊 + 双语提示词 Agent Skill」。核心思路：把「凭感觉选画风」变成「记住一个编号」——先从编号画廊挑风格，再告诉 Skill 主题，即可得到带风格名称的中英文提示词，直接粘贴到各类生图 AI。

- GitHub：`yang0/handraw-style`（约 **265 Stars / 43 Forks**，截至 2026-09-07）
- 定位：**不是绘画引擎**，而是风格索引 + 提示词生成 Skill（默认只出 prompt，明确要求生图时才走生图流程）
- 语言/形态：HTML 编号画廊 + Markdown 风格表 + Python 工具脚本 + `handdraw-style-prompter` Skill
- 协议：仓库未声明 License（使用前自行确认）
- 默认分支：`master`

## 它解决什么问题

| 痛点 | 本仓库做法 |
|------|------------|
| 不会描述画风，只能说「可爱一点/文艺一点」 | 用 **001–216 编号**固定风格，无需背画风名 |
| 同一主题换模型后画风漂移 | 编号 + 风格名称作为稳定锚点 |
| 参考图一页太多，说不清喜欢哪张 | 编号画廊可视化浏览，记下编号即可 |

适合：不知道怎么描述画风的创作者；做公众号/小红书/短视频/品牌内容的人；希望图片保持稳定视觉气质的人。

## 核心能力

| 能力 | 说明 |
|------|------|
| **216 种手绘风格库** | 编号 `001`–`216`，含拼图预览与单张切图 |
| **编号画廊** | `handdraw-style-prompter/gallery/index.html` 浏览风格图片 |
| **双语提示词 Skill** | 输入「编号 + 主题」→ 输出风格名 + 中文 prompt + 英文 prompt |
| **模型能力感知生图** | 明确要求生图时，按模型能力决定：仅名称激活 / 加核心特征 / 传编号参考图 |
| **工具脚本** | 重建索引、切单图、校验库、CLI 出稿、解析是否需要参考图 |

## 风格分类（001–216）

| 分组 | 编号 | 主题 |
|------|------|------|
| A | 001–035 | 国际社论漫画 / 幽默手绘 |
| B | 036–054 | 国际绘本 / 叙事型手绘 |
| C | 055–082 | 现代平面 / 艺术化人物体系 |
| D | 083–123 | 日本作者 / 当代插画体系 |
| E | 124–154 | 中国作者 / 当代插画体系 |
| F | 155–200 | 通用网感 / 媒介 / 地域手绘 |
| G | 201–216 | 附件新增 / 中国当代插画补充 |

资源覆盖（MANIFEST）：16 张拼图 + 216 张单图（`images/individual/001.png`–`216.png`），编号段完整无缺失。权威风格表为 `styles_200_reorganized.md`。

## 怎么用

1. 打开编号画廊，浏览风格图片。
2. 记下喜欢的编号，例如 `041`。
3. 输入「编号 + 主题」，例如：`041号风格，主题：秋天的第一杯奶茶`。
4. 得到带风格名称的中英提示词，复制到生图 AI。

示例输入：

```text
041号风格，主题：秋天的第一杯奶茶
210号风格，主题：小男孩在雪地里点鞭炮
193号风格，主题：大唐夜宴
```

### 生图策略（Skill 行为摘要）

默认**只生成提示词**；用户明确要求「生图」时再调用生图工具。优先级：

1. **作者名称 + 风格名称** 可激活 → 不传参考图
2. 否则若有正向核心风格特征且可激活 → 加特征、仍不传图
3. 仍不足或模型能力未知 → 传对应编号单图作**风格参考**（忽略参考图中的主体/构图/故事）

仓库已按 `gpt-image-2` 建立首轮能力清单；可用 `python scripts/resolve_reference.py` 做确定性判断。

### 常用脚本

```text
python scripts/build_library.py          # 重建索引与画廊
python scripts/split_contact_sheets.py   # 拼图切单图
python scripts/validate_library.py       # 校验一致性
python scripts/prompt_style.py --style 18 --theme "秋天的第一杯奶茶"
python scripts/resolve_reference.py --model <model> --style 18
```

## 仓库结构（要点）

```text
handraw-style/
├── README.md
├── MANIFEST.md
├── styles_200_reorganized.md      # 权威风格表
├── images/                        # 拼图 + individual/ 单图
└── handdraw-style-prompter/       # Agent Skill
    ├── SKILL.md
    ├── gallery/index.html
    ├── references/                # styles.json、model_capabilities.json 等
    ├── agents/
    └── scripts/
```

## 链接

| 资源 | 链接 |
|------|------|
| **GitHub 仓库** | https://github.com/yang0/handraw-style |
| **Issues** | https://github.com/yang0/handraw-style/issues |
| **Pull Requests** | https://github.com/yang0/handraw-style/pulls |
| **编号画廊（仓库内）** | https://github.com/yang0/handraw-style/tree/master/handdraw-style-prompter/gallery |
| **Skill 定义** | https://github.com/yang0/handraw-style/blob/master/handdraw-style-prompter/SKILL.md |
| **风格表** | https://github.com/yang0/handraw-style/blob/master/styles_200_reorganized.md |
| **作者** | https://github.com/yang0 |

## My takeaways

1. **找开源资源时的定位**：需要「稳定手绘画风 + 可复用生图提示词」时优先想到本仓库；不是通用绘画模型，而是编号化风格资产与 Skill。
2. **与提示词类资源互补**：awesome-astra-prompts 偏 3D/Astra 示例策展；本库偏 2D 手绘插画风格编号与双语 prompt。
3. **与 Agent Skills 生态衔接**：可作为 Cursor/Codex 等代理的生图辅助 Skill，和 Superpowers / Vibe Coding CN 的工作流可组合使用。
4. **使用注意**：Skill 内示例路径写死为 `E:/handraw-style/...`，本地部署需按实际路径调整；仓库未标 License。
5. **内容创作场景直接可用**：公众号封面、小红书配图、短视频封面等「要辨识度、要风格一致」的需求，用编号比每次现编画风描述更稳。

## Related

- [[50 来源资料/代码仓库/AI/Skills与方法论/awesome-astra-prompts - GPT-6 Astra 提示词精选|awesome-astra-prompts - GPT-6 Astra 提示词精选]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/Superpowers - Agent Skills 框架|Superpowers - Agent Skills 框架]]
- [[50 来源资料/代码仓库/AI/Skills与方法论/Vibe Coding CN - AI结对编程指南|Vibe Coding CN - AI结对编程指南]]