source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
gem "jekyll-include-cache"
gem "jemoji"

group :jekyll_plugins do
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jemoji"
  gem "jemoji"
end

# Windows 和 JRuby 不支持这些平台
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "wdm", "~> 0.1.1"
end

# 性能优化
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby] 