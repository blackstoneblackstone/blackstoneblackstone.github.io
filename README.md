# 黑石的知识库

黑石的知识库是一个分享知识和经验的博客网站，主要关注AI开发工具、独立开发等相关内容。

## 关于本站

这是一个基于 Jekyll 的静态网站，使用了 Mundana 主题。网站主要用于分享各种技术知识、工具介绍和实践经验，特别是AI编程工具和独立开发相关内容。

### 主要内容分类

- **AI Coding Tools**: 介绍各种AI驱动的编程工具，如Claude Code、Cursor、Windsurf等
- **独立开发工具**: 为独立开发者推荐的工具和资源
- **优秀项目**: 分享值得关注的开源项目和产品

### 特色页面

- [AI开发工具页面](https://yondu.vip/ai-coding-tools.html) - 按类别整理的AI开发工具大全
- [独立开发页面](https://yondu.vip/indie-dev.html) - 专为独立开发者准备的工具和项目资源

## 本地开发

```bash
# 安装依赖
bundle install

# 启动本地服务器
bundle exec jekyll serve

# 访问 http://localhost:4000 查看网站
```

## 内容贡献

欢迎贡献内容！如果您想分享相关知识或工具，请按照以下步骤操作：

1. Fork 此仓库
2. 在 `_posts` 目录下创建新的文章文件，文件名格式为 `YYYY-MM-DD-title.md`
3. 按照现有文章的格式编写内容
4. 提交 Pull Request

### 文章格式要求

所有文章都需要包含以下 front matter:

```yaml
---
layout: post
title: "文章标题"
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [分类1, 分类2]
tags: [标签1, 标签2]
image: /assets/images/文章相关图片.png
author: 黑石
excerpt: 文章摘要，用于在列表页显示
---
```

## 部署

本站使用 GitHub Pages 自动部署，推送到 main 分支后会自动更新网站。

## 版权信息

主题基于 [Mundana by WowThemes.net](https://wowthemes.net) 修改，原主题遵循 MIT 许可证。

内容由黑石创作，保留所有权利。
