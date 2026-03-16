---
layout: post
title: "OpenClaw 飞书机器人对接完全指南"
date: 2026-03-11 21:59:00 +0800
categories: [文章]
tags: [AI 编程，飞书机器人，OpenClaw，Clawdbot，自动化]
image: /assets/images/openclaw-feishu.png
author: 黑石
excerpt: 从安装配置到飞书对接，一步步教你部署 OpenClaw 个人 AI 操作系统，实现飞书机器人智能对话。
---

## 前言

在生成式人工智能从单纯的"对话框"向能够执行复杂任务的"自主代理（Agent）"演进的当下，**OpenClaw**（原名 Clawdbot）作为一个开源的、本地优先的 AI 代理网关，正在重塑个人与 AI 的交互范式。

不同于 ChatGPT 或 Claude 等依赖云端托管的 SaaS 服务，OpenClaw 通过独特的 Gateway-Node 架构，将大模型的推理能力下沉至用户私有硬件，并通过标准化的协议连接至飞书、Telegram、Discord 等主流即时通讯平台。

本文将系统讲解 OpenClaw 的安装配置，并一步步教你如何接入飞书，打造属于自己的 AI 助手。

## 一、前置准备

### 环境要求

| 项目 | 要求 |
|------|------|
| Node.js | ≥ 22.x |
| 操作系统 | macOS / Linux / Windows (WSL2) |
| 内存 | ≥ 2GB 可用 |
| AI API | 您常用的模型的 API Key |
| Docker | 可选，用于容器化部署 |

### 第一步：全局安装 OpenClaw

```bash
npm install -g openclaw
```

### 第二步：运行配置向导

```bash
openclaw config
```

向导会引导你完成：
- **AI 模型配置** – 输入 API Key
- **工作目录设置** – 默认 ~/openclaw
- **渠道启用** – 选择要连接的聊天平台（选择 Feishu/Lark）
- **守护进程安装** – 让 Gateway 后台持续运行

### 第三步：验证安装

```bash
openclaw --version
```

## 二、常用 Skills 配置

OpenClaw 的强大之处在于可扩展的 Skills 系统。以下是几个实用的 Skills：

### 网页搜索 Skill

配置后 OpenClaw 可以搜索实时网络信息回答问题。

**方式一：通过 ClawdHub 安装（推荐）**
```bash
npx skills add @openclaw/web-search
```

**方式二：手动下载**
```bash
# 下载技能到 skills 目录
```

> 若无海外信用卡注册 Brave Search，也可以使用火山云「融合信息搜索 API」

### 文件操作 Skill

```bash
npx skills add @openclaw/file-operations
```

### 其他实用 Skills

- **self-improving-agent**：让 AI 记录错误和经验，转化为长期记忆
- **find-skills**：帮助用户发现并安装代理技能
- **humanizer**：去除文本中 AI 生成痕迹

**关键命令：**
```bash
npx skills find [query]          # 搜索技能
npx skills add <package>         # 安装技能
npx skills check                 # 检查技能更新
npx skills update                # 更新所有已安装的技能
```

## 三、飞书应用配置

### 方式一：通过安装向导添加（推荐）

如果您刚安装完 OpenClaw，可以直接运行向导添加飞书：

```bash
openclaw channels add
```

向导会引导您完成：
- 创建飞书应用并获取凭证
- 配置应用凭证
- 启动网关

✅ 完成配置后，使用以下命令检查网关状态：
```bash
openclaw gateway status        # 查看网关运行状态
openclaw logs --follow         # 查看实时日志
```

### 方式二：通过命令行添加

如果您已经完成了初始安装，可以用以下命令添加飞书渠道：

```bash
openclaw channels add feishu
```

然后根据交互式提示输入 App ID 和 App Secret 即可。

## 四、飞书机器人对接详细步骤

### 1️⃣ 进入飞书应用中心

