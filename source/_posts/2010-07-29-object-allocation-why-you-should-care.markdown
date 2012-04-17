---
date: '2010-07-29 23:47:50'
layout: post
legacy_url: http://merbist.com/2010/07/29/object-allocation-why-you-should-care/
slug: object-allocation-why-you-should-care
source: merbist.com
status: publish
title: Ruby object allocation & why you should care
wordpress_id: '785'
categories:
- Misc
- rails
- ruby
- merbist.com
- blog-post
---

Recently I was tasked with finding how to optimize a web application with heavy traffic. The application (a Rails 2.3.x app) gets about 3 million requests per hour and most of these requests cannot really be easily cached so they go through the entire stack.

This is probably not the case of most web apps out there. None the less, my findings my help you understand Ruby better and maybe think differently about memory management.

This is certainly not an advanced GC blog post, I will try to keep it as simple as possible. My goal is to show you how Ruby memory allocation works and why it can affect your app performance and finally, how can you avoid to allocate to many objects.


## Ruby memory management.


Rubyists are quite lucky since they don't have to manage the memory themselves. Because developers are lazy and Matz developed his language for people and not machine, memory is managed "magically". Programming should be fun and managing memory isn't really considered fun (ask video game developers or iOS programmers ;)).

So in Ruby, the magical memory management is done by a Garbage Collector. The GC's job is to run and free objects that were previously allocated but not used anymore. Without a GC we would saturate the memory available on the host running the program or would have to deallocate the memory manually. Ruby's GC uses a conservative, stop the world, mark-and-sweep collection mechanism.  More simply, the garbage collection runs when the allocated memory for the process is maxed out. The GC runs and blocks all code from being executed and will free unused objects so new objects can be allocated.

Joe Damato did a great talk on that matter during last RailsConf

