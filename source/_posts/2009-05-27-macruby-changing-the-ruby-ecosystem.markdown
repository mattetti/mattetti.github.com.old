---
date: '2009-05-27 12:32:05'
layout: post
legacy_url: http://merbist.com/2009/05/27/macruby-changing-the-ruby-ecosystem/
slug: macruby-changing-the-ruby-ecosystem
source: merbist.com
status: publish
title: MacRuby, changing the Ruby ecosystem
wordpress_id: '497'
categories:
- macruby
- ruby
- merbist.com
- blog-post
tags:
- apple
- cocoa
- hotcocoa
- implementations
- llvm
- macruby
- objective-c
- ruby
---

### What's MacRuby?


[MacRuby](http://www.macruby.org/) is an Apple-sponsored, open source, full Ruby implementation on top of [Objective-C](http://en.wikipedia.org/wiki/Objective-C) runtime. In other words, whatever code runs on Ruby 1.9, should/will run on MacRuby. Yes, you read correctly, MacRuby can/will be able to run all your Ruby code. That means that eventually you will even be able to run your [Rails](http://en.wikipedia.org/wiki/Ruby%20on%20Rails)/[Sinatra](http://en.wikipedia.org/wiki/Sinatra%20%28software%29)/new-sexy-ruby-framework app on MacRuby.

Unlike [RubyCocoa](http://en.wikipedia.org/wiki/RubyCocoa), MacRuby is not a bridge, it is a full implementation of the Ruby language on top of Apple's Objective-C runtime. Taking a huge shortcut, MacRuby implements the Ruby syntax by aliasing it to the Obj-C language counterpart. A Ruby string instance is really in fact, an instance of [NSMutableString](http://developer.apple.com/documentation/Cocoa/Reference/Foundation/Classes/NSMutableString_Class/Reference/Reference.html). This is obviously transparent for you as a developer since you have the same Ruby API, but it also means that MacRuby can make use of the various Objective-C's goodies such as native threads, [ garbage collector](http://en.wikipedia.org/wiki/Objective-c#Garbage_collection) in the background as well as the runtime performance.

On top of that, you have full access to the Obj-C API from your Ruby code. (tip: in macirb, try "my string".methods(true, true).sort to see the available Ruby + Objective-C methods on your String instance)  The reason why having access to Objective-C is important is because it gives you the possibility to write native [Cocoa](http://en.wikipedia.org/wiki/Cocoa%20%28API%29)apps using Ruby. For those who don't know Cocoa, is a set of APIs for MacOSX development.

However, note that even though, the Cocoa support is almost complete and stable, MacRuby is still in development, especially on the Ruby side of things.


### What is it not?





	
  * [MacRuby](http://www.macruby.org/) is not a fork of Ruby. Full [rubyspec](http://rubyspec.org/) compliance is expected! It's true that MacRuby supports smalltalk/Obj-C method selectors so it might be considered a language superset.

	
  * MacRuby is not limited to the OSX platform. All its dependencies are open source and could possibly be compiled for other POSIX-based systems such as Linux.. (not done yet)

	
  * Even if MacRuby's primary goal is to allow you to write efficient Cocoa apps, it does not mean that MacRuby is limited to that.

	
  * MacRuby doesn't require you to learn Objective-C in order to develop Cocoa apps. (you just need to understand Obj-C selectors)




### What's coming up?


The current version of MacRuby (today being the 27th of May 2009) is version 0.4. You might have heard of things about MacRuby crazy performance, [LLVM](http://en.wikipedia.org/wiki/Low%20Level%20Virtual%20Machine), [AOT compilation](http://en.wikipedia.org/wiki/AOT%20compiler)[AOT compilation](http://en.wikipedia.org/wiki/AOT%20compiler) etc... This is all happening in the 'experimental' branch.

What's going on is that up to MacRuby 0.5, MR was  using YARV (Ruby 1.9 interpreter) on top of Obj-C and which obviously limited MacRuby's  to YARV's performance. After RubyConf 2008, Laurent Sansonetti, MacRuby's lead developer, decided to try removing YARV to only keep its [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) and experiment with using [LLVM](http://en.wikipedia.org/wiki/Llvm) instead of the 1.9 interpreter.

This switch turned out to be very promising and was noticed right away by influential people such as [IBM's Antonio Cangiano](http://antoniocangiano.com/2009/03/29/why-macruby-matters/). Since then, performance and compatibility have increased. Laurent even started working on an Ahead Of Time (AOT) compiler: [http://pastie.org/485095](http://pastie.org/485095) What's really impressive is that in this specific example (Fibonacci sequence), **MacRuby's compiled code is faster than Objective-C!** But let's not jump the gun. First this is a very very early prototype and most of your apps won't be using Fib. sequences ;) In this case, MacRuby's recursive method dispatch is faster but again, this is just a proof of concept and even though MacRuby is getting close to Obj-C speed, it's still far from matching Obj-C's impressive performance.

What this basically means is that you will be able to compile your Ruby code down to binary code. Imagine, taking your Rails app and compiling it down to a binary file that you can just push to your server :) But really, what's almost more important is that Ruby will get closer to Objective-C's speed.


### Why will MacRuby change the Ruby ecosystem?


As a web developer, getting better performance is great. But Ruby is already fast enough. Rails is faster than any PHP framework out there and when doing Merb's benchmarks we proved that Ruby for the web can definitely be fast.

Now if MacRuby ends up running 3-7X faster than Ruby 1.9 (no one can tell for sure), existing Ruby developers will certainly be really pleased but it will probably affect more people outside of our community than within. Let's face it, our community isn't that big but it's growing fast and people are mainly coming to Ruby for its web frameworks. But Ruby has much more than that to offer. Desktop applications, [video games](http://www.railsenvy.com/2009/5/11/rubystein-ruby-meets-wolfenstein), scientific computation and even embedded apps. Apple is betting on Ruby probably because of the fact that they see the potential in the language itself.

Would people still use Java if Ruby is as fast/faster than Java? Probably! Would they think about using Ruby for their next project? I would certainly hope so!

Ruby is viral, it's such a great language, people who have started using it are having a hard time going back to what they used before. But to spread the 'love', we need to give people the opportunity to discover why Ruby is so great and to do that, we need to make sure Ruby is relevant to them. By making Ruby a realistic option to write desktop/mobile applications, we are targeting a new audience. An experienced audience which will be able to bring a new perspective to our ecosystem and help it grow.

Of course, MacRuby isn't the only implementation out there trying to do that. JRuby and IronRuby are also very interesting projects. My take on it, is that MacRuby will be able to change things because of its new approach and potential community. It will more than likely be the first Ruby implementation compiling down to binary code, it will more than likely be the fastest implementation and it will more than likely draw a different type of developer.

Does it mean that [JRuby](http://jruby.codehaus.org/), [IronRuby](http://www.ironruby.net/), [BlueRuby](http://www.infoq.com/news/2009/04/ruby-on-sap) will be useless? Absolutely not! Matz is the first one to encourage diversity and I agree that this is a great way to move forward. These projects solve different problems and all have pros and cons, but they also all share a similar goal: making Ruby a better and more popular language outside of its original community. JRuby, IronRuby and MacRuby bring Ruby to the respective Java, .net and Cocoa communities, and indirectly bring fresh developers to Ruby. These implementations are critical in the way they actually bridge existing communities and in the end, all Rubyists will benefit from it. Also, even though MacRuby and IronRuby are in active development, JRuby is the only mature alternative to MRI/YARV at this point and it proved that Ruby can coexist in a different community as well as contribute a lot of interesting things back to the Ruby community.

To summarize, I see tremendous potential in MacRuby. There is the obvious technical aspect of the implementation, but also the indirect affect MacRuby could have on our community. I believe that MacRuby is an agent of change and will help bringing more diversity to our community. It will change mentalities and push Ruby to places where it's being struggling to make a mark. I can see the so-called "Enterprise" people looking at Ruby differently. I also think that MacRuby has the potential to be at the origin of a new type of hybrid application, mixing desktop/mobile/distributed applications with centralized web applications. Why not dream of p2p applications/games using a subset of Rails to communicate between each other and with a central server?

[![macruby-site](http://merbist.com/wp-content/uploads/2009/05/macruby-site.jpg)](http://macruby.org)

If you are interested in learning more about MacRuby, check the list of [resources available on the MacRuby site](http://www.macruby.org/documentation.html).

Rich Kilmer and I are also working on a documentation application for Cocoa and [HotCocoa](http://www.macruby.org/hotcocoa.html) as well as a MVC framework for writing Cocoa apps using HotCocoa (HotCocoa is a thin, idiomatic Ruby layer that sits above Cocoa and other frameworks).

Make sure to keep an eye on [@macruby](http://twitter.com/macruby) and the [MacRuby website](http://macruby.org) if you want to keep track of the latest news.
