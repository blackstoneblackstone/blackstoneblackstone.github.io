---
layout: post
title: "Claude Skill 官方构建指南"
date: 2026-03-18 15:00:00 +0800
categories: [文章]
tags: [AI, Claude, Skills]
author: 黑石
comments: true
excerpt: Claude 官方白皮书：从构思到落地的 Skill 要点。
---

# 介绍

Skill 是一套打包为简单文件夹的指令，它能教会 Claude 如何处理特定任务或工作流，是为你的特定需求定制 Claude 最强大的方式之一。你不需要在每一次对话中重复解释你的偏好、流程和领域专业知识，Skill 可以让你只教一次 Claude，每次使用都能生效。

当你有可重复的工作流时，Skill 就能发挥巨大作用：根据需求生成前端设计、用统一的方法开展研究、创建符合团队风格指南的文档，或是编排多步骤流程。它可以和 Claude 内置的代码执行、文档创建等能力完美配合。对于正在构建 MCP 集成的开发者来说，Skill 还能新增一个强大层，帮助把原始的工具访问转化为可靠、优化的工作流。

本指南涵盖了构建高效 Skill 所需的全部内容——从规划、结构设计到测试和分发。无论你是为自己、团队还是社区构建 Skill，都能在这里找到实用的模式和真实案例。

你将学到：
- Skill 结构的技术要求和最佳实践
- 独立 Skill 与 MCP 增强工作流的实现模式
- 不同使用场景下经过验证的成熟模式
- 如何测试、迭代和分发你的 Skill

适合阅读本指南的人群：
- 希望 Claude 始终遵循特定工作流的开发者
- 希望 Claude 遵循特定工作流的高级用户
- 希望在整个组织统一 Claude 工作方式的团队

## 两种阅读路径

如果你构建**独立 Skill**，请重点关注「基础概念」、「规划与设计」和前两类用例。如果你要**增强 MCP 集成**，请阅读「Skill+MCP」部分和第三类用例。两条路径共享相同的技术要求，你可以根据自己的使用场景选择相关内容阅读。

读完本指南后你将收获：你可以在一次会话中就构建出可用的 Skill。使用 Skill 创建工具构建并测试你的第一个可用 Skill，预计只需要 15 ～ 30 分钟。

让我们开始吧。

---

# 第一章 基础概念

## 什么是 Skill？

