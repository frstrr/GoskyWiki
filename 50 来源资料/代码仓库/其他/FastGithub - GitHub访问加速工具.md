---
id: source-20261015-fastgithub
title: FastGithub - GitHub 访问加速工具
type: source
status: active
created: 2026-10-15
updated: 2026-10-15
tags:
  - 来源/代码仓库
  - 主题/GitHub
  - 主题/开发工具
  - 主题/网络加速
  - 主题/dotnet
aliases:
  - FastGithub
  - github加速神器
author: creazyboyone
source_type: repo
source_url: https://github.com/creazyboyone/FastGithub
source_author: creazyboyone（原作者 dotnetcore / xljiulang）
source_date: 2026-10-15
summary: 面向开发者的 GitHub 访问加速开源工具（C# / MIT）。自动解析并测速 GitHub 相关域名 IP，本地代理加速 clone/pull/push、Releases 与头像加载；支持 Google CDN 替换与 TLS 自定义。原仓库 dotnetcore/FastGithub 已下线，当前常用维护镜像为 creazyboyone/FastGithub（约 1.5k Stars，最新 v2.1.5）。
related:
  - "[[40 知识导航/工作流|工作流]]"
---

# FastGithub - GitHub 访问加速工具

## Source summary

**FastGithub** 是一款面向开发者的 **GitHub 访问加速** 开源工具，用本地代理方式缓解 GitHub 打不开、头像不加载、Releases 下不动、`git clone` / `pull` / `push` 失败等问题。

- **当前维护仓库**：https://github.com/creazyboyone/FastGithub（MIT，约 **1568** Stars / 372 Forks，C#）
- **原仓库**：`dotnetcore/FastGithub`（已删除，404）
- **最新发布**：`v2.1.5`（2025-06）
- **本地代理默认端口**：`127.0.0.1:38457`
- **触发收录**：头条介绍文 [开发者效率翻倍的秘密武器！FastGithub 让国外仓库说拉就拉](https://www.toutiao.com/article/7693930512049635859/)

> 官方 README 明确：**不具备翻墙能力**、不支持已停服的 Windows 7、不能用于游戏加速；建议只从 GitHub Releases 获取，勿信无关渠道。

## 核心能力

| 能力 | 说明 |
|------|------|
| **纯净 IP 解析** | 解析 GitHub 相关域名的可用 IP |
| **IP 测速择优** | 对候选 IP 测速，选用最快的 |
| **TLS 自定义** | 支持域名 TLS 连接自定义配置 |
| **Google CDN 替换** | 替换 Google CDN，缓解国外站点 js/css 加载失败 |
| **一键桌面端** | Windows 解压后运行 `FastGithub.UI.exe`，托盘常驻 |
| **多平台部署** | Windows / Linux / macOS 桌面或服务模式；另有 docker-compose 示例 |

## 快速上手（Windows）

1. 从 [Releases](https://github.com/creazyboyone/FastGithub/releases) 下载对应平台包并解压
2. 双击运行 `FastGithub.UI.exe`（托盘图标即可）
3. 可按需勾选开机自启、启动后最小化；连不上时可在托盘右键使用「更新 IP」

Linux / macOS 需将系统 HTTP(S) 代理设为 `127.0.0.1:38457`；macOS 还需信任 `cacert/fastgithub.cer`（详见仓库 `MacOSXConfig.md`）。

服务模式示例：

```bash
# Windows 服务
fastgithub.exe start
fastgithub.exe stop

# Linux systemd
sudo ./fastgithub start
sudo ./fastgithub stop
```

## 证书与常见问题

| 场景 | 处理 |
|------|------|
| git 报 `SSL certificate problem` | `git config --global http.sslverify false`（按需，注意风险） |
| Firefox 证书警告 | 导入并信任 `cacert/fastgithub.cer` |
| 安全注意 | 每机会生成自签 CA，**勿泄露私钥**；仅在可信环境安装信任 |

## 仓库与镜像线索

| 链接 | 说明 |
|------|------|
| https://github.com/creazyboyone/FastGithub | 常见维护 fork（推荐检索入口） |
| https://github.com/dotnetcore/FastGithub | 原仓库（已不可用） |
| https://gitee.com/mirrors/fastgithub | Gitee 镜像（历史同步源可能已失效） |
| https://gitee.com/tongxiaozhong/FastGithub | 另一份 Gitee 搬运镜像 |

## 适用场景

- 日常 `clone` / `pull` / `push` GitHub 仓库卡顿或失败
- Releases / 头像 / 网页资源加载缓慢
- 希望少改 hosts、少换 DNS，用本地工具一键处理

## 不适用

- 通用翻墙 / VPN
- 游戏加速
- 已停止支持的旧系统（如 Windows 7）

## Related

- [[40 知识导航/工作流|工作流]]