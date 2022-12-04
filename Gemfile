source "https://rubygems.org"

gem "jekyll", "~> 4.2.0"
gem "lbenicio-minimal-v1", "1.0.2", git: "https://github.com/lbenicio/lbenicio-minimal-v1", branch: "main"
#gem "lbenicio-minimal-v1", "1.0.1", path: "/Users/lbenicio/developer/lbenicio/theme.v1"
gem "rouge"
gem "rake"

group :jekyll_plugins do

  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-seo-tag"
  gem "jekyll-sitemap"
  gem "jekyll-archives"
  gem "jekyll-tagging"
  gem "jekyll-tagging-related_posts"
  gem "jekyll-paginate-v2", "3.0.0", git: "https://github.com/lbenicio/jekyll-paginate-v2", branch: "master"
  gem "jekyll-minifier", "0.1.8", git: "https://github.com/lbenicio/jekyll-minifier", branch: "master"
end

group :development, :test do
  gem "rubocop", "~> 1.38.0"
  gem "rubocop-minitest"
  gem "rubocop-performance"
  gem "rubocop-rake"
  gem "rubocop-rspec"
  gem "jekyll_test_plugin"
  gem "jekyll_test_plugin_malicious"
  gem "benchmark-ips"
  gem "rbtrace"
  gem "ruby-prof"
  gem "stackprof"
  gem "memory_profiler"
  gem "httpclient"
  gem "rspec"
  gem "selenium-webdriver"
  gem "chromedriver-helper"
  gem "capybara"
  gem "rack-jekyll"
  gem "pry"
  gem "html-proofer", "~> 3.19.4"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

