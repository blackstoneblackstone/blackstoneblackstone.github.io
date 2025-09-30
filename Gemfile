source "https://rubygems.org"

# Specify Ruby version for Cloudflare Pages compatibility
ruby "3.4.4"

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#

# Use Jekyll version compatible with Ruby 3.4+
gem "jekyll", "~> 4.3.0"

# If you have any plugins, put them here!
gem 'wdm', '>= 0.1.0' if Gem.win_platform?

group :jekyll_plugins do
    gem 'jekyll-feed', '~> 0.15'
    gem 'jekyll-sitemap', '~> 1.4'
    gem 'jekyll-paginate', '~> 1.1'
    gem 'jekyll-seo-tag', '~> 2.8'
end

# Markdown parser
gem "kramdown-parser-gfm", "~> 1.1"

# Bundler version compatible with Ruby 3.4+
gem "bundler", "~> 2.5"
