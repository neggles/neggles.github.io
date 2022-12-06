source "https://rubygems.org"

# dotenv
gem 'dotenv-rails'

gem "jekyll", "~> 3.9.2"

# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", github: "jekyll/minima"

# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
gem "github-pages", "~> 227", group: :jekyll_plugins

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.15.1"
  gem "jekyll-github-metadata", "~> 2.13.0"
  gem "jekyll-remote-theme", "~> 0.4.3"
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.0", :install_if => Gem.win_platform?

# kramdown v2 ships without the gfm parser by default
gem "kramdown-parser-gfm", "~> 1.1.0"

# need dis for ruby 3
gem "webrick", "~> 1.7"

gem "rouge", "~> 3.26.0"
