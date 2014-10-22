# Using Rails Assets with Sinatra

This is a minimal demo app showing how to use [Rails Assets](https://rails-assets.org) service with Sinatra application.

## Integration

This application is using [Sinatra Asset Pipeline](https://github.com/kalasjocke/sinatra-asset-pipeline) that integrates Sinatra with Sprockets.

Integrating [Rails Assets](https://rails-assets.org/) is as simple as appening its load paths.

#### Gemfile

```ruby
source 'https://rubygems.org'
source 'https://rails-assets.org'

gem 'sinatra', require: 'sinatra/base'
gem 'sinatra-asset-pipeline', require: 'sinatra/asset_pipeline'

gem 'uglifier'
gem 'slim'

gem 'rails-assets-jquery'
gem 'rails-assets-bootstrap'
```

#### application.rb

```ruby
class Application < Sinatra::Base
  configure do
    set :assets_precompile, %w(application.js application.css *.png *.jpg *.svg *.eot *.ttf *.woff)
    set :assets_css_compressor, :sass
    set :assets_js_compressor, :uglifier
    register Sinatra::AssetPipeline

    # Actual Rails Assets integration, everything else is Sprockets
    if defined?(RailsAssets)
      RailsAssets.load_paths.each do |path|
        settings.sprockets.append_path(path)
      end
    end
  end

  get '/' do
    slim :index
  end
end
```

#### assets/js/application.js.coffee

```coffee
#= require jquery
#= require bootstrap
```

#### assets/css/application.css.scss

```scss
@import "bootstrap";
```

#### views/layout.slim

```slim
doctype html
html lang="en"
  head
    meta charset="utf-8"
    title Sinatra with Rails Assets
    meta name="viewport" content="width=device-width, initial-scale=1"
    == stylesheet_tag 'application'
  body
    .container
      == yield
    == javascript_tag 'application'
```

## Running application

You can use any ruby server, including bare rack:

```
bundle exec rackup --port 3000
```

## Heroku deployment

[Sinatra Asset Pipeline](https://github.com/kalasjocke/sinatra-asset-pipeline) defines template for `assets:precompile` task. It is enabled in Rakefile:

```ruby
require_relative 'application'

require 'sinatra/asset_pipeline/task'
Sinatra::AssetPipeline::Task.define!(Application)
```

Heroku automatically runs `assets:precompile` upon deploy, so you can just:

```
heroku create rails-assets-sinatra
git push heroku master
```

Deployed demo is available on: http://rails-assets-sinatra.herokuapp.com/

## Contributing

If you think you can improve this template in any way, please send a PR.

## License

MIT
