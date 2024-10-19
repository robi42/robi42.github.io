+++
title = "Play 2 Boilerplate"
date = 2012-09-21
path = "blog/2012/09/21/play2-boilerplate"
[taxonomies]
#tags = ["Code", "Web"]
+++

A little project aimed at getting one up and running quickly with building a
contemporary web app using [Play 2](http://www.playframework.org/) framework.

Contains a bunch of current web dev **best practices** and niceties, from latest
Twitter Bootstrap (LESS) & Backbone.js to CoffeeScript with CommonJS modules
support & proper JS bundling.

Here're a couple of exemplary code snippets to whet one's appetite:
## main.coffee
```ruby
{log} = require './utilities'

$(document).ready ->
  window.app = {}
  log 'Hello, world!'
  return
```
## Application.scala
```scala
object Application extends Controller {

  def index = Action {
    Ok(views.html.index(Messages("app.name")))
  }

}
```
[Get it on GitHub](https://github.com/robi42/play2-boilerplate) while it's hot.
