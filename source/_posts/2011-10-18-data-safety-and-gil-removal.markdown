---
date: '2011-10-18 15:19:17'
layout: post
legacy_url: http://merbist.com/2011/10/18/data-safety-and-gil-removal/
slug: data-safety-and-gil-removal
source: merbist.com
status: publish
title: Data safety and GIL removal
wordpress_id: '1188'
categories:
- Misc
- ruby
- merbist.com
- blog-post
tags:
- concurrency
- gil
- macruby
- python
- ruby
---

After my recent [RubyConf talk](http://rubyconf11.merbist.com) and [follow up post addressing the Ruby & Python's Global Interpreter Lock](http://merbist.com/2011/10/03/about-concurrency-and-the-gil/) (aka GVL/Global VM Lock). a lot of people asked me to explain what I meant by "data safety". While my point isn't to defend one approach or the other, I spent a lot of time explaining why C Ruby and C Python use a GIL and where it matters and where it matters less. As a reminder and as mentioned by Matz himself, the main reason why C Ruby still has a GIL is data safety. But if this point isn't clear to you, you might be missing the main argument supporting the use of a GIL.

Showing obvious concrete examples of data corruption due to unsafe threaded code isn't actually as easy at it sounds. First of all, even with a GIL, developers can write unsafe threaded code. So we need to focus only on the safety problems raised by removing the GIL. To demonstrate what I mean, I will try to create some race conditions and show you the unexpected results you might get. Again, before you go crazy on the comments, remember that threaded code is indeterministic and the code below might potentially work on your machine and that's exactly why it is hard to demonstrate. Race conditions depend on many things, but in this case I will focus on race conditions affecting basic data structures since it might be the most surprising.


## Example:



    
    @array, threads = [], []
    4.times do
      threads << Thread.new { (1..100_000).each {|n| @array << n} }
    end
    threads.each{|t| t.join }
    puts @array.size


In the above example, I'm creating an instance variable of Array type and I start 4 threads. Each of these threads adds 100,000 items to the array. We then wait for all the threads to be done and check the size of the array.

If you run this code in C Ruby the end result will be as expected:

    
    400000


Now if you switch to JRuby you might be surprised by the output. If you are lucky you will see the following:

    
    ConcurrencyError: Detected invalid array contents due to unsynchronized modifications with concurrent users
            << at org/jruby/RubyArray.java:1147
      __file__ at demo.rb:3
          each at org/jruby/RubyRange.java:407
      __file__ at demo.rb:3
          call at org/jruby/RubyProc.java:274
          call at org/jruby/RubyProc.java:233


This is actually a good thing. JRuby detects that you are unsafely modifying an instance variable across threads and that data corruption will occur. However, the exception doesn't always get raised and you will potentially see results such as:

    
    335467
    342397
    341080


This is a sign that the data was corrupted but that JRuby didn't catch the unsynchronized modification. On the other hand MacRuby and Rubinius 2 (dev) won't raise any exceptions and will just corrupt the data, outputting something like:

    
    294278
    285755
    280704
    279865


In other words, if not manually synchronized, shared data can easily be corrupted. You might have two threads modifying the value of the same variable and one of the two threads will step on top of the other leaving you with a race condition. You only need 2 threads accessing the same instance variable at the same time to get a race condition. My example uses more threads and more mutations to make the problem more obvious. Note that TDD wouldn't catch such an issue and even extensive testing will provide very little guarantee that your code is thread safe.

 


## So what? Thread safety isn't a new problem.


That's absolutely correct, ask any decent Java developer out there, he/she will tell how locks are used to "easily" synchronize objects to make your code thread safe. They might also mention the deadlocks and other issues related to that, but that's a different story. One might also argue that when you write web apps, there is very little shared data and the chances of corrupting data across concurrent requests is very small since most of the data is kept in a shared data store outside of the process.

All these arguments are absolutely valid, the challenge is that you have a large community and a large amount of code out there that expects a certain behavior. And removing the GIL does change this behavior. It might not be a big deal for you because you know how to deal with thread safety, but it might be a big deal for others and C Ruby is by far the most used Ruby implementation. It's basically like saying that automatic cars shouldn't be made and sold, and everybody has to switch to stick shifts. They have better gas mileage, I personally enjoy driving then and they are cheaper to build. Removing the GIL is a bit like that. There is a cost associated with this decision and while this cost isn't insane, the people in charge prefer to not pay it.

 


## Screw that, I'll switch to Node.js


I heard a lot of people telling me they were looking into using Node.js because it has a better design and no GIL. While I like Node.js and if I were to implement a chat room or an app keeping connections for a long time, I would certainly compare it closely to EventMachine, I also think that this argument related to the GIL is absurd. First, you have other Ruby implementations which don't have a GIL and are really stable (i.e: JRuby) but then Node basically works the same as Ruby with a GIL. Yes, Node is evented and single threaded but when you think about it, it behaves the same as Ruby 1.9 with its GIL. Many requests come in and they are handled one after the other and because IO requests are non-blocking, multiple requests can be processed concurrently but not in parallel. Well folks, that's exactly how C Ruby works too, and unlike popular believe, most if not all the popular libraries making IO requests are non blocking (when using 1.9). So, next time you try to justify you wanting to toy with Node, please don't use the GIL argument.

 


## What should I do?


As always, evaluate your needs and see what makes sense for your project. Start by making sure you are using Ruby 1.9 and your code makes good use of threading. Then look at your app and how it behaves, is it CPU-bound or IO-bound. Most web apps out there are IO-bound (waiting for the DB, redis or API calls), and when doing an IO call, Ruby's GIL is released allowing another thread to do its work. In that case, not having a GIL in your Ruby implementation won't help you. However, if your app is CPU-bound, then switching to JRuby or Rubinius might be beneficial. However, don't assume anything until you proved it and remember that making such a change will more than likely require some architectural redesign, especially if using JRuby.  But, hey, it might totally be worth it as many proved it in the past.

 

I hope I was able to clarify things a bit further. If you wish to dig further, I would highly recommend you read the many discussions the Python community had in the last few years.

 

 

 

 
