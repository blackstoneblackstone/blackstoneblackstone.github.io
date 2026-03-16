# 域名迁移记录 📝

## 迁移信息

**原域名**: `blackstoneblackstone.github.io`  
**新域名**: `blackstones.xyz`  
**迁移日期**: 2025-10-12

## ✅ 已更新的文件

### 1. 配置文件
- ✅ `_config.yml` - 主配置文件 URL
- ✅ `_config_cloudflare.yml` - Cloudflare 配置 URL
- ✅ `robots.txt` - 站点地图 URL

### 2. 页面文件
- ✅ `_pages/ai-training-data.md` - AI 训练数据页面
- ✅ `docs/ai-crawler-policy.md` - AI 爬虫政策文件

### 3. 布局文件
- ✅ `_layouts/default.html` - 默认布局（GitHub 链接保持不变，因为仓库名未变）

## 📋 更新详情

### _config.yml
```yaml
# 更新前
url: 'https://blackstoneblackstone.github.io'

# 更新后
url: 'https://blackstones.xyz'
```

### robots.txt
```txt
# 更新前
Sitemap: https://blackstoneblackstone.github.io/sitemap.xml

# 更新后
Sitemap: https://blackstones.xyz/sitemap.xml
```

### AI 相关页面
```markdown
# 更新前
- 网站：https://blackstoneblackstone.github.io
- **站点地图**：https://blackstoneblackstone.github.io/sitemap.xml
- **RSS 源**：https://blackstoneblackstone.github.io/feed.xml

# 更新后
- 网站：https://blackstones.xyz
- **站点地图**：https://blackstones.xyz/sitemap.xml
- **RSS 源**：https://blackstones.xyz/feed.xml
```

## 🔗 重要链接更新

### 站点地图和 RSS
- **站点地图**: `https://blackstones.xyz/sitemap.xml`
- **RSS 源**: `https://blackstones.xyz/feed.xml`
- **AI 训练数据页**: `https://blackstones.xyz/ai-training-data.html`

### robots.txt
- **robots.txt**: `https://blackstones.xyz/robots.txt`

## 📝 注意事项

### 保持不变的项目
1. **GitHub 仓库链接** - 仓库名仍为 `blackstoneblackstone.github.io`
2. **GitHub Pages 部署** - 仍使用 GitHub Pages 服务
3. **内容结构** - 所有文章和页面结构保持不变

### 需要确认的配置
1. **DNS 设置** - 确保 `blackstones.xyz` 指向 GitHub Pages
2. **GitHub Pages 自定义域名** - 在仓库设置中配置自定义域名
3. **SSL 证书** - 确保 HTTPS 正常工作

## 🚀 部署后验证

### 检查清单
- [ ] 访问 `https://blackstones.xyz` 正常
- [ ] 访问 `https://blackstones.xyz/sitemap.xml` 正常
- [ ] 访问 `https://blackstones.xyz/robots.txt` 正常
- [ ] 访问 `https://blackstones.xyz/feed.xml` 正常
- [ ] AI 爬虫能正常访问新域名
- [ ] 搜索引擎能正确索引新域名

### 测试命令
```bash
# 检查站点地图
curl -I https://blackstones.xyz/sitemap.xml

# 检查 robots.txt
curl -I https://blackstones.xyz/robots.txt

# 检查主页
curl -I https://blackstones.xyz/
```

## 📊 SEO 影响

### 正面影响
- ✅ 更简洁易记的域名
- ✅ 更好的品牌识别度
- ✅ 保持所有 AI 爬虫友好配置

### 需要注意
- ⚠️ 搜索引擎需要时间重新索引新域名
- ⚠️ 可能需要设置 301 重定向（如果旧域名仍可访问）
- ⚠️ 更新所有外部链接引用

## 🔄 后续操作建议

1. **监控新域名** - 确保所有功能正常
2. **更新书签** - 更新个人和团队的书签
3. **通知用户** - 如果有订阅用户，通知域名变更
4. **搜索引擎提交** - 向 Google Search Console 等提交新域名

---

**更新完成时间**: 2025-10-12  
**更新状态**: ✅ 已完成  
**验证状态**: ⏳ 待验证