访问：**[开发者后台 - 飞书开放平台](https://open.feishu.cn/app)**

- 使用飞书账号登录
- Lark（国际版）请使用 https://open.larksuite.com/app，并在配置中设置 `domain: "lark"`

### 2️⃣ 新建企业自建应用

**路径：** 创建应用 → 企业自建应用

基础信息按提示填写即可（名称、描述等），完成后进入应用详情页。

### 3️⃣ 启用机器人能力

在 **添加应用能力 > 机器人** 页面：
1. 开启机器人能力
2. 配置机器人相关设置

### 4️⃣ 配置应用权限

在 **权限管理** 页面，点击 **批量导入** 按钮，粘贴以下 JSON 配置一键导入所需权限：

**基础权限：**
```json
{
  "im:message": true,
  "im:chat": true,
  "bot":true
}
```

**可选全功能权限：**
```json
{
  "im:message": true,
  "im:chat": true,
  "bot": true,
  "message:send_as_bot": true,
  "group:manage": true
}
```

> 确保消息、机器人、事件订阅等相关权限均已开启。

### 5️⃣ 更新应用 Token

回到 **凭证与基础信息** 页面，将黄色区域中的以下信息同步更新到 OpenClaw 配置中：
- **App ID**
- **App Secret**
- **Token**

然后运行命令重启：
```bash
openclaw gateway restart
```

### 6️⃣ 设置事件回调（Callback）

⚠️ **重要提醒：** 在配置事件订阅前，请务必确保已完成以下步骤：
- 运行 `openclaw channels add` 添加了 Feishu 渠道
- 网关处于启动状态（可通过 `openclaw gateway status` 检查状态）

在 **事件订阅** 页面：
- 选择 **使用长连接接收事件（WebSocket 模式）**
- 添加事件：**`im.message.receive_v1`**（接收消息）

> ⚠️ 注意：如果网关未启动或渠道未添加，长连接设置将保存失败。

### 7️⃣ 发布应用

在 **版本管理与发布** 页面：
- 创建版本
- 提交审核并发布
- 等待管理员审批（企业自建应用通常自动通过）

### 8️⃣ 发送测试消息

在飞书中找到您创建的机器人，发送一条消息。

### 9️⃣ 配对授权

默认情况下，机器人会回复一个配对码。您需要批准此代码：

```
配对码：XXXXXX
```

批准后即可正常对话！🎉

## 五、常用管理命令

```bash
# 查看网关状态
openclaw gateway status

# 重启网关
openclaw gateway restart

# 查看实时日志
openclaw logs --follow

# 添加渠道
openclaw channels add

# 列出所有渠道
openclaw channels list

# 删除渠道
openclaw channels remove <channel-id>
```

## 六、常见问题

### 网关无法启动
- 检查 Node.js 版本是否 ≥ 22.x
- 检查端口是否被占用（默认端口 18789）
- 查看日志：`openclaw logs --follow`

### 飞书机器人不回复消息
- 确认事件订阅已配置（`im.message.receive_v1`）
- 确认网关正在运行
- 检查权限是否已正确配置
- 确认机器人已发布并添加到群聊

### 配对码无效
- 确认机器人配置中的 App ID / App Secret 正确
- 重启网关：`openclaw gateway restart`
- 重新发送消息触发配对

## 总结

OpenClaw 通过本地优先的架构设计，让用户能够完全掌控自己的 AI 助手。相比云端 SaaS 服务，它具有以下优势：

- **数据隐私**：所有对话记录和上下文存储在本地
- **自主控制**：可自定义 Skills，扩展 AI 能力边界
- **成本可控**：只需支付 API 调用费用，无订阅费
- **灵活部署**：支持 Mac Mini、Linux 服务器、树莓派等多种硬件

通过飞书集成，你可以随时随地在熟悉的办公环境中与 AI 对话，让 OpenClaw 成为真正的"数字副驾驶"。

## 参考资料

- [OpenClaw 官方文档](https://github.com/OpenClaw)
- [飞书开放平台](https://open.feishu.cn/)
- [ClawdHub 技能市场](https://clawdhub.example.com)

---

*本文内容整理自飞书官网 - 《一文完全搞懂 OpenClaw（Clawdbot）附飞书对接教程！》*
