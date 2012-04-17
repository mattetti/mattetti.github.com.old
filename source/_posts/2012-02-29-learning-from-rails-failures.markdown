---
date: '2012-02-29 07:48:08'
layout: post
legacy_url: http://merbist.com/2012/02/29/learning-from-rails-failures/
slug: learning-from-rails-failures
source: merbist.com
status: publish
title: Learning from Rails' failures
wordpress_id: '1251'
categories:
- Misc
- rails
- Software Design
- merbist.com
- blog-post
tags:
- django
- documentation
- merb
- migration
- open source
- rails
- ruby
---

Ruby on Rails undisputedly changed the way web frameworks are designed. Rails became a reference when it comes to leveraging conventions, easy baked in feature set and a rich ecosystem. However, I think that Rails did and still does a lot of things pretty poorly.  By writing this post, I'm not trying to denigrate Rails, there are many other people out there already doing that. My hope is that by listing what I think didn't and still doesn't go well, we can learn from our mistakes and improve existing solutions or create better new ones.

[![](http://merbist.com/wp-content/uploads/2012/02/train_fail-300x188.jpg)](http://merbist.com/2012/02/29/learning-from-rails-failures/train_fail/)


## Migration/upgrades


Migrating a Rails App from a version to the other is very much like playing the lottery, you are almost sure you will lose. To be more correct, you know things will break, you just don't know what, when and how. The Rails team seems to think that everybody is always running on the cutting edge version and don't consider people who prefer to stay a few version behind for stability reasons. What's worse is that plugins/gems might or might not compatible with the version you are updating to, but you will only know that by trying yourself and letting others try and report potential issues.

This is for me, by far, the biggest issue with Rails and something that should have been fixed a long time ago. If you're using the WordPress blog engine, you know how easy and safe it is to upgrade the engine or the plugins. Granted WordPress isn't a web dev framework, but it gives you an idea of what kind of experience we should be striving for.

 


## Stability vs playground zone


New features are cool and they help make the platform more appealing to new comers. They also help shape the future of a framework. But from my perspective, that shouldn't come to the cost of stability. Rails 3's new asset pipeline is a good example of a half-baked solution shoved in a release at the last minute and creating a nightmare for a lot of us trying to upgrade. I know, I know, you can turn off the asset pipeline and it got better since it was first released. But shouldn't that be the other way around? Shouldn't fun new ideas risking the stability of an app or making migration harder, be off by default and turned on only by people wanting to experiment? When your framework is young, it's normal that you move fast and sometimes break, but once it matures, these things shouldn't happen.

 


## Public/private/plugin APIs


This is more of a recommendation than anything else. When you write a framework in a very dynamic language like Ruby, people will "monkey patch" your code to inject features. Sometimes it is due to software design challenges, sometimes it's because people don't know better. However,  by not explicitly specifying what APIs are private (they can change at anytime, don't touch), what APIs are public (stable, will be slowly deprecated when they need to be changed) and which ones are for plugin devs only (APIs meant for instrumentation, extension etc..), you are making migration to newer versions much harder. You see, if you have a small, clean public API, then it's easy to see what could break, warn developers and avoid migration nightmares. However, you need to start doing that early on in your project, otherwise you will end up like Rails where all code can potentially change anytime.

 


## Rails/Merb merge was a mistake


This is my personal opinion and well, feel free to disagree, nobody will ever be able to know to for sure. Without explaining what happened behind closed doors and the various personal motivations, looking at the end result, I agree with the group of people thinking that the merge didn't turn up to be a good thing. For me, Rails 3 isn't significantly better than Rails 2 and it took forever to be released. You still can't really run a mini Rails stack like promised. I did hear that Strobe (company who was hiring Carl Lerche, Yehuda Katz and contracted Jose Valim) used to have an ActionPack based, mini stack but it was never released and apparently only Rails core members really knew what was going on there. Performance in vanilla Rails 3 are only now getting close to what you had with Rails 2 (and therefore far from the perf you were getting with Merb). Thread-safety is still OFF by default meaning that by default your app uses a giant lock only allowing a process to handle 1 request at a time. For me, the flexibility and performance focus of Merb were mainly lost in the merge with Rails. (Granted, some important things such as ActiveModel, cleaner internals and others have made their way into Rails 3)

But what's worse than everything listed so far is that the lack of competition and the internal rewrites made Rails lose its headstart.  Rails is very much HTML/view focused, its primarily strength is to make server side views trivial and it does an amazing job at that. But let's be honest, that's not the future for web dev. The future is more and more logic pushed to run on the client side (in JS) and the server side being used as an API serving data for the view layer. I'm sorry but adding support for CoffeeScript doesn't really do much to making Rails evolve ahead of what it currently is. Don't get me wrong, I'm a big fan of CoffeeScript, that said I still find that Rails is far from being optimized to developer web APIs in Rails. You can certainly do it, but you are basically using a tool that wasn't designed to write APIs and you pay the overhead for that. If there is one thing I wish Rails will get better at is to make writing pure web APIs better (thankfully there is Sinatra). But at the end of the day, I think that two projects with different philosophies and different approaches are really hard to merge, especially in the open source world. I wouldn't go as far as saying like others that Rails lost its sexiness to node.js because of the wasted time, but I do think that things would have been better for all if that didn't happen. However, I also have to admit that I'm not sure how much of a big deal that is. I prefer to leave the past behind, learn from my own mistake and move on.

 


## Technical debts


Here I'd like to stop to give a huge props to Aaron "[@tenderlove](http://twitter.com/tenderlove)" Patterson, the man who's actively working to reduce the [technical debts](http://en.wikipedia.org/wiki/Technical_debt) in the Rails code base. This is a really hard job and definitely not a very glamorous one. He's been working on various parts of Rails including its router and its ORM (ActiveRecord). Technical debts are unfortunately normal in most project, but sometimes they are overwhelming to the point that nobody dares touching the code base to clean it up. This is a hard problem, especially when projects move fast like Rails did. But looking back, I think that you want to start tackling technical debts on the side as you move on so you avoid getting to the point that you need a hero to come up and clean the piled errors made in the past. But don't pause your entire project to clean things up otherwise you will lose market, momentum and excitement. I feel that this is also very much true for any legacy project you might pick up as a developer.

 


## Keep the cost of entry level low


Getting started with Rails used to be easier. This can obviously argued since it's very subjective, but from my perspective I think we forgot where we come from and we involuntary expect new comers to come with unrealistic knowledge. Sure, Rails does much more than it used to do, but it's also much harder to get started. I'm not going to argue how harder  it is now or why we got there. Let's just keep in mind that it is a critical thing that should always be re-evaluated. Sure, it's harder when you have an open source project, but it's also up to the leadership to show that they care and to encourage and mentor volunteers to  focus on this important part of a project.

 


## Documentation


Rails documentation isn't bad, but it's far from being great. Documentation certainly isn't one of the Ruby's community strength, especially compared with the Python community, but what saddens me is to see the state of [the official documentation](http://guides.rubyonrails.org/) which, should, in theory be the reference. Note that the Rails guides are usually well written and provide value, but they too often seem too light and not useful when you try to do something not totally basic (for instance use an ActiveModel compliant object). That's probably why most people don't refer to them or don't spend too much time there. I'm not trying to blame anyone there. I think that the people who contributed theses guides did an amazing job, but if you want to build a strong and easy to access community, great documentation is key. Look at the [Django](https://docs.djangoproject.com/en/1.3/) documentation as a good example. That said, I also need to acknowledge the amazing job done by many community members such as [Ryan Bates](http://railscasts.com/) and [Michael Hartl](http://ruby.railstutorial.org/) consistently providing high value external documentation via the [railscasts](http://railscasts.com/) and the intro to [Rails tutorial](http://ruby.railstutorial.org/) available for free.

 

In conclusion, I think that there is a lot to learn from Rails, lots of great things as well as lots of things you would want to avoid. We can certainly argue on Hacker News or via comments about whether or not I'm right about Rails failures, my point will still be that the mentioned issues should be avoided in any projects, Rails here is just an example. Many of these issues are currently being addressed by the Rails team but wouldn't it be great if new projects learn from older ones and avoid making the same mistakes? So what other mistakes do you think I forgot to mention and that one should be very careful of avoiding?

 


### Updates:





	
  1. Rails 4 had an API centric app generator but it [was quickly reverted](https://github.com/rails/rails/commit/6db930cb5bbff9ad824590b5844e04768de240b1) and will live as gem until it's mature enough.

	
  2. Rails 4 improved the ActiveModel API to be simpler to get started with. See [this blog](http://blog.plataformatec.com.br/2012/03/barebone-models-to-use-with-actionpack-in-rails-4-0/) post for more info.


