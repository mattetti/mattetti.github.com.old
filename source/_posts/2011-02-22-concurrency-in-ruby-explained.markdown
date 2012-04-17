---
date: '2011-02-22 22:34:30'
layout: post
legacy_url: http://merbist.com/2011/02/22/concurrency-in-ruby-explained/
slug: concurrency-in-ruby-explained
source: merbist.com
status: publish
title: Ruby concurrency explained
wordpress_id: '978'
categories:
- ruby
- Software Design
- merbist.com
- blog-post
tags:
- async
- concurrency
- EventMachine
- fibers
---

Concurrency is certainly [not a new problem](http://en.wikipedia.org/wiki/Petri_Net) but it's getting more and more attention as machines start having more than 1 core, that web traffic increases drastically and that some new technologies show up saying that they are better because they handle concurrency better.
If that helps, think of concurrency as multitasking. When people say that they want concurrency, they say that they want their code to do multiple different things at the same time. When you are on your computer, you don't expect to have to choose between browsing the web and listening to some music. You more than likely want to run both concurrently. It's the same thing with your code, if you are running a webserver, you probably don't want it to only process one request at a time.
The aim of this article is to explain as simply as possible the concept of concurrency in Ruby, the reason why it's a complicated topic and finally the different solutions to achieve concurrency.

First off, if you are not really familiar with concurrency, take a minute to [read the wikipedia article on the topic](http://en.wikipedia.org/wiki/Concurrency_%28computer_science%29) which is a great recap on the subject. But now, you should have noticed that my above example was more about parallel programming than concurrency, but we'll come back to that in a minute.


> **The real question at the heart of the quest for concurrency is: "how to increase code throughput".**


We want our code to perform better, and we want it to do more in less time. Let's take two simple and concrete examples to illustrate concurrency. First, let's pretend you are writing a twitter client, you probably want to let the user scroll his/her tweets while the latest updates are  being fetched. In other words, you don't want to block the main loop and interrupt the user interaction while your code is waiting for a response from the Twitter API. To do that, a common solution is to use multiple **threads**. Threads are basically processes that run in the same memory context. We would be using one thread for the main event loop and another thread to process the remote API request. Both threads share the same memory context so once the Twitter API thread is done fetching the data it can update the display. Thankfully, this is usually transparently handled by asynchronous APIs (provided by the OS or the programming language std lib) which avoid blocking the main thread.

The second example is a webserver. Let's say you want to run a Rails application. Because you are awesome, you expect to see a lot of traffic. Probably more than 1 QPS (query/request per second). You benchmarked your application and you know that the average response time is approximately 100ms. Your Rails app can therefore handle 10QPS using a single process (you can do 10 queries at 100ms in a second).

But what happens if your application gets more than 10 requests per second? Well, it's simple, the requests will be backed up and will take longer until some start timing out. This is why you want to improve your concurrency. There are different ways to do that, a lot of people feel really strong about these different solutions but they often forget to explain why they dislike one solution or prefer one over the other. You might have heard people conclusions which are often one of these: [Rails can't scale](http://canrailsscale.com/), you only get concurrency with [JRuby](http://jruby.org/), [threads suck](http://adam.heroku.com/past/2009/8/13/threads_suck/), the only way to concurrency is via threads, we should switch to [Erlang](http://www.erlang.org/)/[Node.js](http://nodejs.org/)/[Scala](http://www.scala-lang.org/), use[ fibers](http://www.rubyinside.com/fibers-eventmachine-rack-performance-gains-3395.html) and you will be fine, add more machines, [forking > threading](http://tomayko.com/writings/unicorn-is-unix).  Depending on who said what and how often you heard it on twitter, conferences, blog posts, you might start believing what others are saying. But do you really understand why people are saying that and are you sure they are right?

The truth is that this is a complicated matter. The good news is that it's not _THAT_ complicated!

The thing to keep in mind is that the concurrency models are often defined by the programming language you use. In the case of Java, [threading is the usual solution](http://download.oracle.com/javase/tutorial/essential/concurrency/index.html), if you want your Java app to be more concurrent, just run every single request in its own thread and you will be fine (kinda). In PHP, you simply don't have threads, instead you will start a new process per request. Both have pros and cons, the advantage of the Java threaded approach is that the memory is shared between the threads so you are saving in memory (and startup time), each thread can easily talk to each other via the shared memory. The advantage of PHP is that you don't have to worry about locks, deadlocks, threadsafe code and all that mess hidden behind threads. Described like that it looks pretty simple, but you might wonder why PHP doesn't have threads and why Java developers don't prefer starting multiple processes. The answer is probably related to the language design decisions. PHP is a language designed for the web and for short lived processes. PHP code should be fast to load and not use too much memory. Java code is slower to boot and to warm up, it usually uses quite a lot of memory. Finally, Java is a general purpose programming language not designed primarily for the internet. Others programming languages like [Erlang](http://www.erlang.org/) and [Scala](http://www.scala-lang.org/) use a third approach: [the actor model](http://en.wikipedia.org/wiki/Actor_model). The actor model is somewhat a bit of a mix of both solutions, the difference is that actors are a like threads which don't share the same memory context. Communication between actors is done via exchanged messages ensuring that each actor handles its own state and therefore avoiding corrupt data (two threads can modify the same data at the same time, but an actor can't receive two messages at the exact same time). We'll talk about that design pattern later on, so don't worry if you are confused.

What about Ruby? Should Ruby developers use threads, multiple processes, actors, something else? The answer is: **yes**!


## Threads


Since version 1.9, Ruby has native threads (before that [green threads](http://en.wikipedia.org/wiki/Green_threads) were used). So in theory, if we would like to, we should be able to use threads everywhere like most Java developers do. Well, that's almost true, the problem is that Ruby, like Python uses a [Global Interpreter Lock](http://en.wikipedia.org/wiki/Global_Interpreter_Lock) (aka GIL). This GIL is a locking mechanism that is meant to protect your data integrity. The GIL only allows data to be modified by one thread at time and therefore doesn't let threads corrupt data but also it doesn't allow them to truly run concurrently. That is why some people say that Ruby and Python are not capable of (true) concurrency.


![Global Interpreter Lock by Matt Aimonetti](https://img.skitch.com/20110223-kk58iq5yjdpmyswf7nuya4c4kp.jpg)


However these people often don't mention that the GIL makes single threaded programs faster, that multi-threaded programs are much easier to develop since the data structures are safe and finally that a lot of C extensions are not thread safe and without the GIL, these C extensions don't behave properly. These arguments don't convince everyone and that's why you will hear some people say you should look at another Ruby implementation without a GIL, such as [JRuby](http://jruby.org/), [Rubinius](http://rubini.us/) (hydra branch) or [MacRuby](http://macruby.org) (Rubinius & MacRuby also offer other concurrency approaches). If you are using an implementation without a GIL, then using threads in Ruby has exactly the same pros/cons than doing so in Java. However, it means that now you have to deal with the nightmare of threads: making sure your data is safe, doesn't deadlock, check that your code, your libs, plugins and gems are thread safe. Also, running too many threads might affect the performance because your OS doesn't have enough resources to allocate and it ends up spending its time context switching. It's up to you to see if it's worth it for your project.


## Multiple processes & forking


That's the most commonly used solution to gain concurrency when using Ruby and Python. Because the default language implementation isn't capable of true concurrency or because you want to avoid the challenges of thread programming, you might want to just start more processes. That's really easy as long as you don't want to share states between running processes. If you wanted to do so, you would need to use [DRb](http://segment7.net/projects/ruby/drb/introduction.html), a message bus like [RabbitMQ](http://www.rabbitmq.com/), or a shared data store like memcached or a DB. The caveat is that you now need to use a LOT more memory. If want to run 5 Rails processes and your app uses 100Mb you will now need 500Mb, ouch that's a lot of memory! That is exactly what happens when you use a Rails webserver like Mongrel. Now some other servers like [Passenger](http://www.modrails.com/) and [Unicorn](http://unicorn.bogomips.org/) found a workaround, they rely on [unix forking](http://en.wikipedia.org/wiki/Fork_%28operating_system%29). The advantage of forking in an unix environment implementing the copy-on-write semantics is that we create a new copy of the main process but they both "share" the same physical memory. However, each process can modify its own memory without affecting the other processes. So now, Passenger can load your 100Mb Rails app in a process, then fork this process 5 times and the total footprint will be just a bit more than 100Mb and you can now handle 5X more concurrent requests. Note that if you are allocating memory in your request processing code (read controller/view) your overall memory will grow but you can still run many more processes before running out of memory. This approach is appealing because really easy and pretty safe. If a forked process acts up or leaks memory, just destroy it and create a new fork from the master process. Note that this approach is also used in [Resque](https://github.com/defunkt/resque), the async job processing solution by [GitHub](http://github.com).

This solution works well if you want to duplicate a full process like a webserver, however it gets less interesting when you just want to execute some code "in the background". Resque took this approach because by nature async jobs can yield weird results, leak memory or hang. Dealing with forks allows for an external control of the processes and the cost of the fork isn't a big deal since we are already in an async processing approach.


![](http://s3.amazonaws.com/cogit8-org/img/hardcore-forking-action.png)





## Actors/Fibers


Earlier we talked a bit about the [actor model](http://en.wikipedia.org/wiki/Actor_model). Since Ruby 1.9, developers now have access to a new type of "lightweight" threads called [Fibers](http://www.ruby-doc.org/core-1.9/classes/Fiber.html). Fibers are not actors and Ruby doesn't have a native Actor model implementation but some people wrote [some actor libs](http://doc.revactor.org/files/README.html) on top of fibers. A fiber is like a simplified thread which isn't scheduled by the VM but by the programmer. Fibers are like blocks which can be paused and resumed from the outside of from within themselves. Fibers are faster and use less memory than threads as demonstrated in [this blog post](http://oldmoe.blogspot.com/2008/08/ruby-fibers-vs-ruby-threads.html). However, because of the GIL, you still cannot truly run more than one concurrent fiber by thread and if you want to use multiple CPU cores, you will need to run fibers within more than one thread. So how do fibers help with concurrency? The answer is that they are part of a bigger solution. Fiber allow developers to manually control the scheduling of "concurrent" code but also to have the code within the fiber to auto schedule itself. That's pretty big because now you can wrap an incoming web request in its own fiber and tell it to send a response back when it's done doing its things. In the meantime, you can move on the to next incoming request. Whenever a request within a fiber is done, it will automatically resume itself and be returned. Sounds great right? Well, the only problem is that if you are doing any type of blocking IO in a fiber, the entire thread is blocked and the other fibers aren't running. Blocking operations are operations like database/memcached queries, http requests... basically things you are probably triggering from your controllers. The good news is that the "only" problem to fix now is to avoid blocking IOs. Let's see how to do that.


![fiber](https://img.skitch.com/20110223-8wkfs2g12p15ku18rm7aq9negf.jpg)





## Non blocking IOs/Reactor pattern.


The reactor pattern is quite simple to understand really. The heavy work of making blocking IO calls is delegated to an external service (reactor) which can receive concurrent requests. The service handler (reactor) is given callback methods to trigger asynchronously based on the type of response received. Let me take a limited analogy to hopefully explain the design better. It's a bit like if you were asking someone a hard question, the person will take a while to reply but his/her reply will make you decide if you raise a flag or not. You have two options, or you choose to wait for the response and decide to raise the flag based on the response, or your flag logic is already defined and you tell the person what to do based on their answer and move on without having to worry about waiting for the answer. The second approach is exactly what the reactor pattern is. It's obviously slightly more complicated but the key concept is that it allows your code to define methods/blocks to be called based on the response which will come later on.


![Reactor from Matt Aimonetti's blog](https://img.skitch.com/20110223-xkit6utnty1sdt84n15w7dgtnh.jpg)


In the case of a single threaded webserver that's quite important. When a request comes in and your code makes a DB query, you are blocking any other requests from being processed. To avoid that, we could wrap our request in a fiber, trigger an async DB call and pause the fiber so another request can get processed as we are waiting for the DB. Once the DB query comes back, it wakes up the fiber it was trigger from, which then sends the response back to the client. Technically, the server can still only send one response at a time, but now fibers can run in parallel and don't block the main tread by doing blocking IOs (since it's done by the reactor).

This is the approach used by [Twisted](http://twistedmatrix.com/trac/), [EventMachine](http://eventmachine.rubyforge.org/EventMachine/Deferrable.html) and [Node.js](http://nodejs.org/). Ruby developers can use EventMachine or an EventMachine based webserver like [Thin](http://code.macournoyer.com/thin/) as well as [EM clients/drivers](https://github.com/igrigorik/em-synchrony) to make non blocking async calls. Mix that with some Fiber love and you get Ruby concurrency. Be careful though, using Thin, non blocking drivers and Rails in threadsafe mode doesn't mean you are doing concurrent requests. Thin/EM only use one thread and you need to let it know that it's ok to handle the next request as we are waiting. This is done by [deferring the response](http://eventmachine.rubyforge.org/EventMachine/Deferrable.html) and let the reactor know about it.

The obvious problem with this approach is that it forces you to change the way you write code. You now need to set a bunch of callbacks, understand the Fiber syntax, and use deferrable responses, I have to admit that this is kind of a pain. If you look at some Node.js code, you will see that it's not always an [elegant approach](http://howtonode.org/control-flow-part-ii/file-write.js). The good news tho, is that this process can be wrapped and your code can be written as it if was processed synchronously while being handled asynchronously under the covers. This is a bit more complex to explain without showing code, so this will be the topic of a future post. But I do believe that things will get much easier soon enough.


## Conclusion


High concurrency with Ruby is doable and done by many. However, it could made easier. Ruby 1.9 gave us fibers which allow for a more granular control over the concurrency scheduling, combined with non-blocking IO, high concurrency can be achieved. There is also the easy solution of forking a running process to multiply the processing power. However the real question behind this heated debate is what is the future of the Global Interpreter Lock in Ruby, should we remove it to improve concurrency at the cost of dealing with some new major threading issues, unsafe C extensions, etc..? Alternative Ruby implementers seem to believe so, but at the same time Rails still ships with a default mutex lock only allowing requests to be processed one at a time, the reason given being that a lot of people using Rails don't write thread safe code and a lot of plugins are not threadsafe. Is the future of concurrency something more like [libdispatch](http://libdispatch.macosforge.org/)/[GCD](http://www.macruby.org/documentation/gcd.html) where the threads are handled by the kernel and the developer only deals with a simpler/safer API?

Further reading:



	
  * [Concurrency is a myth in Ruby](http://www.igvita.com/2008/11/13/concurrency-is-a-myth-in-ruby/)

	
  * [Ruby fibers vs Ruby threads](http://oldmoe.blogspot.com/2008/08/ruby-fibers-vs-ruby-threads.html)

	
  * [Multi-core, threads, passing messages](http://www.igvita.com/2010/08/18/multi-core-threads-message-passing/)

	
  * [Threads suck](http://adam.heroku.com/past/2009/8/13/threads_suck/)

	
  * [Non blocking Active Record and Rails](http://www.igvita.com/2010/04/15/non-blocking-activerecord-rails/)

	
  * [Scalable Ruby processing with EventMachine](http://www.mikeperham.com/2010/01/27/scalable-ruby-processing-with-eventmachine/)

	
  * [Ruby concurrency with actors](http://on-ruby.blogspot.com/2008/01/ruby-concurrency-with-actors.html)

	
  * [Concurrency in MRI; threads](http://www.engineyard.com/blog/2010/concurrency-real-and-imagined-in-mri-threads/)

	
  * [Ruby 1.9 adds fibers for lightweight concurrency](http://www.infoq.com/news/2007/08/ruby-1-9-fibers)

	
  * [Threads in Ruby, enough already](http://yehudakatz.com/2010/08/14/threads-in-ruby-enough-already/)

	
  * [Untangling Evented Code with Ruby Fibers](http://www.igvita.com/2010/03/22/untangling-evented-code-with-ruby-fibers)

	
  * [Elise Huard's RubyConf Concurrency talk slides](http://www.slideshare.net/ehuard/concurrency-5615029)


