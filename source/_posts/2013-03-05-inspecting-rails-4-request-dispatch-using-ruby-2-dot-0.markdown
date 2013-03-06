---
layout: post
title: "Inspecting Rails 4 using Ruby 2.0"
date: 2013-03-05 22:18
comments: true
categories: 
- ruby
- rails
---

Ruby 2.0 has a cool new feature that many people talk about:
[TracePoint](http://ruby-doc.org/core-2.0/TracePoint.html).

`TracePoint` essentially allows you to hook into Ruby's events and
listen for events.

Being curious and since I just started a brand new Rails 4/Ruby 2 app, I
decided to write a little middleware and see what Rails is up to when
handling incoming requests.

Here is my [TracePoint Rack Middleware](https://gist.github.com/mattetti/5097206).

```ruby
class TracePoint
  class Middleware
 
    def initialize(app)
      @app = app
    end
 
    def call(env)
      stats = {}
      trace = TracePoint.new(:call) do |tp|
        stats[tp.defined_class] ||= {}
        stats[tp.defined_class][tp.method_id] ||= 0
        stats[tp.defined_class][tp.method_id] += 1
      end
      trace.enable
      response = @app.call(env)
      trace.disable
 
      puts "#{stats.keys.size} classes used"
      puts "#{stats.map{|k,v| v.keys}.flatten.size} methods used"
      puts "#{stats.map{|k,v| v.values}.flatten.sum} methods dispatched"
      response
    end
 
  end
end
```
(the gist shows a modified version so I could dump to disk the json
representation of the calls)

I then inserted the middleware in Rails:

```ruby
# in application.rb
config.middleware.insert_before(ActionDispatch::Static, TracePoint::Middleware)
```

I saved the output in json format for the curious: [click here](https://gist.github.com/mattetti/5097178)

On average, in production mode, using Ruby 2.0 and Puma on my laptop, my hello world index page takes 5ms.

To render my page, Rails uses (more or less):

* 250 classes
* 750 methods (not including C functions)
* and dispatches 2704 methods (not including calls to C functions)

Here is a small selection of some of the methods dispatched:

```json
"String": {
    "underscore": 1,
    "blank?": 14,
    "html_safe": 78
  },
"ActiveSupport::Inflector": {
  "underscore": 2,
  "inflections": 2
},
"Hash": {
  "with_indifferent_access": 2,
  "except": 1,
  "except!": 1,
  "stringify_keys": 5,
  "transform_keys": 13,
  "stringify_keys!": 1,
  "transform_keys!": 3,
  "extractable_options?": 4,
  "extract!": 2,
  "symbolize_keys": 8,
  "reverse_merge": 1,
  "slice": 2,
  "symbolize_keys!": 2
},
"ActionView::CompiledTemplates": {
  "_app_views_welcome_index_html_erb__4177595130715791755_70209827438920": 1,
  "_app_views_layouts_application_html_erb___652124533295419796_70209827456500": 1
},
"ActiveSupport::Notifications::Fanout": {
  "start": 4,
  "listeners_for": 12,
  "listening?": 5,
  "finish": 3
}
```


`TracePoint` is a great new addition and I hope to see some new crazy
tools being developed (production dead-code analyzer, deprecation code path
finder anyone?)

To end, this short post, here is an interesting quote from [Chad
Fowler](http://chadfowler.com/) Berliner by adoption:

> Abstractions are expensive. The cost increases exponentially as you add them to a codebase.
> [Chad Fowler](https://twitter.com/chadfowler/status/308959527217270786)