Skill 是一个包含以下内容的文件夹：
- **SKILL.md**（必填）：带 YAML 前置元数据的 Markdown 格式指令
- **scripts/**（可选）：可执行代码（Python、Bash 等）
- **references/**（可选）：按需加载的文档
- **assets/**（可选）：输出用到的模板、字体、图标

## 渐进式披露

Skill 采用三级加载机制：
1. **一级（YAML 前置元数据）**：始终加载在 Claude 的系统提示中，只提供刚好够用的信息，让 Claude 知道何时应该使用该 Skill，不会把全部内容加载到上下文里
2. **二级（SKILL.md 正文）**：当 Claude 判断该 Skill 和当前任务相关时加载，包含完整的指令和指南
3. **三级（链接文件）**：捆绑在 Skill 目录中的额外文件，只有在需要时 Claude 才会导航访问

这种渐进式披露机制可以在保持专业能力的同时最大限度减少 token 占用。

## 可组合性

Claude 可以同时加载多个 Skill，你的 Skill 应该能和其他 Skill 良好配合，而不是假设自己是唯一可用的能力。

## 可移植性

Skill 在 Claude.ai、Claude Code 和 API 上的表现完全一致。只要环境支持 Skill 所需的依赖，一次创建就可以在所有平台使用，不需要修改。

---

## 给 MCP 构建者：Skill + 连接器

💡 如果你是构建不依赖 MCP 的独立 Skill，可以直接跳转到「规划与设计」，之后随时可以回来阅读这部分。

如果你已经有可用的 MCP 服务器，那最难的部分已经完成了。Skill 就是叠加在之上的知识层——把你已经掌握的工作流和最佳实践沉淀下来，让 Claude 可以始终一致地应用它们。

## 厨房类比

- MCP 提供专业厨房：连接工具、食材和设备的入口
- Skill 提供烹饪食谱：一步步指导你如何创造有价值的成果

两者结合就能让用户不用自己拆解每一步就能完成复杂任务：

| MCP（连接能力）| Skill（知识能力）|
| ---- | ---- |
| 有效连接 Claude 到你的服务（Notion、Asana、Linear 等）| 教会 Claude 如何使用你的服务 |
| 提供实时数据访问和工具调用 | 沉淀工作流和最佳实践 |
| 说明 Claude **能做什么** | 说明 Claude **该怎么做** |

## 为什么这对你的 MCP 用户很重要

**没有 Skill 时：**
- 用户连接了你的 MCP，但不知道接下来该做什么
- 出现大量求助工单：「我怎么用你的集成做 X」
- 每次对话都要从头开始
- 因为用户每次提示方式不同，结果不稳定
- 用户会把流程指导缺失导致的问题归咎于你的连接器

**有了 Skill 后：**
- 需要时预构建工作流会自动激活
- 工具使用始终一致可靠
- 每次交互都内嵌最佳实践
- 降低你的集成学习门槛

---

# 第二章 规划与设计

## 从用例开始

在写任何代码之前，先确定 2 ～ 3 个你的 Skill 应该支持的具体用例。

好的用例定义示例：
> **用例：** 项目冲刺规划
> **触发条件：** 用户说「帮我规划这个冲刺」或「创建冲刺任务」
> **步骤：**
> 1. 通过 MCP 从 Linear 获取当前项目状态
> 2. 分析团队速度和承载力
> 3. 给出任务优先级建议
> 4. 在 Linear 创建带正确标签和预估的任务
> **结果：** 创建好任务的完整冲刺规划

问自己这些问题：
- 用户想要完成什么目标？
- 这个过程需要哪些多步工作流？
- 需要哪些工具（内置工具还是 MCP 工具？）
- 需要嵌入哪些领域知识或最佳实践？

---

## 常见 Skill 用例分类

在 Anthropic，我们观察到三类常见用例：

## 类别1：文档与资源创建

**适用场景**：创建一致、高质量的输出，包括文档、演示文稿、应用、设计、代码等
**真实示例**：`frontend-design`Skill（同时也支持 docx、pptx、xlsx、pptSkill）
> "创建与众不同、生产级的高质量前端界面。在构建 web 组件、页面、成果、海报或应用时使用。"

**核心方法**：
- 内嵌样式指南和品牌标准
- 模板结构保证输出一致性
- 最终输出前的质量检查清单
- 不需要外部工具——使用 Claude 内置能力即可

## 类别2：工作流自动化

**适用场景**：需要统一方法的多步流程，包括跨多个 MCP 服务器的协作
**真实示例**：`skill-creator`Skill
> "创建新 Skill 的交互式指南。引导用户完成用例定义、前置元数据生成、指令编写和验证。"

**核心方法**：
- 带验证关卡的分步工作流
- 通用结构模板
- 内置评审和改进建议
- 迭代优化循环

## 类别 3：MCP 增强

**适用场景**：为 MCP 服务器提供的工具访问增强工作流指导
**真实示例**：Sentry 的`sentry-code-review`Skill
> "通过 Sentry 的 MCP 服务器，使用错误监控数据自动分析并修复 GitHub 拉取请求中检测到的 bug。"

**核心方法**：
- 按顺序协调多个 MCP 调用
- 内嵌领域专业知识
- 提供用户原本需要手动指定的上下文
- 常见 MCP 问题的错误处理

---

## 定义成功标准

你怎么知道你的 Skill 能用？这些是期望目标——是粗略基准，不是精确阈值。目标力求严谨，但接受一定程度的经验判断，我们目前正在开发更完善的测量指南和工具。

## 量化指标：

- 90% 相关查询都会触发 Skill
  - 测量方法：运行 10 ～ 20 个应该触发 Skill 的测试查询，统计自动加载和需要手动调用的次数
- 在 X 次工具调用内完成工作流
  - 测量方法：对比开启和关闭 Skill 时完成同一任务的工具调用次数和总 token 消耗
- 每个工作流 0 次 API 调用失败
  - 测量方法：测试运行时监控 MCP 服务器日志，统计重试率和错误码

## 定性指标：

- 用户不需要提示 Claude 下一步该做什么
  - 评估方法：测试过程中，统计你需要纠正或澄清的频率，邀请 beta 用户提供反馈
- 不需要用户修正就能完成工作流
  - 评估方法：把同一个请求运行 3 ～ 5 次，对比输出的结构一致性和质量
- 不同会话结果始终一致
  - 评估方法：新用户只需少量指导就能第一次就完成任务吗？

---

## 技术要求

## 文件结构

```
your-skill-name/
├── SKILL.md # 必填——Skill主文件

├── scripts/ # 可选——可执行代码

│   ├── process_data.py # 示例

│   └── validate.sh # 示例

├── references/ # 可选——参考文档

│   ├── api-guide.md # 示例

└── assets/ # 可选——模板等资源

    └── report-template.md # 示例

```

## 核心规则

### SKILL.md 命名：

- 必须完全是 `SKILL.md`（大小写敏感）
- 不接受任何变体（`SKILL.MD`、`skill.md`等都不行）

### Skill 文件夹命名：

- 使用烤肉串命名法（kebab-case，全小写单词用横杠分隔）：`notion-project-setup`
- 不能有空格：`Notion Project Setup` ❌
- 不能有下划线：`notion_project_setup` ❌
- 不能有大写：`NotionProjectSetup` ❌

### 不能放 README.md：

- 不要在 Skill 文件夹内放 README.md
- 所有文档都放到`SKILL.md`或`references/`目录下
- 提示：通过 GitHub 分发时，你仍然可以在仓库根放给人类看的 README——参见「分发与分享」

---

## YAML 前置元数据：最重要的部分

YAML 前置元数据是 Claude 决定是否加载你 Skill 的依据，一定要写对。

最简必填格式：
```yaml
name: your-skill-name
description: What it does. Use when user asks to [specific phrases].
```

### 字段要求

**name（必填）：**
- 只能用烤肉串命名法
- 不能有空格和大写
- 应该和文件夹名称一致

**description（必填）：**
- 必须同时包含：
 - Skill 能做什么
 - 什么时候使用它（触发条件）
- 长度不超过1024 个字符
- 不能包含 XML 标签（`<`或`>`）
- 需要包含用户可能会说的具体任务关键词
- 如果和文件类型相关，要提及文件类型

**license（可选）：**
- 开源 Skill 可以使用
- 常见协议：MIT、Apache-2.0

**compatibility（可选）**
- 长度 1 ～ 500 个字符
- 说明环境要求：例如适用产品、需要的系统包、网络访问需求等

**metadata（可选）**
- 任意自定义键值对
- 建议填写：作者、版本、关联 mcp 服务器
- 示例：
```yaml
metadata: 
  author: ProjectHub 
  version: 1.0.0
  mcp-server: projecthub
```

## 安全限制

前置元数据中禁止出现：
- XML 尖括号（`< >`）
- 名称中包含`claude`或`anthropic`（保留名称）

原因：前置元数据会出现在 Claude 的系统提示中，恶意内容可能会注入指令。

---

## 编写高效 Skill

## description 字段

根据 Anthropic 工程博客的说法：「这些元数据... 只提供刚好够用的信息，让 Claude 知道什么时候应该使用 Skill，不需要把全部内容加载到上下文。」这是渐进式披露的第一层。

**结构：** `[Skill 做什么] + [什么时候用] + [核心能力]`

好描述示例：

```yaml
# 好的——具体可执行

description: Analyzes Figma design files and generates developer handoff documentation. Use when user uploads .fig files, asks for "design specs", "component documentation", or "design-to-code handoff".

# 好的——包含触发短语

description: Manages Linear project workflows including sprint planning, task creation, and status tracking. Use when user mentions "sprint", "Linear tasks", "project planning", or asks to "create tickets".

# 好的——清晰价值主张

description: End-to-end customer onboarding workflow for PayFlow. Handles account creation, payment setup, and subscription management. Use when user says "onboard new customer", "set up subscription", or "create PayFlow account".
```

坏描述示例：

```yaml
# 太模糊

description: Helps with projects.

# 缺少触发条件

description: Creates sophisticated multi-page documentation systems.

# 太技术化，没有用户触发词

description: Implements the Project entity model with hierarchical relationships.
```

---

## 编写主指令

前置元数据之后，用 Markdown 编写实际指令即可。

推荐结构，你可以适配这个模板，把括号内容替换成你的具体内容：

```markdown
---
name: your-skill
description: [...]
---
# 你的Skill名称

# 指令

## 第一步：[第一个主要步骤]

清晰说明会发生什么。
示例：
bash
python scripts/fetch_data.py --project-id PROJECT_ID

预期输出：[描述成功是什么样的]
(需要的话添加更多步骤)

# 示例

## 示例1：[常见场景]

用户说：「Set up a new marketing campaign」
操作：
1. 通过MCP获取现有活动
2. 用提供的参数创建新活动
结果：创建了活动，提供确认链接
(需要的话添加更多示例)

# 问题排查

错误：[常见错误信息]
原因：[为什么会发生]
解决方法：[怎么修复]
(需要的话添加更多错误案例)

```

---

## 指令编写最佳实践

### 具体、可执行

好示例：
```
运行 `python scripts/validate.py --input {filename}` 检查数据格式。
如果验证失败，常见问题包括：
- 缺少必填字段（把它们添加到CSV里）
- 日期格式无效（使用YYYY-MM-DD格式）
```

❌ 坏示例：
```
在继续之前验证数据。
```

### 包含错误处理

```
# 常见问题

## MCP 连接失败

如果你看到「Connection refused」：
1. 验证MCP服务器正在运行：检查设置>扩展
2. 确认API密钥有效
3. 尝试重新连接：设置>扩展 > [你的服务] > 重新连接
```

### 清晰引用绑定资源

```
编写查询之前，请参考 `references/api-patterns.md` 获取：
- 限流指南
- 分页模式
- 错误码和处理方法
```

### 使用渐进式披露

让 SKILL.md 专注于核心指令，把详细文档移动到 `references/` 然后添加链接即可（参见核心设计原则了解三级系统工作原理）。

---

# 第三章 测试与迭代

根据你的需求，你可以用不同严格程度测试 Skill：
- 在 Claude.ai 手动测试——直接运行查询观察行为，迭代快速，不需要配置
- 在 Claude Code 做脚本测试——自动化测试用例，变更后可重复验证
- 通过 Skill API 做程序化测试——构建评估套件，针对定义好的测试集系统化运行

选择和你的质量要求、Skill 可见度匹配的方法即可。小团队内部使用的 Skill，和部署给数千企业用户的 Skill，测试需求本来就不一样。

**专业提示：在扩展之前先在单个任务上迭代**

我们发现，最高效的 Skill 创建者会先在单个有挑战的任务上反复迭代，直到 Claude 成功，然后把这个成功方法提取成 Skill。这种方式利用了 Claude 的上下文内学习能力，比宽泛测试能更快得到反馈。有了可用基础之后，再扩展到多个测试用例保证覆盖度就好。

---

## 推荐测试方法

根据早期经验，高效的 Skill 测试通常覆盖三个方面：
## 1. 触发测试

**目标**：确保 Skill 在正确的时机加载。

**测试用例：**
- 明显任务能触发
- 改写表述的请求也能触发
- 不相关主题不会触发

**测试集示例：**
应该触发：
- "帮我新建一个 ProjectHub 工作区"
- "我需要在 ProjectHub 里创建一个项目"
- "初始化一个 Q4 规划的 ProjectHub 项目"

不应该触发：
- "旧金山今天天气怎么样？"
- "帮我写 Python 代码"
- "创建一个电子表格"（除非 ProjectHubSkill 本身处理表格）

## 2. 功能测试

**目标**：验证 Skill 产生正确输出。

**测试用例：**
- 生成了合法输出
- API 调用成功
- 错误处理生效
- 覆盖边界案例

**示例：**
> 测试：创建带5 个任务的项目
> 给定条件：项目名称「Q4 Planning」，5 个任务描述
> 当 Skill 执行工作流：
> 预期结果：
> - ProjectHub 中创建了项目
> - 创建了5 个带正确属性的任务
> - 所有任务都关联到项目
> - 没有 API 错误

## 3. 性能对比

**目标**：证明 Skill 比基线结果更好。使用定义成功标准里的指标即可，下面是一个对比示例。

| 对比项 | 无 Skill | 有 Skill |
| ---- | ---- | ---- |
| 自动工作流执行 | ❌ 需要手动引导 | ✅ 自动执行 |
| 澄清问题数量 | 5 个 | 2 个 |
| API 调用失败次数 | 2 次 | 0 次 |
| 总 token 消耗 | 8200 | 6000 |

---

## 使用 Skill 创建工具（skill-creator skill）

Skill 创建工具可以在 Claude.ai 插件目录获取，也可以下载给 Claude Code 使用，它能帮你构建和迭代 Skill。如果你有一个 MCP 服务器，并且明确了最核心的 2 ～ 3 个工作流，你就可以在一次会话中构建并测试出可用 Skill，通常只需要 15 ～ 30 分钟。

它的能力：
- 从自然语言描述生成 Skill
- 生成格式正确的带前置元数据的 SKILL.md
- 建议触发短语和结构
- 评审 Skill：标记常见问题（模糊描述、缺失触发、结构问题）
- 识别过度/不足触发风险
- 根据 Skill 的目标用途建议测试用例
- 迭代优化：使用 Skill 遇到边界案例或失败后，可以把这些案例带回给 Skill 创建工具
 - 示例：「用本次对话中发现的问题和解决方案，改进 Skill 处理[特定边界案例]的方式」

使用方式：
```
Use the skill-creator skill to help me build a skill for [你的用例]
```

提示：Skill 创建工具帮你设计和优化 Skill，但不会执行自动化测试套件，也不会生成量化评估结果。

---

## 基于反馈迭代

Skill 是活文档，需要根据以下情况计划迭代：

## 不足触发信号：

- 该加载的时候 Skill 不加载
- 用户手动开启 Skill
- 关于什么时候用这个 Skill 的求助问题
**解决方案**：给 description 添加更多细节，包括技术术语相关的关键词。

## 过度触发信号：

- 不相关查询也会加载 Skill
- 用户主动禁用 Skill
- 对 Skill 用途感到困惑
**解决方案**：添加负面触发，描述更具体。

---

# 第四章 分发与分享

Skill 让你的 MCP 集成更完整，用户对比连接器时，带 Skill 的连接器能更快创造价值，比只用 MCP 的方案更有优势。

## 当前分发模式（2026年1月）

个人用户获取 Skill 的流程：
1. 下载 Skill 文件夹
2. 压缩文件夹（如果需要）
3. 上传到 Claude.ai：设置 > 功能 > Skill
4. 或者放到 Claude Code 的 Skill 目录中

组织级 Skill：
- 管理员可以全工作区部署（2025 年12 月18 日上线）
- 自动更新
- 集中管理

---

## 开放标准

我们已经把 AgentSkill 发布为开放标准。和 MCP 一样，我们相信 Skill 应该可以跨工具和平台移植——同一个 Skill 不管你用 Claude 还是其他 AI 平台都能用。当然，有些 Skill 就是为了充分利用特定平台能力设计的，作者可以在 Skill 的`compatibility`字段说明这一点。我们一直在和生态成员协作完善这个标准，也对早期采用感到兴奋。

---

## 通过 API 使用 Skill

这个意思是自己用 Agent SDK 去搭建 Agent ，然后调用 Claude 的接口获取 skills 列表的方式。
对于程序化使用场景，比如构建应用、智能体或自动化工作流，API 可以直接控制 Skill 管理和执行。

核心能力：
- `/v1/skills` 端点用于列出和管理 Skill
- 通过 `container.skills` 参数把 Skill 添加到 Messages API 请求中
- 通过 Claude 控制台进行版本控制和管理
- 配合 Claude Agent SDK 构建自定义智能体

何时用 API vs Claude.ai 使用 Skill：

| 使用场景 | 最佳平台 |
| ---- | ---- |
| 终端用户直接和 Skill 交互 | Claude.ai / Claude Code |
| 开发过程中手动测试和迭代 | Claude.ai / Claude Code |
| 个人、临时工作流 | Claude.ai / Claude Code |
| 程序化使用 Skill 的应用 | API |
| 大规模生产部署 | API |
| 自动化流水线和智能体系统 | API |

提示：API 中的 Skill 需要代码执行工具 beta，它提供 Skill 运行所需的安全环境。

实现细节请参考：
- Skill API 快速开始
- 创建自定义 Skill
- Agent SDK 中的 Skill

---

## 目前推荐的方法

先把你的 Skill 托管在 GitHub，用公开仓库，放清晰的 README（给人类看的，和 Skill 文件夹分开，Skill 文件夹不能放 README.md），带截图的使用示例。然后在你的 MCP 文档中添加一个章节，链接到 Skill，说明一起使用的价值，提供快速开始指南。

1. 托管到 GitHub
- 开源 Skill 用公开仓库
- 带安装说明的清晰 README
- 使用示例和截图

2. 在你的 MCP 仓库文档说明
- 从 MCP 文档链接到 Skill
- 说明一起使用的价值
- 提供快速开始指南

3. 创建安装指南
```
# Installing the [Your Service] skill

1. Download the skill:
 - Clone repo: `git clone https: /github.com/yourcompany/
 skills`
 - Or download ZIP from Releases

2. Install in Claude:
 - Open Claude.ai > Settings > skills
 - Click "Upload skill"
 - Select the skill folder (zipped)

3. Enable the skill:
 - Toggle on the [Your Service] skill
 - Ensure your MCP server is connected

4. Test:
 - Ask Claude: "Set up a new project in [Your Service]"
```

---

## 描述你的 Skill

你怎么描述 Skill，决定了用户能否理解它的价值并真正尝试。在 README、文档或推广内容中描述 Skill 时，请记住这些原则。

**聚焦结果，而非功能：**
好示例：
> ProjectHubSkill 能让团队在几秒钟内搭建完整项目工作区——包括页面、数据库和模板，不用花 30分钟手动配置。

❌ 坏示例：
> ProjectHubSkill 是一个包含 YAML 前置元数据和 Markdown 指令的文件夹，会调用我们的 MCP 服务器工具。

**突出 MCP+Skill 组合价值：**
> 我们的 MCP 服务器让 Claude 访问你的 Linear 项目，我们的 Skill 教会 Claude 你的团队冲刺规划工作流，两者结合就能实现 AI 驱动的项目管理。

---

# 第五章 模式与问题排查

这些模式来自早期采用者和内部团队创建的 Skill，它们是我们见过的通用有效方法，不是强制模板。

## 选择方向：问题优先还是工具优先

就像家得宝（美国建材零售商），你可能带着问题进门——「我需要修橱柜」，员工会给你指向正确的工具。或者你选了一个新电钻，想知道怎么用它完成你手头的工作。

Skill 也是一样：
- **问题优先**：「我需要搭建项目工作区」→ Skill 编排正确顺序的 MCP 调用，用户描述结果，Skill 处理工具。
- **工具优先**：「我已经连接了 Notion MCP」→ Skill 教会 Claude 最优工作流和最佳实践，用户已经有工具访问，Skill 提供专业知识。

大多数 Skill 都会偏向一个方向，知道哪种框架适合你的用例，能帮你选择下面正确的模式。

---

## 模式1：顺序工作流编排

**适用场景**：用户需要按特定顺序执行多步流程。

**示例结构：**
```markdown
# 工作流：新客户 onboard

## 第一步：创建账户

调用MCP工具：`create_customer`
参数：name, email, company

## 第二步：配置支付

调用MCP工具：`setup_payment_method`
等待：支付方式验证完成

## 第三步：创建订阅

调用MCP工具：`create_subscription`
参数：plan_id, customer_id（来自第一步）

## 第四步：发送欢迎邮件

调用MCP工具：`send_email`
模板：welcome_email_template
```

**核心方法：**
- 明确步骤顺序
- 步骤之间的依赖关系
- 每个阶段做验证
- 失败时的回滚指令

---

## 模式2：多MCP协作

**适用场景**：工作流跨多个服务。

**示例：设计转开发交接**
```markdown
## 阶段一：设计导出（Figma MCP）

1. 从Figma导出设计资源
2. 生成设计规范
3. 创建资源清单

## 阶段二：资源存储（Drive MCP）

1. 在Drive创建项目文件夹
2. 上传所有资源
3. 生成可分享链接

## 阶段三：任务创建（Linear MCP）

1. 创建开发任务
2. 把资源链接附加到任务
3. 分配给工程团队

## 阶段四：通知（Slack MCP）

1. 在#engineering频道发布交接摘要

2. 包含资源链接和任务引用
```

**核心方法：**
- 清晰阶段划分
- MCP 之间传递数据
- 进入下一阶段前做验证
- 集中错误处理

---

## 模式3：迭代优化

**适用场景**：输出质量通过迭代提升。

**示例：报告生成**
```markdown
# 迭代式报告创建

## 初始草稿

1. 通过MCP获取数据
2. 生成第一版报告草稿
3. 保存到临时文件

## 质量检查

1. 运行验证脚本：`scripts/check_report.py`
2. 识别问题：
- 缺失章节
- 格式不一致
- 数据验证错误

## 优化循环

1. 解决每个识别出的问题
2. 重新生成受影响章节
3. 重新验证
4. 重复直到满足质量阈值

## 最终完成

1. 应用最终格式
2. 生成摘要
3. 保存最终版本
```

**核心方法：**
- 明确质量标准
- 迭代改进
- 验证脚本
- 知道什么时候停止迭代

---

## 模式4：上下文感知工具选择

**适用场景**：同样目标，根据上下文选择不同工具。

**示例：文件存储**
```markdown
# 智能文件存储

## 决策树

1. 检查文件类型和大小
2. 确定最佳存储位置：
- 大文件（>10MB）：使用云存储MCP
- 协作文档：使用Notion/Docs MCP
- 代码文件：使用GitHub MCP
- 临时文件：使用本地存储

## 执行存储

根据决策结果：
- 调用对应MCP工具
- 添加服务专属元数据
- 生成访问链接

## 给用户说明上下文

解释为什么选择这个存储
```

**核心方法：**
- 清晰决策标准
- 降级选项
- 选择透明化

---

## 模式5：领域特定智能

**适用场景**：Skill 在工具访问之外添加专业知识。

**示例：金融合规**
```markdown
# 带合规检查的支付处理

## 处理前（合规检查）

1. 通过MCP获取交易详情
2. 应用合规规则：
- 检查制裁名单
- 验证司法辖区允许
- 评估风险等级
3. 记录合规决策

## 处理

如果合规通过：
- 调用支付处理MCP工具
- 应用适当欺诈检查
- 处理交易
否则：
- 标记待审核
- 创建合规案例

## 审计追踪

- 记录所有合规检查
- 记录处理决策
- 生成审计报告
```

**核心方法：**
- 把领域专业知识内嵌到逻辑中
- 行动前先做合规检查
- 完整文档记录
- 清晰管控流程

---

## 问题排查

### Skill 无法上传

**错误：**「在上传文件夹中找不到 SKILL.md」
**原因：**文件没有精确命名为`SKILL.md`
**解决：**
- 重命名为`SKILL.md`（大小写敏感）
- 用`ls -la`命令验证可以看到`SKILL.md`

**错误：**「前置元数据无效」
**原因：**YAML 格式错误
常见错误：

```yaml
# 错误——缺少分隔符

name: my-skill
description: Does things

# 错误——引号未闭合

name: my-skill
description: "Does things

# 正确

name: my-skill
description: Does things
```

**错误：**「Skill 名称无效」
**原因：**名称包含空格或大写字母

```yaml
# 错误

name: My Cool Skill
# 正确 

name: my-cool-skill
```

---

### Skill 不触发

**症状：** Skill 从来不会自动加载
**修复：** 修改描述字段，参考「描述字段」部分的好坏示例。

快速检查清单：
- 描述是不是太泛泛？（「对项目有帮助」无法正常工作）
- 是否包含用户实际会说的触发短语？
- 如果和文件类型相关，是否提到了对应文件类型？

调试方法：
问 Claude：「你什么时候会使用[Skill 名称]Skill？」Claude 会引用描述，你可以根据缺失内容调整。

---

### Skill 触发太频繁

**症状：** 不相关查询也会加载 Skill
**解决方案：**
1. 添加否定触发
```yaml
description: Advanced data analysis for CSV files. Use for statistical modeling, regression, clustering. Do NOT use for simple data exploration (use data-viz skill instead).
```
> 翻译：CSV 文件高级数据分析，用于统计建模、回归、聚类。不要用于简单数据探索（改用 data-vizSkill）。

2. 描述写得更具体
```yaml
# 过于宽泛

description: Processes documents
# 更具体

description: Processes PDF legal documents for contract review
```
翻译：处理 PDF 法律文档做合同审查。

3. 明确范围
```yaml
description: PayFlow payment processing for e-commerce. Use specifically for online payment workflows, not for general financial queries.
```
翻译：电商 PayFlow 支付处理，仅用于在线支付工作流，不用于通用金融查询。

---

### MCP 连接问题

**症状：** Skill 能加载但 MCP 调用失败
**检查清单：**
1. 确认 MCP 服务器已连接
 - Claude.ai：设置 > 扩展 > [你的服务]应该显示「已连接」状态
2. 检查认证
 - API 密钥有效且未过期
 - 授予了正确的权限/范围
 - OAuth 令牌已刷新
3. 独立测试 MCP
 - 让 Claude 直接调用 MCP（不通过 Skill）
 - 「使用[服务]MCP 获取我的项目」
 - 如果这一步失败，问题出在 MCP 而非 Skill
4. 验证工具名称
 - Skill 引用了正确的 MCP 工具名称
 - 查看 MCP 服务器文档确认
 - 工具名称大小写敏感

---

### 指令不被遵守

**症状：** Skill 能加载但 Claude 不遵循指令
**常见原因：**
1. 指令太冗长
 - 保持指令简洁
 - 使用项目符号和编号列表
 - 把详细参考移动到独立文件

2. 指令被淹没
 - 把关键指令放在顶部
 - 使用`# 重要`或`# 关键`标题
 - 需要时可以重复关键点

3. 语言模糊
❌ 坏示例：
> 确保正确验证内容

✅ 好示例：
> **关键：** 调用`create_project`之前，必须验证：
> - 项目名称非空
> - 至少分配了一个团队成员
> - 开始日期不在过去

**高级技巧：** 对于关键验证，可以考虑打包一个脚本以编程方式完成检查，而不是依赖自然语言指令。代码是确定性的，而语言解释存在不确定性，可以参考 OfficeSkill 查看这种模式的示例。

4. 模型「惰性」：添加明确提示

```markdown
# 性能说明

- 慢慢来，彻底完成
- 质量比速度更重要
- 不要跳过验证步骤
```
提示：把这句话加在用户提示里比写在 SKILL.md 里更有效。

---

### 大上下文问题

**症状：** Skill 反应缓慢或响应质量下降

**原因：**
- Skill 内容太大
- 同时启用了太多 Skill
- 没有使用渐进式披露，加载了全部内容

**解决方案：** 
1. 优化 SKILL.md 大小
 - 把详细文档移动到`references/`
 - 链接到参考文件而不是内联
 - 保持 SKILL.md 在5000 词以内
2. 减少同时启用的 Skill 数量
 - 评估你是否同时启用了超过 20 ～ 50 个 Skill
 - 建议选择性启用
 - 考虑把相关能力打包为 Skill 包

---

# 第六章 资源与参考

如果你是第一次构建 Skill，从最佳实践指南开始，然后按需参考 API 文档。

## 官方文档

Anthropic 资源：
- [最佳实践指南](https://modelcontextprotocol.io/docs/getting-started/intro)
- [Skill 文档](https://claude.com/app-unavailable-in-region)
- [API 参考](https://claude.com/app-unavailable-in-region)
- [MCP 文档](https://claude.com/app-unavailable-in-region)

博客文章：
- [《AgentSkill 介绍》](https://claude.com/blog/skills)
- [工程博客：《为智能体装备能力，应对真实世界》](https://claude.com/blog/equipping-agents-for-the-real-world-with-agent-skills)
- [Skill 详解](https://claude.com/blog/skills-explained)
- [《如何为 Claude 创建 Skill》](https://claude.com/blog/how-to-create-skills-key-steps-limitations-and-examples)
- [《为 Claude Code 构建 Skill》](https://claude.com/blog/building-skills-for-claude-code)
- [《通过 Skill 提升前端设计质量》](https://claude.com/blog/improving-frontend-design-through-skills)

---

## 示例 Skill

公共 Skill 仓库：
- GitHub：`anthropics/skills`
- 包含 Anthropic 创建的 Skill，你可以自定义修改

---

## 工具与实用程序

Skill 创建工具（skill-creator skill）：
- 内建在 Claude.ai 中，Claude Code 也可以使用
- 可以根据描述生成 Skill
- 评审并提供改进建议
- 使用方式：「帮我用 skill-creator 构建一个 Skill」

验证：
- skill-creator 可以评估你的 Skill
- 提问方式：「评审这个 Skill 并给出改进建议」

---

## 获取支持

技术问题：
- 一般问题：Claude Developers Discord 社区论坛
问题报告：
- GitHub Issues：`anthropics/skills/issues`
- 需要包含：Skill 名称、错误信息、复现步骤

---

# 附录 A：快速检查清单

在上传前后，你可以用这个清单验证你的 Skill。如果你想更快上手，可以用 skill-creator 生成初稿，然后对照这个清单检查，确保没有遗漏。

## 开始之前

- 确定了 2 ～ 3 个具体用例
- 确定了所需工具（内置或 MCP）
- 阅读过本指南和示例 Skill
- 规划好了文件夹结构

## 开发过程中

- 文件夹使用烤肉串命名法
- 存在 SKILL.md 文件（拼写完全正确）
- YAML 前置元数据有`---`分隔符
- name 字段：烤肉串命名，无空格，无大写
- description 同时包含「做什么」和「什么时候用」
- 任何位置都没有 XML 标签（`< >`）
- 指令清晰可执行
- 包含错误处理
- 提供了示例
- 引用清晰链接化

## 上传之前

- 测试过明显任务可以触发
- 测试过换一种说法也能触发
- 验证过不相关主题不会触发
- 功能测试通过
- 工具集成正常（如果适用）
- 压缩为 zip 文件

## 上传之后

- 在真实对话中测试
- 监控触发不足/过度问题
- 收集用户反馈
- 迭代改进描述和指令
- 更新 metadata 中的版本号

---

# 附录 B：YAML 前置元数据

## 必填字段

```yaml
name: skill-name-in-kebab-case
description: What it does and when to use it. Include specific trigger phrases.
```

## 所有可选字段

```yaml
name: skill-name
description: [required description]
license: MIT # 可选：开源协议

allowed-tools: "Bash(python:*) Bash(npm:*) WebFetch" # 可选：限制工具访问

metadata: # 可选：自定义字段

  author: Company Name
  version: 1.0.0
  mcp-server: server-name
  category: productivity
  tags: [project-management, automation]
  documentation: https://example.com/docs
  support: support@example.com
```

## 安全说明

允许：
- 任何标准 YAML 类型（字符串、数字、布尔、列表、对象）
- 自定义元数据字段
- 长描述（最多1024 字符）

禁止：
- XML 尖括号（`< >`）——安全限制
- YAML 中执行代码（使用安全 YAML 解析）
- 用`claude`或`anthropic`前缀命名 Skill（保留名称）

---

# 附录 C：完整 Skill 示例

可以查看展示本指南中所有模式的生产级完整 Skill：
- 文档 Skill——PDF、DOCX、PPTX、XLSX 创建
- 示例 Skill——各种工作流模式
- 合作伙伴 Skill 目录——查看来自 Asana、Atlassian、Canva、Figma、Sentry、Zapier 等合作伙伴的 Skill

这些仓库保持更新，除了本指南覆盖的内容，还包含更多示例。你可以克隆、按需修改，把它们用作模板。
