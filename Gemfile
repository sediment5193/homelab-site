source "https://rubygems.org"

# Deploying via GitHub Actions (see .github/workflows/jekyll.yml)
# rather than GitHub's legacy Pages build pipeline, so we're not locked
# to the old Jekyll/Ruby versions the "github-pages" gem pins to. This
# uses current Jekyll, which supports current Ruby without the stdlib
# shims (csv/base64/logger/bigdecimal) older Jekyll needed.
gem "liquid", ">= 4.0.4"
gem "jekyll", "~> 4.3"

# Theme, installed as a regular gem instead of remote_theme — no network
# fetch needed at build time, and no dependency on GitHub's whitelist.
gem "minimal-mistakes-jekyll"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
  gem "jekyll-sitemap"
  gem "jekyll-paginate"
  gem "jekyll-include-cache"
end

# Windows and JRuby do not include zoneinfo files, so bundle the tzinfo-data gem
platforms :windows, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# webrick is no longer bundled with Ruby 3+, needed for local serving
gem "webrick", "~> 1.8"
