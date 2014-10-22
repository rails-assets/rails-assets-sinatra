# Using Rails Assets with Sinatra

This is a minimal demo app showing how to use [Rails Assets](https://rails-assets.org) service with Sinatra application.

## Heroku deployment

`sinatra-asset-pipeline`
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

## Contributing

If you think you can improve this template in any way, please send a PR.

## License

MIT
