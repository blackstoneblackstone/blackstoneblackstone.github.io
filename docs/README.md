# 文档目录 📚

本目录包含网站优化和配置相关的所有技术文档。

## 📋 文档列表

### 🖼️ 图片优化
- **[IMAGE-OPTIMIZATION-GUIDE.md](./IMAGE-OPTIMIZATION-GUIDE.md)** - 图片优化完整指南
  - Cloudflare 图片优化格式
  - 参数配置说明
  - 性能优化建议
  - 批量替换工具

### 🤖 AI 爬虫配置
- **[AI-CRAWLER-POLICY.md](./AI-CRAWLER-POLICY.md)** - AI 爬虫友好政策
  - 支持的 AI 爬虫列表
  - 内容许可说明
  - 技术细节和使用建议

### 🌐 域名管理
- **[DOMAIN-MIGRATION.md](./DOMAIN-MIGRATION.md)** - 域名迁移记录
  - 从 `blackstoneblackstone.github.io` 到 `blackstones.xyz`
  - 配置文件更新记录
  - 验证清单和测试命令

## 🎯 快速导航

### 新文章写作
1. **添加图片** → 查看 [图片优化指南](./IMAGE-OPTIMIZATION-GUIDE.md)
2. **配置元数据** → 参考现有文章格式
3. **分类设置** → 使用预定义分类

### 网站优化
1. **图片性能** → [图片优化指南](./IMAGE-OPTIMIZATION-GUIDE.md)
2. **SEO 配置** → 检查 `_config.yml` 和 `robots.txt`
3. **AI 友好性** → [AI 爬虫政策](./AI-CRAWLER-POLICY.md)

### 域名和部署
1. **域名配置** → [域名迁移记录](./DOMAIN-MIGRATION.md)
2. **GitHub Pages** → 检查仓库设置
3. **Cloudflare** → 验证 CDN 配置

## 📊 网站配置概览

### 基础信息
- **域名**: `blackstones.xyz`
- **GitHub 仓库**: `blackstoneblackstone/blackstoneblackstone.github.io`
- **部署平台**: GitHub Pages + Cloudflare CDN
- **图片服务**: Cloudflare Image Resizing

### 技术栈
- **静态网站**: Jekyll
- **主题**: 自定义 Bootstrap 主题
- **评论系统**: Giscus
- **搜索**: Lunr.js
- **图片优化**: Cloudflare CDN

### AI 友好配置
- ✅ robots.txt 允许所有 AI 爬虫
- ✅ Meta 标签明确允许 AI 训练
- ✅ 内容许可: CC-BY-SA-4.0
- ✅ 结构化数据和站点地图

## 🔧 常用命令

### 本地开发
```bash
# 启动本地服务器
bundle exec jekyll serve

# 构建静态文件
bundle exec jekyll build

# 检查语法
bundle exec jekyll doctor
```

### 图片优化
```bash
# 批量替换图片 URL（使用编辑器）
查找: https://pic\.iu101\.org/([^/\s?]+\.(?:png|jpg|jpeg))
替换: https://pic.iu101.org/cdn-cgi/image/width=800,quality=85,format=webp/$1
```

### 部署
```bash
# 提交更改
git add .
git commit -m "更新说明"
git push origin main
```

## 📈 性能监控

### 关键指标
- **页面加载速度**: < 3秒
- **图片优化率**: 80-95% 压缩
- **SEO 评分**: 90+ 分
- **AI 爬虫可访问性**: 100%

### 测试工具
- [PageSpeed Insights](https://pagespeed.web.dev/)
- [GTmetrix](https://gtmetrix.com/)
- [WebPageTest](https://www.webpagetest.org/)

## 🆘 故障排除

### 常见问题

#### 图片不显示
1. 检查 URL 格式是否正确
2. 确认图片文件存在于 R2 存储
3. 验证 Cloudflare 图片优化服务状态

#### 域名访问问题
1. 检查 DNS 配置
2. 验证 GitHub Pages 自定义域名设置
3. 确认 SSL 证书状态

#### AI 爬虫无法访问
1. 检查 robots.txt 配置
2. 验证 Meta 标签设置
3. 确认服务器响应正常

### 联系支持
- **GitHub Issues**: [创建 Issue](https://github.com/blackstoneblackstone/blackstoneblackstone.github.io/issues)
- **技术文档**: 参考本目录下的相关文档

---

**最后更新**: 2025-10-12  
**维护者**: 黑石  
**版本**: 1.0
