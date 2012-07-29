---
date: '2012-05-04 07:17:47'
layout: post
legacy_url: http://merbist.com/2012/05/04/macruby-on-ios-rubymotion-review/
slug: macruby-on-ios-rubymotion-review
source: merbist.com
status: publish
title: MacRuby on iOS - RubyMotion review
description: "Overview of the pros and cons of MacRuby on iOS aka RubyMotion"
wordpress_id: '1363'
categories:
- Ruby
- merbist.com
- blog-post
- Popular
tags:
- iOS
- ruby
- RubyMotion
---

Yesterday, [RubyMotion](http://www.rubymotion.com/) was released and let's be honest, it is one the best alternatives to Objective-C out there (if not the best).

RubyMotion is a commercial, proprietary fork of MacRuby that targets iOS. This is not a small achievement, MacRuby relies on Objective C's Garbage Collector (libauto) which is not available on iOS. Static compilation and new memory management solution was required to target the iOS platform . The new runtime had to be small and efficient. Furthermore, being able to run code on iOS isn't enough, you need tools to interact with the compiler, to debug, to packages applications etc...

I don't think anyone will contest the fact that RubyMotion is a well done product. The question however is, "**is it worth for you to invest some money, time and energy in this product instead of using Apple's language and tools**". In this article, I'll try to balance the pros and cons of RubyMotion so you can have a better understanding of what RubyMotion could mean for you. As a disclaimer I should say that I was beta testing RubyMotion, that they are strong ties between RubyMotion and the MacRuby project I'm part of and finally that having MacRuby on iOS has been something I've been looking forward for a very long time.

Over the last few months I've seen RubyMotion take shape and finally hit the big 1.0. As you can see from [Twitter](http://twitter.com/#!/search/rubymotion?q=rubymotion) and [HackerNews](http://news.ycombinator.com/item?id=3924657), the Ruby community is excited about being able to use their language to write statically compiled, native iOS apps. Spoiler alert, they are right, it's a lot of fun.

 



* * *



 


## What I like about RubyMotion:




### Ruby Language


I don't mind Objective-C, I think it's a fine superset of C, with the arrival of blocks, new literals and automatic memory management via ARC, Objective-C is actually getting better over time. But frankly, it's not Ruby. You still have to deal with headers, you always have to compile your code via some weird Xcode voodoo settings, testing is a pain, the language, even with the new literals is quite verbose. On the other hand, using Ruby syntax I can get much more flexibility, reuse my code via mixins, easily reopen existing classes etc... At the end of the day, I end up with some code that seems cleaner, easier to understand and maintain even though I'm calling the same underlying APIs. Ruby's flexibility also allows developers to make their own higher level APIs, take a look at some of the [wrappers/helpers](https://github.com/mattetti/BubbleWrap) I wrote while playing with RubyMotion.


[![Matt Aimonetti - Ruby Logo](http://merbist.com/wp-content/uploads/2012/05/matt_aimonetti-Ruby_logo-150x150.jpg)](http://www.ruby-lang.org/en/)





### MacRuby


RubyMotion is based on MacRuby, meaning that all the time and energy invested in the project will benefit RubyMotion's users. All the concepts I explain in my [MacRuby book](http://shop.oreilly.com/product/0636920000723.do) apply to RubyMotion. You don't have to find workarounds to work with native APIs, Ruby objects are Objective-C objects and performance is great. I do regret Apple didn't decide to embrace MacRuby for iOS but at the same time, even though we lost the Open Source aspect of the project and Apple's backing, we gained much more flexibility and freedom on Laurent's part.


[![](http://merbist.com/wp-content/uploads/matt_aimonetti/matt_aimonetti_macruby_book.gif)](https://www.amazon.com/dp/1449380379?tag=merbist-20&camp=213381&creative=390973&linkCode=as4&creativeASIN=1449380379&adid=1SKHT7ABMG1YJZ3136WQ&)





### REPL/Interactive shell


RubyMotion doesn't currently have a debugger, but it does have something Objective-C developers don't have, a [REPL](http://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) working with the simulator. This feature is quite handy when debugging your application or learning the Cocoa APIs. You can click on a visual element in the simulator and start modifying the objects in real time in a terminal window and see the modifications in the simulator. It reminds me of the first time I used firebug to edit the html/css of a web page and saw the changes in real time.

[![Matt Aimonetti - RubyMotion REPL](http://merbist.com/wp-content/uploads/2012/05/matt_aimonetti-RubyMotion-REPL-150x150.jpg)](http://www.youtube.com/watch?feature=player_embedded&v=rejYKzLglSE#!)


### Not dependent on Xcode


Xcode is fine when you write Objective-C code, but it crashes often, it has a complicated UI and never really worked well for MacRuby due to the fact that Objective-C and Ruby have different requirements and the that Xcode is not open source. It's also fully controlled by Apple and doesn't provide APIs for 3rd party developers. (That said, the Xcode team has often helped out when a new released of Xcode broke MacRuby, so thank you guys).

Being able to use simple rake tasks to compile, simulate and deploy applications is just really really nice. I'm sure we'll end up with better IDE integration, nice GUIs for some who like that, but in the meantime, as a "hacker", I really enjoy the simplicity of the Rake tasks and not being forced in using a specific IDE.

 


### Memory management


Even though ARC made memory management much easier for Objective-C developers, when using RubyMotion you don't have to worry about memory (well at least not explicitly, don't be dumb and create a bazillion objects and hold references to them either). This includes the CoreFoundation objects that you still have to manually manage in Objective-C. Memory management is transparent and in most cases it's really nice.

 



* * *



 


## What I like less about RubyMotion


Here is a list of things that are cons to using RubyMotion, note that while the list is longer than my list of "pros", I listed a lot of small things. I also think that most of these issues will get solved in the next few months.

 


### Ruby language


There are some cases where Ruby just isn't that great or is not an option. Examples include dealing with API relying heavily on pointers, when using some of the lower level APIs or when you have to interact with C++ (video game engines for instance). The good news is that within the same project, you can write part of your code in Objective-C and the rest in RubyMotion. The other thing that bothers me a little bit with writing Ruby code for iOS is that you can't easily enforce argument types and therefore you are losing a lot of the features provided by Clang to the Objective-C developers. I dream of an optionally typed Ruby -- but that's a different topic.

Another downside of using Ruby is that Ruby developers will assume all standard libraries and gems will be compatible with RubyMotion. This isn't the case. You need to think of RubyMotion as only offering the Ruby syntax (modulo a few differences). To be honest, most of the std libs and gems aren't that useful when writing iOS apps. Even when I write MacRuby apps, I rarely rely on them and pick libraries designed to work in a non-blocking, multi-threaded environment (usually ObjC libs that I wrap).

 


### Cocoa Touch


If you're already an iOS/OS X developer, you know that most of the hurdles aren't the language syntax but the Cocoa APIs. These APIs are what you need to interact with to create your application. Cocoa APIs are usually much lower-level compared to what you usually see in Python, Ruby or even Java. While they are quite consistent, the APIs still have a stiff learning curve and currently,  if you want to write iOS applications, even if you know Ruby, you still have to learn Cocoa.

However, I do think that with RubyMotion now building a userbase, we will start seeing more and more [wrappers](https://github.com/mattetti/BubbleWrap) around these sometimes [hideous APIs](https://github.com/HipByte/RubyMotionSamples/blob/master/GestureTable/app/gesture_recognizer.rb).

 


### No Xcode/IDE


There are cases where an IDE is really practical, especially when learning new APIs. Being able to have code completion, quick access to the documentation, instrumentation, debugging, interface builder, refactoring tools are things that Objective-C developers might have a hard time with when switching to RubyMotion. If you don't know either Ruby or Cocoa, getting started with RubyMotion might be quite hard and you are probably not currently in the target audience.

 


### Writing UI code by hand


In some cases, it makes sense, in other, it should be much easier. I know that Laurent is working on a DSL to make that easier and I'm looking forward to it. But in the mean time, this is quite a painful exercise, especially due to the complexity of the Cocoa UI APIs. Using Xcode's interface builder and Storyboards is something I know a lot of us wish we could do with RubyMotion when developing specific types of applications.

[![Matt Aimonetti - Xcode iOS storyboard](http://merbist.com/wp-content/uploads/2012/05/matt_aimonetti_storyboard-1.jpg)](http://kurrytran.blogspot.fr/2011/07/simple-ios-5-tutorial-using-storyboard.html)


### No debugger


Again, this is eventually coming but the current lack of debugger can be problematic at times, especially when the problem isn't obvious.

 


### Lack of clear target audience


It's hard to blame a brand new product for not having clearly defined a target audience. But as a developer I find myself wondering "when should I use RubyMotion and for what kinds of problems?" Is RubyMotion great for quick prototypes I can then turn into production code? Or is good for throw away prototypes? Is it reserved for "fart and flash light" applications? Is it ready for prime time and should I invest and write my new awesome apps using it? Should I convert over my existing code base over from Titanium (or whatever other alternatives you used)? Should I use RubyMotion every time I would use Objective-C?

I guess we will see when the first applications start hitting the app store and people start reporting on their experience.


### Documentation


I'm partially to blame here since I could have moved my butt and start writing a book but the point is nonetheless valid. All the iOS documentation out there is for Objective-C, all the APIs and samples provided by Apple are obviously only for Objective-C. Thankfully, you can use the 2 MacRuby books available out there to understand how to convert this existing documentation into something useful, but RubyMotion will need to provide better and more adapted documentation for beginners. I have no doubt that this is coming sooner than later.

 


### Proprietary solution


RubyMotion isn't open source and currently fully relies on the shoulders of a single man. If unfortunately, Laurent goes out of business or decides to do something else then we will have to rewrite our apps in Objective-C.  Using RubyMotion for a professional product represents a significant business risk, which is exactly the same as using proprietary technology from any vendor. Apple could also decide to switch to JavaScript or rewrite iOS in Java and deprecate Objective-C. Let's just say that it is unlikely.

I usually favor open source solutions, from the programming language I use to the OS I deploy on. This isn't always possible and if you want to write iOS applications, you don't currently have a choice. I do wish Laurent had found a way to make money while keeping the source code open. But who knows -- after he makes his first million(s), he might change his mind.

[![Matt Aimonetti - RMS](http://merbist.com/wp-content/uploads/2012/05/matt_aimonetti-rms-150x150.jpg)](http://merbist.com/wp-content/uploads/2012/05/matt_aimonetti-rms.jpg)


## Conclusion


I would strongly suggest you consider giving RubyMotion a try. I can assure you that it will provide at least a few hours of 'hacking fun' (and you will be able to brag about havng written your own iPhone app).  It will also help support financially someone who's taking a risk in trying to push mobile development to the next level.

RubyMotion is, by far, my favorite alternative to Objective-C. But it is hard to tell, just 48 hours after its release, what people will do with it. Can it transcend the programming language barriers and attract Python, PHP, Java, ObjC and JavaScript developers? What is the sweet spot for RubyMotion applications? Will it affect the native vs web app battle? Can it make iOS development more accessible to the masses? Only time will tell.



<br/>

* * *

### Update:

Since RubyMotion 1.0 was released, I spent quite a lot of my free time leading the
development of [BubbleWrap](http://bubblewrap.io/), a free and open
source 3rd party library for [RubyMotion](http://www.rubymotion.com/).
Unfortunately, as of July 2011 **I took a break from this project and
RubyMotion in general**. I explained my motivations in [this mailing list
post](https://groups.google.com/d/topic/rubymotion/XIE673vnuQk/discussion).
