# GitHub Actions 部署修复指南 🔧

## 问题描述

GitHub Actions 部署时出现 Ruby 版本不匹配错误：

```
Your Ruby version is 3.4.4, but your Gemfile specified 3.4.7
Error: The process '/opt/hostedtoolcache/Ruby/3.4.4/x64/bin/bundle' failed with exit code 18
```

## ✅ 已修复的问题

### 1. Ruby 版本统一
已将所有配置文件中的 Ruby 版本统一为 3.4.4：

- ✅ `Gemfile`: `ruby "3.4.4"`
- ✅ `.ruby-version`: `3.4.4`
- ✅ `_config_cloudflare.yml`: `ruby_version: "3.4.4"`
- ✅ `.github/workflows/jekyll.yml`: `ruby-version: 3.4.4`

### 2. 版本兼容性
- **GitHub Actions**: 使用 Ruby 3.4.4
- **Cloudflare Pages**: 使用 Ruby 3.4.4
- **本地开发**: 建议使用 Ruby 3.4.4

## 🚀 解决方案

### 方案 1：本地安装 Ruby 3.4.4（推荐）

#### 使用 rbenv
```bash
# 安装 Ruby 3.4.4
rbenv install 3.4.4

# 设置为项目版本
rbenv local 3.4.4

# 更新依赖
bundle install
```

#### 使用 RVM
```bash
# 安装 Ruby 3.4.4
rvm install 3.4.4

# 设置为项目版本
rvm use 3.4.4

# 更新依赖
bundle install
```

### 方案 2：使用 Docker（无需本地安装）

#### 创建 Dockerfile
```dockerfile
FROM ruby:3.4.4-slim

WORKDIR /app
COPY Gemfile Gemfile.lock ./
RUN bundle install

COPY . .
EXPOSE 4000

CMD ["bundle", "exec", "jekyll", "serve", "--host", "0.0.0.0"]
```

#### 使用 Docker 运行
```bash
# 构建镜像
docker build -t jekyll-site .

# 运行容器
docker run -p 4000:4000 jekyll-site
```

### 方案 3：直接推送让 GitHub Actions 处理

如果本地环境配置复杂，可以直接推送代码让 GitHub Actions 处理：

```bash
# 提交更改
git add .
git commit -m "修复 Ruby 版本不匹配问题"
git push origin main
```

## 📋 验证步骤

### 1. 检查版本一致性
```bash
# 检查 Ruby 版本
ruby --version

# 检查 Gemfile
grep "ruby " Gemfile

# 检查 .ruby-version
cat .ruby-version
```

### 2. 本地测试构建
```bash
# 安装依赖
bundle install

# 测试构建
bundle exec jekyll build

# 本地预览
bundle exec jekyll serve
```

### 3. 检查 GitHub Actions
1. 推送代码到 GitHub
2. 查看 Actions 标签页
3. 确认构建成功

## 🔍 故障排除

### 如果仍然出现版本错误

#### 1. 清理缓存
```bash
# 删除 Gemfile.lock
rm Gemfile.lock

# 清理 bundle 缓存
bundle clean --force

# 重新安装
bundle install
```

#### 2. 检查 GitHub Actions 缓存
- 在 GitHub 仓库的 Actions 页面
- 查看是否有缓存问题
- 可以手动清除缓存

#### 3. 更新 GitHub Actions 工作流
确保 `.github/workflows/jekyll.yml` 中的版本正确：

```yaml
- name: Setup Ruby
  uses: ruby/setup-ruby@v1
  with:
    ruby-version: 3.4.4  # 确保这里是 3.4.4
    bundler-cache: true
```

## 📊 版本兼容性表

| 环境 | Ruby 版本 | 状态 |
|------|-----------|------|
| GitHub Actions | 3.4.4 | ✅ 已配置 |
| Cloudflare Pages | 3.4.4 | ✅ 已配置 |
| 本地开发 | 3.4.4 | ⚠️ 需要安装 |
| Jekyll | >= 4.4.0 | ✅ 兼容 |

## 🎯 最佳实践

### 1. 版本锁定
- 始终在 `Gemfile` 中指定确切的 Ruby 版本
- 使用 `.ruby-version` 文件锁定版本
- 定期更新依赖以保持安全性

### 2. 环境一致性
- 开发、测试、生产环境使用相同的 Ruby 版本
- 使用 Docker 或版本管理器确保环境一致
- 在 CI/CD 中明确指定版本

### 3. 依赖管理
- 定期运行 `bundle update` 更新依赖
- 检查 `Gemfile.lock` 确保版本一致
- 使用 `bundle audit` 检查安全漏洞

## 📞 获取帮助

如果问题仍然存在：

1. **检查 GitHub Actions 日志**
   - 查看详细的错误信息
   - 确认版本配置是否正确

2. **创建 Issue**
   - 在 GitHub 仓库创建 Issue
   - 提供完整的错误日志

3. **参考文档**
   - [Jekyll 官方文档](https://jekyllrb.com/docs/)
   - [GitHub Actions 文档](https://docs.github.com/en/actions)

---

**修复状态**: ✅ 已完成版本统一  
**下一步**: 推送代码验证 GitHub Actions 构建  
**预计结果**: 部署成功，网站正常访问
