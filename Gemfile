source "https://mirrors.tuna.tsinghua.edu.cn/rubygems/"

# 明确指定与Ruby 2.6.10兼容的依赖版本
gem "jekyll", "~> 3.9.0"
gem "minima", "~> 2.5.1"
gem "kramdown-parser-gfm", "~> 1.1.0"

# 常用插件
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.15.1"
  gem "jekyll-seo-tag", "~> 2.7.1"
  gem "jekyll-paginate", "~> 1.1.0"
  gem "jekyll-sitemap", "~> 1.4.0"
  gem "jemoji", "~> 0.12.0"
end

# Windows 和 JRuby 不支持这些平台
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "wdm", "~> 0.1.1"
end

# 性能优化
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby] 