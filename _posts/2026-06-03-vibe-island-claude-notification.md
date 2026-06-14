---
layout: post
title: "两个小工具，非常惊艳地解决了 Claude Code 多开通知难题"
date: 2026-06-03 10:00:00 +0800
categories: [文章, AI Coding]
tags: [Claude Code, Vibe Island, 效率工具, AI Agent, 硬件, 小而美]
image: /assets/images/vibe-island-claude-notification-cover.png
author: 黑石
excerpt: 介绍两个惊艳的 Claude Code 多开通知工具：Vibe Island 利用 MacBook 刘海实现免切换通知，Vibe Coding Signal Light 用三色信号灯指示 AI Agent 状态。
---

这是我最近发现的两个非常有创意的小工具，专门用来解决 Claude Code（或 Codex）等 AI Agent 客户端的通知问题。一个软件，一个硬件，都非常有想法。

---

## 一、Vibe Island — 基于 MacBook 刘海的通知工具

### 它是怎么工作的

Vibe Island 依托于 MacBook 的 Dynamic Island（刘海屏），在刘海上做了一层通知层。它通过监控本地 Agent 客户端（如 Claude Code、Codex）的系统 Hook，在需要人工干预或任务完成时，直接在刘海上弹出通知，甚至可以基于弹窗直接操作。

### 解决的核心痛点

当我们需要**多开 Claude Code 或 Codex**、多线程工作时，最麻烦的就是来回切换窗口。Vibe Island 让你不用切窗口，直接点击刘海就能操作和查看通知，非常高效。

### 价格

- 买断制，按设备数量计算，每台 15 刀
- 我买了 2 台（笔记本 + Mac Mini），花了 25 刀
- 给的是一个激活码，两台设备共用

**官网**：https://vibeisland.app/

![Vibe Island](https://get-notes.umiwi.com/get_notes_prod%2F202606022347%2Fgetnotes_img_1a87bec980051bc0Lp4XMiNM.png?Expires=1783008459&OSSAccessKeyId=LTAI5t7toTp72R3TvdXf9QdK&Signature=DBhWjGtB5bmZ%2B%2BNaWRX1LbE5I1k%3D&x-oss-process=image%2Fresize%2Cm_lfit%2Cw_720%2Ch_3240)

![Vibe Island](https://get-notes.umiwi.com/get_notes_prod%2F202606022347%2Fgetnotes_img_1a87bec6001494609Lb5Jamk.png?Expires=1783008459&OSSAccessKeyId=LTAI5t7toTp72R3TvdXf9QdK&Signature=uRLzxqUpX5gBDCTz7MXfPaRo3ic%3D&x-oss-process=image%2Fresize%2Cm_lfit%2Cw_720%2Ch_3240)

![Vibe Island](https://get-notes.umiwi.com/get_notes_prod%2F202606022328%2Fgetnotes_img_1a87bda180049460jz9EW5VY.png?Expires=1783008459&OSSAccessKeyId=LTAI5t7toTp72R3TvdXf9QdK&Signature=3%2Fcof2XjKotG61dqseEmwmdcKT0%3D&x-oss-process=image%2Fresize%2Cm_lfit%2Cw_720%2Ch_3240)

![Vibe Island](https://get-notes.umiwi.com/get_notes_prod%2F202606022326%2Fgetnotes_img_1a87bd88c02419c8YvwI19s8.png?Expires=1783008459&OSSAccessKeyId=LTAI5t7toTp72R3TvdXf9QdK&Signature=%2FfAbFiq%2B5CzeQhOw%2FajmAZ8uALY%3D&x-oss-process=image%2Fresize%2Cm_lfit%2Cw_720%2Ch_3240)

![Vibe Island](https://get-notes.umiwi.com/get_notes_prod%2F202606022323%2Fgetnotes_img_1a87bd56800494608wu6PGps.png?Expires=1783008459&OSSAccessKeyId=LTAI5t7toTp72R3TvdXf9QdK&Signature=uswtQWKHxg6NHRuSFtGY6wWdvRE%3D&x-oss-process=image%2Fresize%2Cm_lfit%2Cw_720%2Ch_3240)

### 激活码展示

![激活码](https://get-notes.umiwi.com/get_notes_prod%2F202606022315%2Fgetnotes_img_1a87bce2c0351bc012KZ1yIf.png?Expires=1783008459&OSSAccessKeyId=LTAI5t7toTp72R3TvdXf9QdK&Signature=N9EBbX9dbcc75EKsC%2BsN%2Famiwd4%3D&x-oss-process=image%2Fresize%2Cm_lfit%2Cw_720%2Ch_3240)

### 开源版本

这么好的软件，很快就有了开源复刻版：

- https://github.com/Octane0411/open-vibe-island
- https://github.com/farouqaldori/vibe-notch

我一开始用过开源版，但功能简陋且有 bug（点击会有问题），加上这是刚需，最终还是选择了付费购买原版。

> ⚠️ 提醒：闲鱼上 25 块钱就能买到破解版，但这么好的软件建议尽量付费支持作者。

### 关于作者

作者是中国人，听说还是个设计师，有完美主义的强迫症，所以做出来的东西非常丝滑好用。5 月 20 号才发布上线，大家可以去 X 上关注他。

![作者](https://get-notes.umiwi.com/get_notes_prod%2F202606022356%2Fgetnotes_img_1a87bf5100051bc0ExqSgYtS.png?Expires=1783008459&OSSAccessKeyId=LTAI5t7toTp72R3TvdXf9QdK&Signature=Zomvh7wHJKgrfAfPvVJ5SAXjuP0%3D&x-oss-process=image%2Fresize%2Cm_lfit%2Cw_720%2Ch_3240)

---

## 二、Vibe Coding Signal Light — 开源硬件信号灯

这是一个用 USB 连接的三色信号灯硬件，通过 Claude Hook 发送信号控制。原理很简单但非常有创意：

| 灯色 | 含义 |
|------|------|
| 🔴 红灯闪烁 | Claude 需要人工介入 |
| 🟡 黄灯 | Claude 正在运行中 |
| 🟢 绿灯 | Claude 空闲 |

**开源地址**：https://github.com/starlight36/vibecoding-signal-light

![信号灯](https://get-notes.umiwi.com/get_notes_prod%2F202606022317%2Fgetnotes_img_1a87bcfd801419c8RrIDqMgU.png?Expires=1783008459&OSSAccessKeyId=LTAI5t7toTp72R3TvdXf9QdK&Signature=N0t2TDiiKqdvdszMpuEpFfFxzKk%3D&x-oss-process=image%2Fresize%2Cm_lfit%2Cw_720%2Ch_3240)

### 现状

淘宝上没有卖成品，只有一些需要自己组装的小硬件套件。感兴趣的可以自己 DIY，甚至可以考虑做成蓝牙版本（不需要插 USB），放电池或充电都行，直接贴在桌上，体验会更好。

---

## 三、我的启发

这两个工具给我几点启发：

1. **做小而美的产品** — 只要足够解决某个小问题，就会有人愿意付费。
2. **Vibe Coding 不止于软件** — AI 时代也可以做硬件，各种带电子元件的小设备跟 AI 结合，都是很好的方向。
3. **赚美刀的机会** — 尤其是发到国外，美国人很愿意为效率工具付费。
4. **定义问题比解决问题更重要** — 在 AI 时代，解决问题的成本已经非常低了，关键是你能否定义出正确的问题。

大家可以想一想，身边有没有类似的问题出现？这又是一波通过做工具赚钱的新兴产业。
