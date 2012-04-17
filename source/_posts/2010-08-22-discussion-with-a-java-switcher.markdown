---
date: '2010-08-22 17:18:55'
layout: post
slug: discussion-with-a-java-switcher
status: publish
title: Discussion with a Java switcher
wordpress_id: '796'
categories:
- blog-post
- Misc
- Rails
- Ruby
tags:
- rails
- ruby
---

For the past 6 months, I have had regular discussions with an experienced Java developers who switched to Ruby a couple years ago. Names have been changed to protect the guilty but to help you understand my friend 'Duke' better, you need to know that he has been a developer for 10 years and lead many complicated, high traffic projects. He recently released two Ruby on Rails projects and he has been fighting with performance issues and scalability challenges.

Duke is a happy Ruby developer but he sometimes has a hard time understanding why things are done in a certain way in the Ruby community. Here are some extracts from our conversations. My answers are only based on my own experience and limited knowledge. They are probably not shared by the entire  community, feel free to use the comment section if you want to add more or share your own answers.


## Threads / Concurrency


**Duke:** Why does the Ruby community hate threads so much. It seems to be a taboo discussion and the only answer I hear is that threads are hard to deal with and that Ruby does not have a good threading implementation. What's the deal there? If you want concurrent processing, threads are important!

**Me:** This is a very good question and I think there are two main reasons why threads and thread safety are not hot topics in the Ruby world. First, look at Ruby's main implementation itself. If you are using an old version of Ruby (pre Ruby 1.9) you don't use native threads but green threads mapping to only 1 native thread. Ilya has a great (yet a bit old) [blog post explaining the difference](http://www.igvita.com/2008/11/13/concurrency-is-a-myth-in-ruby/), why it matters and also the role and effect of the Global Interpreter Lock (GIL). Also, even though Rubyists like to say that they live in the edge, most of them still use Ruby 1.8 and therefore don't really see the improvements in Ruby 1.9 nor yet understand the potential of [fibers](http://ruby-doc.org/core-1.9/classes/Fiber.html).

The other part of the explanation is that the Rails community never really cared until recently. Yehuda Katz recently wrote a [good article on thread safety](http://yehudakatz.com/2010/08/14/threads-in-ruby-enough-already/) in Ruby and if you read his post and [Zed Shaw's comment](http://dpaste.de/5xyG/raw/) you will understand a bit better the historical background. As a matter of fact, the current version of Rails is not multi-threaded by default and developers interested in handling concurrent requests in one process should [turn on this option](http://api.rubyonrails.org/classes/Rails/Configuration.html#M002069). Thread safety appeared for the first time in Rails 2.2 but from what I saw, most people still don't enable this option. There are many reasons for that. First, enabling thread safety disables some Rails features like automatic dependency loading after boot and code reloading. A lot of Rails developers take these two features for granted and don't understand that they are technically "hacks" to make their lives easier. I do believe a lot of Rails developers don't understand how threads, thread safety, concurrency, blocking IO and dependencies work. They care about getting their app done and meet their deadlines. They usually use and know Rails without paying too much attention to how Rails extends Ruby. Imagine what would happen if their code wasn't thread safe and Rails wasn't not using a global lock by default. Now you see why things are not exactly as you expect and also why some Rubyists are getting excited about new projects like [node.js](http://nodejs.org/) which takes a different approach.

The other thing to keep in mind is that at least 90 to 95% of the Rails apps out there don't get more than a dozen requests/second (a million requests/day). You can scale that kind of load pretty easily using simple approaches like caching,  optimize your DB queries, load balancing to a couple servers. As a matter of fact, compared to the amount of people using Rails on a daily basis, only a very little amount of people are struggling with performance and scalability like you do. This is not an excuse but that explains why these people don't care about the things you care about.


## Rails is slow


**Duke:** I don't understand why Rails developers are not more concerned about the speed/performance penalty induced by Rails.

**Me:** Again, Rails is fast enough for the large majority of developers out there. As you know, as a developer you have to always make compromises. The Rails team always said that development time is more expensive than servers and therefore the focus is on making development easier, faster and more enjoyable. However to get there, they have to somewhat sacrifice some performance. What can be totally unacceptable for you is totally fine for others and your contribution is always welcome. This is probably the root cause of the things you don't like in Rails. Rails was built for startups, by startup developers and you don't fall in this category. People contributing new features and fixes are the people using Rails for what it is designed to do. There is no real 'Enterprise' support behind Rails and that might be why you feel the way you feel. Since you find yourself questioning some key Rails conventions and you are struggling with missing features, it looks  to me that you chose the wrong tool for the job since you don't even use 70% of the Rails features and are dreaming of things such 3 tier architecture. [Sinatra](http://sinatrarb.com) might be a better fit for you if you want lower level control, less conventions and less built-in features.


## Object allocation / Garbage Collection


**Duke:** I recently read that Twitter was spending [20% of its request cycles in the GC](http://blog.evanweaver.com/articles/2009/10/21/object-allocations-on-the-web/), am I the only finding that concerning?

**Me:** Most people don't realize how the GC works and what it means to allocate objects since Ruby does that automatically. But at the same time, most of these people don't really see the affect of the Garbage Collection since they don't have that much traffic or they scale in ways that just skips their Ruby stack entirely. (Or they just blame Ruby for being slow)

If you are app deals with mainly reads/GET requests, using HTTP caching (Rails has that built-in) and something like Varnish/[Rack-cache](http://rtomayko.github.com/rack-cache/) will dramatically reduce the load on your server apps. Others don't investigate their issues and just add more servers. As mentioned in a [previous post](http://merbist.com/2010/07/29/object-allocation-why-you-should-care/), some libraries like Builder are allocating LOTS more objects than others (Nokogiri), use the existing debugging tools to see where your object allocations occur and try to fix/workaround these. In other words, Ruby's GC isn't great but by ignoring its limitations, we made things even worse. My guess is that the GC is going to improve (other implementations already have better GCs) and that people will realize that Ruby is not magic and critical elements need to be improved.


## Tools


**Duke:** I really have a hard time finding good tools to help scale my apps better and understand where I should optimize my code.

**Me: **It is true that we area lacking tools but things are changing. On top of the built-in tools like [ObjectSpace](http://ruby-doc.org/core-1.9/classes/ObjectSpace.html), [GC::Profiler](http://ruby-doc.org/core-1.9/classes/GC/Profiler.html), people interested in performance/debugging are working to provide the Ruby community with their expertise, look at [memprof](http://memprof.com/) and [ruby-debug](http://rubyforge.org/projects/ruby-debug/) for instance. Of course you can also use tools such as [Ruby-prof](http://ruby-prof.rubyforge.org/), [Kcachegrind](http://kcachegrind.sourceforge.net/html/Home.html), [Valgrind](http://valgrind.org/) and [GDB](http://www.gnu.org/software/gdb/). (1.9.2 was [scheduled to have DTrace support](http://github.com/yugui/ruby/tree/feature/dtrace) but I did not check yet). Maybe you should be more explicit about what tools you miss and how we could solve the gap.


## ActiveRecord


**Duke:** ActiveRecord doesn't do what I need. How come there is no native support for master/slave DBs, sharding, DB view support is buggy,  suggested indexes on queries is not built-in and errors are not handled properly (server is gone, out of sync etc..)?

**Me:** You don't have to use ActiveRecord, you could use any ORM such as [Sequel](http://sequel.rubyforge.org/), [DataMapper](http://datamapper.org/) or your own. But to answer your question, I think that AR doesn't do everything you want because nobody contributed these features to the project and the people maintaining ActiveRecord don't have the need for these features.


## What can we do?


We, as a community, need to realize that we have to learn from other communities and other programming languages, this kind of humorous graph is unfortunately not too far from reality.


![](http://i.imgur.com/G7WyP.gif)


Bringing your expertise and knowledge to the Ruby community is important. Looking further than just our own little will push us to improve and fulfill the gaps. Let the community know what tools you are missing, the good practices you think we should be following etc...

Take for instance [Node.js](http://nodejs.org/), it's a port of [Ruby's EventMachine](http://wiki.github.com/eventmachine/eventmachine/) / [Python's twisted](http://twistedmatrix.com/trac/). There is no reasons why the Ruby or Python versions could not do what the Javascript version does. However people are getting excited and are jumping ship. What do we do about that? One way would be to identify what makes node more attractive than EventMachine and what needs to be done so we can offer what people are looking for. I asked this question a few weeks ago and the response was that a lot of the Ruby libraries are blocking and having to check is too bothersome. Maybe that's something that the community should be addressing. Node doesn't have that many libraries and people will have to write them, in the mean time we can make our libs non-blocking. Also, let's not forget that this is not a competition and people should choose the best tool for their projects.

Finally, things don't change overnight, as more people encounter the issues you are facing, as we learn from others, part of the community will focus on the problems you are seeing and things will get better. Hopefully, **you** will also be able to contribute and influence the community to build an even better Ruby world.
