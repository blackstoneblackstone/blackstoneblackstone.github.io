---
layout: post
title: "Agentic Engineering 的六月实践清单"
date: 2026-06-05 10:00:00 +0800
categories: [文章, AI]
tags: [AI, Agentic Engineering, Claude Code, Codex, 工作流, AI 编程]
image: https://pbs.twimg.com/media/HJu3llba0AA8PAp?format=jpg&name=large
author: 黑石
excerpt: Matt Van Horn 在 X 上整理了他截至 2026 年 6 月的 Agentic Engineering 工作流：用 plan.md 驱动代理，用语音输入，用多会话并行，把 Codex 和 Claude Code 组合成一套真实可用的生产系统。
---

# Agentic Engineering 的六月实践清单

![Every Agentic Engineering Hack I Know](https://pbs.twimg.com/media/HJu3llba0AA8PAp?format=jpg&name=large)

> 原文：[Every Agentic Engineering Hack I Know (June 2026)](https://x.com/mvanhorn/status/2061877533885473181?s=46)  
> 作者：Matt Van Horn  
> 来源：X Article  
> 说明：本文是基于原文公开内容整理的中文译介和摘译，不是逐字全文翻译。重点保留原文结构、核心观点、实践清单和对 AI 编程工作流的启发。

Matt Van Horn 在这篇长文里总结了自己截至 2026 年 6 月的 Agentic Engineering 工作流。他的核心观点很直接：过去大家叫它 vibe coding，但模型能力提升之后，这套东西已经不只是玩具，而是能真实支撑软件交付的工程方式。

他把自己的工作方式概括成一句话：**不靠 IDE，靠 plan.md 文件和语音。**

这篇文章最有价值的地方，不是某一个工具，而是它把“人如何和多个 coding agent 协同工作”讲得非常具体：什么时候规划、什么时候执行、什么时候切换上下文、什么时候把任务交给 Codex、什么时候用 Claude Code、怎么让语音、终端、邮件、远程控制和个人知识库都接入同一个循环。

## 1. 先让 agent 写 plan.md

Matt 认为最重要的习惯是：只要不是一行改动，就先生成一个 `plan.md`。

有想法时，不是马上写代码，也不是先自己慢慢想，而是直接用 `/ce-plan` 生成计划。这个计划可以来自任何输入：

- 一个产品想法；
- 一个 GitHub issue；
- 一段终端错误；
- 一张截图；
- 一段 Slack 对话；
- 一个设计稿；
- 一段还没想清楚的模糊需求。

如果想法还很模糊，他会先用 `/ce-brainstorm` 和 agent 一起把问题想清楚，再进入 `/ce-plan`。

`/ce-plan` 的价值在于，它不是随便写个待办列表，而是会并行派出多个研究 agent：一个读代码库、一个找已有模式、一个查历史方案，必要时再去查外部文档和最佳实践。最后合并成一个结构化计划：问题是什么、方案是什么、要改哪些文件、验收标准是什么、应该遵循项目里的哪些既有模式。

然后 `/ce-work` 按计划执行。

这套循环把传统软件开发里的比例倒过来了：以前像是 80% 编码、20% 规划；现在是把真正的思考放进计划里，执行变得更机械。

## 2. 计划是给 agent 看的，不一定是给人读的

他有一个很反直觉的习惯：总是生成计划，但几乎不读完整计划。

原因是，计划的主要读者不是人，而是 agent。强制生成计划，会迫使 agent 先研究、先承诺方案、先写验收标准，然后再执行。没有计划的 coding agent 很容易偷懒、绕路、提前停下；有计划的 agent 更像是在按工程任务推进。

如果他不理解计划，就直接在会话里问：

- `TLDR?`
- `eli5 this plan`
- `wait, why this approach?`

他要的是一段可理解摘要，而不是自己坐在那里读几百行 Markdown。

## 3. 非代码知识工作也走同一套循环

原文强调，`/ce-plan` 和 `/ce-work` 不只适合写代码。战略文档、产品规格、竞品分析、董事会更新、商业问题拆解，都可以走同样的流程。

他的一个技巧是：**先让 agent 为“如何产出最终文档”写计划。**

比如他要结合一本 PDF 书、一段两小时会议转录和自己的商业问题产出一个策略文档。他不会直接让 Claude Code 写最终文档，而是先说：请先制定一个计划，说明你会如何读这本书、如何挖掘会议转录、如何把这些内容合并成一个有用文档。现在不要写最终文档，写作本身是下一步工作。

他认为这是避免 LLM 偷懒的最好办法之一。直接要交付物，模型常常会走捷径；先要求它规划“如何产出交付物”，再执行计划，结果通常更扎实。

## 4. 语音输入终于变得可用

他把语音输入看作 Agentic Engineering 的关键入口。

过去语音转文字最大的问题是转录必须很准。但对 LLM 来说，转录不完美也可以接受，因为模型能根据上下文补齐、纠错、理解你说到一半又重来的句子。

他的配置大致是：

- Mac 上用 Monologue 或 Wispr Flow，把语音输入到当前聚焦的应用；
- 手机上直接用 Apple 内置听写，因为对 LLM 说话不需要像对人发消息那样严谨；
- 办公室里配一个鹅颈麦克风；
- 在开放办公空间里，语音仍然是他的弱点，因为他不想打扰旁边的人。

这个点很真实：AI 工作流不是只有工具问题，也有场景问题。一个人在家、在车里、在办公室、在共享空间，适合的输入方式并不一样。

## 5. 一天同时跑多个 agent 会话

他的日常状态是开四到六个 `cmux` 标签页，每个标签页是一个独立 Claude Code 会话：

- 一个在写计划；
- 一个在按另一个计划构建；
- 一个在跑 `last30days` 做最近资料研究；
- 一个在修测试时发现的新 bug。

当一个窗口里的 `/ce-plan` 正在做研究，他就切到另一个窗口执行已有计划。等再切回来，第一个计划可能已经完成。

这不是“一个 agent 替你写代码”，而是“多个 agent 形成并行工作台”。人的角色变成调度、判断、验收和纠偏。

## 6. 新终端标签页直接进入 Claude Code

他希望每次打开新终端标签页时，不是进入 shell，而是直接进入 Claude Code。

理由很简单：当新会话只需要一个快捷键时，你会更频繁地启动 agent。agent 可以自己找到项目，不必每次都手动 `cd` 到目录再输入命令。

他建议让 Ghostty 的新标签页默认运行一个 `claude-launcher.sh`，先启动 Claude Code，退出后再掉回交互式 zsh。

## 7. 让每个会话都能远程接管

原文里有两个让会话随时可达的技巧。

第一个是给 Claude Code 开启自动远程控制。这样你在桌面启动的 session，走开以后也可以通过 Claude 手机 app 接上同一个运行中的任务。

第二个是给 Claude Code 一个邮箱。通过 AgentMail，邮件到达后可以自动打开一个新的 Claude session，把邮件主题、正文和附件路径交给 agent 处理。

他还开源了相关项目：一个守护进程监听邮箱，一个终端后端负责打开 cmux 或 Ghostty，一个发送端让手机也能触发任务。安全边界靠 allowlist、DKIM 和 SPF 校验。

## 8. 多会话必须减少权限确认

Claude Code 默认会对编辑和命令频繁请求确认。Matt 的观点很激进：如果同时跑六个会话，就不能一直盯着每个确认弹窗。

他会打开绕过权限的设置，也会给会话结束加一个声音 hook。这样人可以走开，听到声音再回来处理完成的任务。

他也提到 Codex 有类似模式：

```toml
approval_policy = "never"
sandbox_mode = "danger-full-access"
```

这部分不适合无脑照搬。它提高效率，也放大风险。更合理的理解是：当你真的采用多 agent 并行工作流时，权限、沙箱、回滚、版本控制和告警会变成工作流设计的一部分，而不是可有可无的设置。

## 9. Claude 负责计划，Codex 负责构建

他一天里经常把工作交给 Codex，但几乎不直接打开 Codex CLI。

他的组合方式是：Claude 做计划和判断，Codex 做大规模并行构建。交接方式包括：

- 在 Claude 会话里把任务发给 Codex；
- 用 `/ce-work --codex` 直接委派构建；
- 在 Printing Press 生成 CLI 的 prompt 末尾加上 `codex`，把构建交给 Codex。

他的个人配置是：Codex 开高推理和 fast mode；Claude Code 也开高推理，但不开 fast mode。两个订阅并行，相当于多一个工程引擎。

## 10. 规划前先查最近 30 天

在做 `/ce-plan` 之前，他经常先跑 `/last30days <topic>`。

例子是他要在 Vercel agent-browser 和 Playwright 之间选择。与其读文档，他先跑最近 30 天资料，收集 Reddit、X、YouTube、Hacker News、GitHub 等来源。结果发现 agent-browser 每次调用消耗的上下文更少，而 Playwright 的工具定义会占用大量 token。

然后他把这些新鲜资料喂给 `/ce-plan`。这样计划不是基于模型半年前的训练数据，而是基于社区最近正在讨论的实际经验。

这一点很关键：Agentic Engineering 的循环不是“问模型、写代码”，而是 **研究、计划、构建**。

## 11. 原始会议转录比人工摘要更有价值

他分享了一个招聘和产品讨论的例子：一次 90 分钟午餐会议，里面既有产品想法，也有食物、孩子等闲聊。会后他没有先人工整理，而是把 Granola 的完整原始转录直接丢给 Claude Code，让它生成产品提案计划。

重点是“原始”。不要先总结，不要先清洗，让模型自己从真实上下文里提取有价值信息，并结合代码库和历史策略文档一起处理。

后来他又做了 Printing Press Granola CLI，可以直接把会议以结构化数据拉进 session，搜索历史会议，找到几周前某个人说过的一句话，再放进计划里。

## 12. 人的价值是信号，不是打字

当你同时跑六个 agent 时，你的工作不再是亲手完成每个任务，而是成为高质量信号源。

agent 提供产量，人提供品味、方向和反馈：

- 第二个方案更接近，但用第一个方案的语言；
- 先处理最大风险；
- 这段太长；
- 这个判断不成立；
- 这个输出可以发，那个不行。

他总结得很清楚：让 agent 做手，人负责判断。

## 13. 视频也可以进入同一个循环

原文提到 HyperFrame：把视频当成 HTML 来构建，所以 agent 也能写视频。

流程和写代码类似：一个目录里有 `script.md`，按场景写脚本、动效、字幕和节奏；agent 根据脚本生成 composition 并渲染成 MP4。

![Agent Cookie Launch Video Made in HyperFrame](https://pbs.twimg.com/amplify_video_thumb/2061847550911733761/img/uqIzUcvWIZZTClBQ.jpg)

这样，产品 launch reel、demo 视频、动画讲解、带字幕短片，都可以从“剪辑工作”变成“和 agent 对话”。

## 14. 把个人知识库接给 agent

他认为 plan.md 变强的原因之一，是 Claude 能访问过去写过的计划。上下文会复利。

于是他把更多个人知识库接进 agent：

- Bear：十年的笔记、会议、想法和决策；
- Obsidian：很多人喜欢它的插件生态；
- Dropbox：跨机器同步；
- Memory 工具：作为 agent 的长期记忆层。

重点不是某个具体软件，而是选择有 CLI 或 API 的笔记工具，让 agent 能读写你的知识。

## 15. 远程机器、移动网络和长任务

他还整理了一组远程工作技巧：

- 在必须 SSH 时，用能让远程体验更像本地的工具；
- 飞机上用远程机器加 tmux，让任务跑在远端而不是笔记本上；
- 同时使用 Hermes 和 OpenClaw 处理自治远程工作；
- 同步 Mac mini 和主力 Mac 上的 cookies 与 `.env`。

这里背后的原则是：agent 任务经常是长时间运行的，不应该被本地网络、电量、会话断开轻易打断。

## 16. 用 Proof 把 plan.md 交给非终端用户审阅

plan.md 对终端用户很友好，但对同事不一定友好。Matt 提到 Every 的 Proof 可以把 plan.md 或 spec 变成一个可阅读、可评论的文档链接。

![cmux and Proof working together](https://pbs.twimg.com/media/HJ0mf4TaYAEwXev?format=jpg&name=large)

同事能在文档里评论，评论再回到 agent 工作流。这样 agentic 工作不再只是个人终端里的黑箱，也能进入团队协作。

## 17. 重复两次以上的工作，就做成 skill

他认为最大的升级不是使用 agent，而是把自己的技巧沉淀成可复用 skill。

凡是做超过两次的流程，都可以变成一个 agent 以后能反复调用的命令。做法也不是从零写，而是让 agent 参考一个已经工作的 skill，比如 Compound Engineering skill，然后照着结构为新的工作流生成一个类似的 skill。

这也是他参与开源的主要方式：很多项目本质上都是把自己高频使用的流程沉淀成 skill 或 agent-native CLI。

## 18. 用同一套循环贡献开源

他用 `/ce-plan + /ce-work` 给很多开源项目提交过真实功能 PR，并且在多个项目贡献榜上排名靠前。

![Contributors for Superpowers](https://pbs.twimg.com/media/HJ0m1fLbsAARDHG?format=png&name=large)

但他认为真正的收获不是 PR 数量，而是人。参与项目 Discord，认识维护者，结交朋友，甚至招聘到新同事。

他的建议是：选一个每天都用的工具，找到一个它真正缺少的功能，用同样的 plan/work 循环把它做出来。

## 19. 硬件和电力也会变成瓶颈

他原来的两年旧笔记本已经扛不住六个 Claude session 加 Codex 的日常负载，于是换了 M5 Max 和 64GB 内存。即便如此，新机器在这种工作负载下也可能一小时就耗完电。

![Agentic engineering hardware setup](https://pbs.twimg.com/media/HJ0moyIaYAESail?format=jpg&name=large)

所以他开始随身带大功率移动电源，并在车里准备逆变器。

这不是炫硬件，而是说明一件事：当 agent 真的成为日常生产力基础设施，算力、电池、远程机器和会话持续性都会影响产出。

## 20. Agentic Engineering 不只写代码，也做现实世界的杂事

最后一部分很有意思：他提到一组 agent-native CLI，可以包装现实世界服务，让 agent 直接替你做事。

关键在认证。新的 auth 方案可以把真实浏览器 session 交给 CLI，让 CLI 以你的登录身份行动，而不是每次重新登录。

他的例子包括：

- 让 Tesla 提前把车内温度调到 72 华氏度；
- 让 Instacart 加购商品；
- 监控比赛，只有接近关键时刻才提醒；
- 查询航班票价和积分余额，生成订票策略。

这已经超出“AI 帮我写代码”，更像是让 agent 处理日常事务、监听信息、触发动作。

## 21. 最后也要小心：这会让人上瘾

原文结尾有一个重要提醒：agent 让构建东西变得像最好玩的游戏一样。很多人因此比以往更努力工作，也更容易陷入停不下来的状态。

他说，休息、和家人朋友说话、确认有没有人真的想要你正在做的东西，都很重要。即便最后只是为自己做工具，也没问题；但不要在无尽构建里消失。

这段其实很值得放在所有 agentic workflow 教程最后：效率工具越强，越需要人自己决定什么值得做。

## 我看到的三个核心启发

第一，Agentic Engineering 的核心不是“让 AI 写代码”，而是建立一个可重复的工作系统：研究、计划、执行、审阅、复盘、沉淀成 skill。

第二，plan.md 是这套系统的中间格式。它既是任务说明，也是 checkpoint，也是团队协作对象，还是 agent 跨会话工作的上下文锚点。

第三，人没有消失。相反，人的判断更重要了。以前你的瓶颈可能是打字和实现；现在瓶颈更可能是选题、品味、验收、风险判断和持续反馈。

如果要把这篇长文压缩成一句可执行建议，我会这样写：

**把你最近一个真实任务交给 agent，先不要让它做，先让它写 plan.md；等计划明确后，再让另一个 session 按计划执行。**

这就是 Agentic Engineering 最小可用循环。