[Garbage Collection and the Ruby Heap](http://www.scribd.com/doc/32718051/Garbage-Collection-and-the-Ruby-Heap) 

The problem is that Ruby's GC was not designed to support hundred thousand objects allocation per second. Unfortunately, that's exactly what frameworks like Ruby on Rails do, and you might contribute to the problem too without even knowing it.


## Does it really matter?


I believe it does. In my case improving the object allocation means much better response time, less servers, less support and less headaches. You might think that servers are cheaper than developers. But more servers mean more developer time spent fixing bugs and more IT support. That's why I think, memory management is something Ruby developers should be aware of and should take in consideration, especially the ones writing frameworks, libraries or shared code.

I am using Ruby 1.9 so I could not profile my Rails 2.x app using [memprof](http://memprof.com/), instead I wrote a [simple and basic middleware](http://github.com/mattetti/GC-stats-middleware) that keeps track of the memory allocation/deallocation and GC cycles during a web request (Ruby1.9 only). One of my simple Rails2 actions (1 DB call, simple view) is allocating 170,000 objects per requests. Yes, you read right: 170k objects every single request. At 3 million requests/hour, you can imagine that we are spending a LOT of time waiting for the GC. This is obviously not 100% Rails fault as I am sure our code is contributing to the problem. I heard from the memprof guys that Rails was allocating 40k objects. I decided to check Rails3.

After warming up, a basic Rails3 'hello world' app clocks at about **8,500 objects allocated per request**, forcing the GC to run more or less every 6 requests. On my machine (mac pro) the GC takes about 20ms to free the objects. A Rack 'hello world' app clocks at **7 objects** per request and a Sinatra app at **181 objects**. Of course you can't really compare these different libraries/frameworks but that gives you an idea of the price you pay to get more features.

One thing to remember is that the more objects you allocate, the more time you "lose" at code execution. For more developers, it probably doesn't matter much, but if you should still understand that concept especially if you decide to contribute to the OSS community and offer patches, libraries, plugins etc...


# What can I do?


Be aware that you are allocating  objects, for instance something as simple as 100.times{ 'foo' } allocates 100 string objects (strings are mutable and therefore each version requires its own memory allocation).

Make sure to evaluate the libraries you use, for instance switching a Sinatra XML rendering action from Builder to Nokogiri XML Builder saved us about 12k object allocations (Thanks Aaron Patterson). Make sure that **if **you are using a library allocating a LOT of objects, that other alternatives are not available and your choice is worth paying the GC cost. (you might not have a lot of requests/s or might not care for a few dozen ms per requests). You can use memprof or one of the many existing tools to check on the GC cycles using load tests or in dev mode. Also, be careful to analyze the data properly and to not only look at the first request. [Someone](http://twitter.com/akeem) sent me [this memory dump](http://memprof.com/dump/4c52503c7fdeb62cff000001) from a Rails3 'hello world' with Ruby 1.8.7 and it shows that Rails is using [331973 objects](http://memprof.com/dump/4c52503c7fdeb62cff000001/detail?where=%7B%7D).  While this is totally true, it doesn't mean that 330k objects are created per request. Instead that means that 330k objects are currently in memory. Rubygems loading already allocate a lot of objects, Rails even more but these objects won't be GC'd and don't matter as much as the ones allocated every single request. The total amount of memory used by a Ruby process isn't that important, however the fluctuation forcing the GC to run often is. This is why my middleware only cares about the allocation change during a request. (The GC should still traverse the entire memory so, smaller is better)

The more object allocation you do at runtime/per request, the more the GC will need to run, the slower your code will be. So this is not a question of memory space, but more of performance. If your framework/ORM/library/plugin allocates too many objects per request maybe you should start by reporting the problem and if you can, offer some patches.

Here are some hints about memory allocation:

Creating a hash object really allocates more than an object, for instance {'joe' => 'male', 'jane' => 'female'} doesn't allocate 1 object but 7. (one hash, 4 strings + 2 key strings) If you can use symbol keys as they won't be garbage collected. However because they won't be GC'd you want to make sure to not use totally dynamic keys like converting the username to a symbol, otherwise you will 'leak' memory.

Looking at a GC cycle in the Rails3 hello world example shows what objects get deallocated:


> GC run, previous cycle was 6 requests ago.

GC 203 invokes. (amount of cycles since the program was started)
Index   1

Invoke Time(sec)   25.268

Use Size(byte)   4702440

Total Size(byte)   7307264

Total Object   182414

GC Time(ms) 22.35600000000204090611

## 56322 freed objects. ##
**[78%] 44334 freed strings.**
**[7%] 4325 freed arrays.**
[0%] 504 freed bignums.
[1%] 613 freed hashes.
[0%] 289 freed objects.
**[5%] 3030 freed parser nodes (eval usage).**


I did not list all the object types but it's pretty obvious that the main issue in the case of Rails is string allocation. To a certain extend the allocated arrays and the runtime use of eval are not helping either. (what is being eval'd at runtime anyway?)

If you use the same string in various place of you code, you can "cache" them using a local var, instance variable, class variable or constant. Sometimes you can just replaced them by a symbol and save a few allocations/deallocations per request. Whatever you do tho, make sure there is a real need for it. My rule of thumb is that if some code gets exercised by 80% of the requests, it should be really optimized and avoid extra allocations so the GC won't slow us down.


## What about a better GC?


That's the easy answer. When I mentioned this problem with Rails, a lot of people told me that I should use JRuby or Rubinius because their GC were much better. Unfortunately, that's not that simple and choosing an alternative implementation requires much further research and tests.

But what annoys me with this solution is that using it is not solving the issue, it's just working around it. Yes, Ruby's GC isn't that great but that's the not the key issue, **the key issue is that some libraries/frameworks allocate way too many objects** and that nobody seems to care (or to even know it). I know that the Ruby Core Team is working on some optimizations and I am sure Ruby will eventually get an improved GC. In the meantime, it's easy to blame Matz, Koichi and the rest of the core team but again, it's ignoring that the root cause, totally uncontrolled memory allocation.

**Maybe it's time for us, Rubyists, to think a bit more about our memory usage.**
