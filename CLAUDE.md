# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个基于 Jekyll 的静态网站，使用了 Mundana 主题，主要用于分享知识和经验。网站内容以博客文章形式组织，支持分类和标签功能。

网站专注于AI编程、全栈开发、技术分享，特别关注AI开发工具和独立开发相关内容。

## 常用命令

### 本地开发
```bash
# 安装依赖
bundle install

# 启动本地服务器
bundle exec jekyll serve

# 构建站点
bundle exec jekyll build
```

### 部署
网站通过 GitHub Pages 自动部署，只需推送到 main 分支即可。

## 代码架构

### 目录结构
- `_posts/` - 博客文章，按日期命名
- `_pages/` - 静态页面
- `_layouts/` - 页面布局模板
- `_includes/` - 可复用的页面组件
- `assets/` - 静态资源（CSS、JS、图片）
- `_config.yml` - Jekyll 配置文件

### 主要组件
1. **首页** (`index.html`) - 显示最新文章列表，包含个人介绍区域和"Buy Me A Coffee"按钮
2. **AI 开发工具页面** (`_pages/ai-coding-tools.html`) - 专门展示 AI 开发工具的分类页面，按子分类组织工具
3. **独立开发页面** (`_pages/indie-dev.html`) - 专为独立开发者准备的工具和项目资源
4. **营销工具页面** (`_pages/marketing-tools.html`) - 营销相关工具页面
5. **课程页面** (`_pages/course.html`) - 课程相关内容页面
6. **文章系统** - 使用 Markdown 编写，支持分类和标签，具有图片查看器功能
7. **导航菜单** - 在 `_includes/menu-header.html` 中定义

### 主题和样式
- 使用 Bootstrap 4 框架
- 自定义样式在 `assets/css/custom.css` 中
- 响应式设计，支持移动端浏览
- 使用 Prism.js 进行代码高亮
- 支持图片查看器功能（点击文章中的图片可放大查看）

## 开发注意事项

### 文章格式
所有文章都位于 `_posts/` 目录下，文件名格式为 `YYYY-MM-DD-title.md`。每篇文章需要包含以下 front matter：

```yaml
---
layout: post
title: "文章标题"
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [分类1, 分类2]
tags: [标签1, 标签2]
image: /assets/images/文章相关图片.png
author: 作者名
excerpt: 文章摘要，用于在列表页显示
---
```

### 页面创建
静态页面位于 `_pages/` 目录下，需要包含以下 front matter：

```yaml
---
layout: page
title: "页面标题"
permalink: "/页面路径/"
---
```

### 导航菜单
导航菜单在 `_includes/menu-header.html` 文件中定义，如需添加新菜单项，请在此文件中添加相应链接。

### 样式定制
自定义样式应添加到 `assets/css/custom.css` 文件中，避免修改主题核心样式文件。

### 分类页面结构
AI开发工具等分类页面使用特定的结构来组织内容：
- 使用文章的第二个分类作为子分类进行分组展示
- 具有侧边栏导航，可快速跳转到不同分类
- 支持平滑滚动和高亮效果

### SEO和结构化数据
- 网站包含完整的SEO配置，包括Open Graph标签和Twitter Card标签
- 支持结构化数据（JSON-LD）以提高搜索引擎可见性
- 支持Google Analytics和Google Search Console集成

## Cursor 规则
- 新增工具和优秀项目时需要提供工具名、logo和网址
- 工具和优秀项目的标题最多6个字

## 技术栈
- Jekyll 3.9.x (使用较旧版本以确保Apple Silicon兼容性)
- Ruby
- Bootstrap 4
- Prism.js (代码高亮)
- Lunr.js (搜索功能)
- jQuery

## 重要的配置文件
- `_config.yml` - 网站的主要配置文件，包含SEO设置、社交链接、Giscus评论配置等
- `Gemfile` - Ruby依赖管理文件