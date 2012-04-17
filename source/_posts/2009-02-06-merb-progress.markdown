---
date: '2009-02-06 14:09:27'
layout: post
legacy_url: http://merbist.com/2009/02/06/merb-progress/
slug: merb-progress
source: merbist.com
status: publish
title: merb progress
wordpress_id: '439'
categories:
- merb
- Misc
- News
- merbist.com
- blog-post
---

As they say: fail early, fail often. Well, I've been failing to blog recently, but as always I have some good excuses ;)



	
  * [Yehuda](http://yehudakatz.com/) has been blogging a lot about the work done on the merge.

	
  * I have been busy working on probably the [awesomest CouchDB Ruby DSL/ORM](http://github.com/mattetti/couchrest).

	
  * I have been working with the [Rails Activists](http://weblog.rubyonrails.com/activism) on the [new wiki](http://newwiki.rubyonrails.org) and many other projects.

	
  * My sister is visiting from France :)  (most of my free time is not spent in front of a computer anymore :p)

	
  * I've been playing with MacRuby (see the end of the post)

	
  * Ohh and spent some time maintaining Merb and preparing 1.0.9




## Merb 1.0.9


Merb 1.0.8.1 has some issues with the merb-cache settings set in init.rb being called twice when you use some Rake tasks. (rake db:automigrate for instance).The problem is due to the fact that the rake tasks load the dependencies twice:



	
  * Merb rake file loads all of your app dependencies and their rake tasks. (for instance rake db:automigrate is a rake task coming from merb_datamapper)

	
  * The task you are invoking might start merb itself to load the models etc.. and starting Merb reloads the dependencies.


Dependencies have the option to have a require block. A require block is a Proc that gets called when the dependency is being required. In the case of 1.8.1, we added a default block to set merb-cache. The problem was that the block was being set and called twice and merb-cache was complaining that the default cache was already setup.

To fix this issue we worked on a band-aid type solution (read: kinda evil but ok). Even if you start Merb multiple times, the init.rb file will no only load once and the dependency require blocks will only be called once. However, in the case of merb-cache, the setup block is being called everytime you go through the bootloader. That's why we added a verification on the block itself. In the long term, we are going to fix things nicely and optimize the way merb-cache loads.

We also addressed some other issues. Some people have been pointed out issues with some merb-helpers and patches were provided by the community (thanks a lot).

Talking about patches, we also accepted a patch fixing a small problem with merb-auth and some open-id servers.

Ruby 1.9.1: We started going through all the different places were things are not compatible yet, most of the work that needs to be done right away is focused on the router. Carl has been working on that and you can check his branch to see how we are doing. During the next week we will try to get things ironed out, we still don't know how we are going to deal with the fact that action-args uses parsetree which is not 1.9.1 compatible.

The upcoming version also gets a brand new feature: memory monitoring by the merb master process. The master process checks that the workers don't use too much memory ( you can set what you consider beign too much memory using Merb::Config[:max_memory]) and if one of the workers reaches the limit, it does a kill -1. It then waits a configured amount of seconds (defaults to 5) then kill using -9.

Finally, we also fixed a bunch of tiny issues.

1.0.9 should be released in the next few days.



* * *




## MacRuby


Finally, because for those interested in MacRuby here are my slides from last night's SDRuby meetup:


[MacRuby - When objective-c and Ruby meet](http://www.slideshare.net/mattetti/macruby-when-objectivec-and-ruby-meet?type=presentation)


View more [presentations](http://www.slideshare.net/) from [Matt Aimonetti](http://www.slideshare.net/mattetti).



