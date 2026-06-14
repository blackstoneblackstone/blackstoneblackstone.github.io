---
layout: post
title: "使用 MCP 构建可连接生产系统的 Agent"
date: 2026-04-25 20:00:00 +0800
categories: [技术分享, MCP]
tags: [MCP, Agent, Claude, 翻译]
author: 黑石
excerpt: Claude 官方博客翻译：Agent 的价值取决于它能够触达的系统。本文介绍三种连接 Agent 到外部系统的方式，以及 MCP 如何成为生产级 Agent 的关键层。
---

> **原文**: [Building agents that reach production systems with MCP](https://claude.com/blog/building-agents-that-reach-production-systems-with-mcp)
> **来源**: Claude 官方博客
> **日期**: 2026年4月22日

Agent 的价值取决于它能够触达的系统。团队在将 Agent 连接外部系统时，通常会收敛到三种方式：直接 API 调用、CLI 和 MCP。本文将阐述每种方式的适用场景、为什么生产级 Agent 最终倾向于选择 MCP，以及有效构建这些集成的模式。

---

## 将 Agent 连接到外部系统

我们通常会看到三种将 Agent 连接到外部系统的路径：直接 API 调用、CLI 和 MCP。每种方式在某些场景下都有意义，具体取决于你在构建什么。关键区别在于 Agent 和服务之间是否存在通用层，以及这个通用层能延伸到多远。

### 直接 API 调用

Agent 直接调用你的 API——要么在代码执行沙箱内编写代码发出 HTTP 请求，要么通过一个通用的函数调用工具。这是大多数团队的起点，对于一个 Agent 与一个服务对话，或少量不需要跨 Agent 平台复用的集成来说，这种方式工作得很好。

挑战在规模化时开始出现。由于 Agent 和服务之间没有通用层，每一对 Agent-服务 组合都变成了需要独立处理认证、工具描述和边缘情况的定制集成——这就是 M×N 集成问题。

### 命令行界面 (CLI)

Agent 在 shell 中运行你的命令行工具。这种方式快速、轻量，并且可以利用已有的工具。它在本地环境和沙箱容器等任何有文件系统和 shell 的地方都运行良好。这提供了一个通用层，但非常薄。

CLI 在触达移动设备、Web 或不暴露容器的云托管平台时会遇到硬性限制，而且认证由 CLI 自己的机制处理——通常是磁盘上的凭证文件。这最适合本地环境中快速、宽松的集成。

### 模型上下文协议 (MCP)

MCP 将通用层作为一个协议来提供。Agent 连接到一个暴露你的系统能力的服务器，认证、发现和丰富的语义都被标准化了。一个远程服务器可以触达任何兼容的客户端（Claude、ChatGPT、Cursor、VS Code 等），在任何部署环境中都是如此。

它需要多一点前期投入。回报是集成的可移植性，以及功能丰富的 Agent 集成所需的语义。

---

## 生产级 Agent 运行在云端

生产级 Agent 越来越多地运行在云端，以便能够扩展和持续运行。它们需要触达的系统也在云端托管：数据所在的地方、工作被跟踪的地方、基础设施运行的地方。通常这些系统是远程的、需要认证的，而 MCP 正好提供了这个通用层。

我们已经在采用率上看到了这一点。MCP SDK 最近每月下载量超过 3 亿次，而年初还是 1 亿次，企业和流行的 Agent 平台都在广泛采用。每天有数百万人使用 MCP 与 Claude 交互，该协议支撑了我们最近发布的许多产品，包括 Claude Cowork、Claude Managed Agents 和 Claude Code 中的 channels。

随着 MCP 持续支持生产级 Agent 系统，我们正在分享有效构建这些集成的模式：从构建高级服务器到上下文高效的客户端，以及 skills 如何补充协议。

---

## 构建有效的 MCP 服务器

我们的目录中有超过 200 个 MCP 服务器，每天被数百万人使用。通过与在协议上构建的企业和开发者密切合作，我们发现了一些决定 Agent 能多可靠地使用服务器的设计模式。

### 构建远程服务器以获得最大覆盖

远程服务器是让你获得分发的关键——它是唯一能在 Web、移动和云托管 Agent 中运行的配置，也是每个主要客户端都优化消耗的配置。构建远程服务器，让 Agent 无论在哪里运行都能使用你的系统。

### 围绕意图而非端点来分组工具

更少、描述更清晰的工具始终优于详尽的 API 镜像。不要将你的 API 一对一地封装成 MCP 服务器——围绕意图来分组工具，让 Agent 能用几次调用就完成任务，而不是把许多原语拼接在一起。一个 `create_issue_from_thread` 工具胜过 `get_thread + parse_messages + create_issue + link_attachment`。

### 当接口面很大时，为代码编排而设计

如果你的服务需要数百种不同的操作，比如 Cloudflare、AWS 或 Kubernetes，基于意图分组的工具集可能无法覆盖。相反，暴露一个薄的工具面，接受代码：Agent 编写一个短脚本，你的服务器在沙箱中针对你的 API 运行它，只返回结果。Cloudflare 的 MCP 服务器是参考示例——两个工具（search 和 execute）用大约 1K token 覆盖了约 2500 个端点。

### 在有帮助的地方提供丰富的语义

MCP Apps 是第一个官方协议扩展，它让工具可以返回交互式界面，比如图表、表单或仪表盘，所有内容都内联渲染在聊天界面中。提供 MCP apps 的服务器比仅返回文本的服务器有显著更高的采用率和留存率。

**Elicitation** 让你的服务器在工具调用中途暂停，向用户请求输入。Form 模式发送一个简单的 schema，客户端渲染原生表单——用它来请求缺失的参数、确认破坏性操作或消除选项歧义。URL 模式将用户交给浏览器——用它来完成下游 OAuth、收款或收集任何不应经过 MCP 客户端的凭证。Form 模式被广泛支持；URL 模式在 Claude Code 中受支持，更多客户端正在接入中。

### 依赖标准化的认证

标准化认证让 MCP 对云托管 Agent 变得实用。如果你的服务器需要 OAuth，最新的 MCP 规范支持 CIMD（Client ID Metadata Documents）用于客户端注册——它为用户提供了快速的首页认证流程，以及更少的意外重新认证提示。

用户授权后，Claude Managed Agents 中的 Vaults 解决了运行时 token 管理的问题：一次性注册用户的 OAuth token，在创建会话时通过 ID 引用 vault，平台会将正确的凭证注入每个 MCP 连接并替你刷新——无需构建密钥存储，无需每次调用传递 token。

---

## 让 MCP 客户端更上下文高效

MCP 标准化了 AI Agent（客户端）如何连接和使用它们需要的工具和数据源（服务器）。服务器安全地暴露一系列能力，而客户端编排它们并管理上下文。如果你正在构建 MCP 客户端，可以通过渐进式披露的模式让它更上下文高效。

### 通过工具搜索按需加载工具定义

工具搜索将所有工具延迟加载到上下文中，而不是预先加载。这让 Agent 可以在运行时搜索目录，在需要时拉取相关工具。在我们的测试中，工具搜索通常能将工具定义的 token 减少 85% 以上，同时保持较高的选择准确率。

### 通过程序化工具调用在代码中处理工具结果

程序化工具调用在代码执行沙箱中处理工具结果，而不是将原始结果返回给模型。这让 Agent 可以在代码中跨调用循环、过滤和聚合，只有最终输出进入上下文。在我们的测试中，这在复杂的多步骤工作流中将 token 使用量减少约 37%。

![通过工具搜索减少上下文使用](/assets/images/mcp-context-usage.webp)
*通过工具搜索减少上下文使用。来源：[advanced tool use](https://claude.com/blog/advanced-tool-use)*

这些模式自然地跨多个服务器组合：更精简的上下文、更少的往返、更快的响应。

---

## 将 MCP 服务器与 Skills 配对

Skills 和 MCP 是互补的。MCP 让 Agent 访问外部系统的工具和数据，而 skills 教会 Agent 如何使用这些工具完成实际工作的程序性知识。最有能力的 Agent 同时使用两者，而 skills 让 MCP 服务器超越少数连接的限制。

### 将 skills 和 MCP 服务器打包为插件

Claude 的插件是一种有用的抽象，让开发者能将 skills、MCP 服务器、hooks、LSP 服务器和专门的子 Agent 打包到一个易于消费的分发方式中。使用这种方法是统一多个上下文提供者的最佳方式，摩擦最小。

将 MCP 服务器与 skills 结合，让 Claude 更像一个领域专家。通过 MCP 获取工具，然后给 Claude 从头到尾编排工作流的 skills。

![将 Skills 与 MCP 结合](/assets/images/mcp-skills-work-together.png)
*将 Skills 与 MCP 结合。来源：[Extending Claude's capabilities with skills and MCP servers](https://claude.com/blog/extending-claude-with-skills-and-mcp-servers)*

### 从 MCP 服务器分发 skills

提供商越来越常见地在其 MCP 服务器旁边发布一个 skill，这样 Agent 既能获得原始能力，也能获得如何良好使用它们的有主见的使用指南。Canva、Notion、Sentry 等今天在 Claude 中就是这样做的。

为了让这种配对在每个客户端之间可移植，MCP 社区正在积极开发一个扩展，用于直接从服务器交付 skills。这样客户端自动继承相关专业知识，版本与它所依赖的 API 保持一致。我们预计随着扩展的稳定，这种模式将被广泛采用。

---

## 复合层

我们开篇介绍了三种连接 Agent 与外部系统的路径。在实践中，成熟的集成会同时提供三种：API 作为基础，CLI 用于本地优先环境，MCP 用于基于云的 Agent。

随着生产级 Agent 向云端迁移，MCP 成为关键层，而且是那个具有复合效应的层。今天，一个远程服务器可以在任何部署环境中触达每个兼容的客户端，认证、交互性和丰富的语义都由协议处理。随着更多客户端采用规范、更多扩展加入其中，同一个服务器会变得越来越强大，而你无需发布任何新内容。

在构建集成时，如果你的目标是让云端的生产级 Agent 触达你的系统，就构建一个 MCP 服务器，并使用上述模式让它变得卓越。每一个基于 MCP 构建的集成都在强化生态系统：需要独自解决的边缘情况更少，需要维护的定制集成更少。

---

## 致谢

感谢 Den Delimarsky、David Soria Parra、Henry Shi、Felix Rieseberg、Conor Kelly、Molly Vorwerck、Andy Schumeister、Kevin Garcia、Amie Rotherham、Matt Samuels、Angela Jiang、Katelyn Lesse、AJ Rebeiro 和 Jess Yan 对本博客的贡献。
