[![Gem Version](https://img.shields.io/gem/v/capistrano-logtail.svg)](https://rubygems.org/gems/capistrano-logtail)
[![Dependencies](https://img.shields.io/gemnasium/ydkn/capistrano-logtail.svg)](https://gemnasium.com/ydkn/capistrano-logtail)
[![Code Climate](https://img.shields.io/codeclimate/github/ydkn/capistrano-logtail.svg)](https://codeclimate.com/github/ydkn/capistrano-logtail)

# capistrano-logtail

Log tailing for capistrano

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'capistrano-logtail'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install capistrano-logtail

## Configuration

```ruby
set :logtail_files, %w( /var/log/syslog )
set :logtail_lines, 50
```

## Usage

Require in `Capfile`:

```ruby
require 'capistrano/logtail'
```

This will add the following tasks:
* logs:tail[]
* logs:tail:all
* logs:tail:rails

Add tasks for other log files as required:

```ruby
# config/deploy.rb

namespace :logs do
  namespace :tail do
    desc 'Tail sidekiq log'
    task :sidekiq do
      on roles(:app) do
        logtail_utility.tail(release_path.join('log', 'sidekiq.log'))
      end
    end
  end
end

# cap production logs:tail:sidekiq
```

### Examples:

    $ cap production logs:tail:all
    $ cap production logs:tail:rails
    $ cap production logs:tail[production] # tails shared/log/production.log
    $ cap production logs:tail[/var/log/nginx/access.log]
    $ cap production logs:tail # uses value of logtail_files
    $ cap production logs:tail n=100 # specify number of existing lines to show

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
