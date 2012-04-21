---
layout: post
title: "mruby and MobiRuby"
date: 2012-04-20 12:32
comments: true
categories: 
- Ruby
- mruby
- MobiRuby
- MacRuby
- Objective-C
---

Today, two big Ruby news came directly from Japan:

* The Open Source release of [Matz'](http://en.wikipedia.org/wiki/Yukihiro_Matsumoto) [mruby on GitHub](https://github.com/mruby/mruby).
* The announce of [MobiRuby](http://mobiruby.org/), an upcoming solution
  to develop iOS and Android applications using Ruby.

Probably due to my involvement with the [MacRuby](http://macruby.org/)
project, people have been asking me what I thought of these news.

## mruby

mruby is far from being a new project. It's based on the RiteVM which is a
sponsored project by the [Japanese ministry of Economy, Trade and Industry](http://www.meti.go.jp/english/) and lead by Ruby's creator: [Yukihiro "Matz" Matsumoto](http://en.wikipedia.org/wiki/Yukihiro_Matsumoto) and was explained in details during [Matz's RubyConf 2010 keynote](http://www.slideshare.net/yukihiro_matz/rubyconf-2010-keynote-by-matz).


Back in November 2011 Matz also explained mruby. His talk was recorded
and he explains very well the current Ruby ecosystem and why mruby makes
sense.
<div class="video-container">
<iframe width="560" height="315" src="http://www.youtube.com/embed/sB-IifjyeLI" frameborder="0" allowfullscreen></iframe></div>

As explained, the main goal of mruby is to have a Ruby version that can
be embedded and therefore have a smaller footprint, be compiled and
linked within another application.

Hiroshi Nakamura gave a great 1 line definition of mruby:

![mruby](/images/mruby_def.jpg)


mruby targets game developers (to use instead of Lua), embedded
application developers (devices, TV, phones..) and small memory
footprint server applications (instead of JS for instance).

I'm personally quite excited by mruby, it's not there yet and there is
still a lot of work to do to prove the value of the project but it's
certainly a great step in the right direction. What's also really nice
is that the project is released under an OSS license allowing for all of
us to contribute and companies to improve the implementation based on
their own needs.

**Summary:** mruby is a promising project even if it is still in its infancy. 
Besides being yet another Ruby implementation, the fact that the
target audience and the project scope are well defined and that the project is lead
by Ruby's author and sponsored by the Japanese government makes me want to believe that it can be a successful project. That said Lua is a simpler language and it is already well implemented in the targeted market, so hopefuly Matz, his team and the Japanese government have a plan to advocate and champion this new technology. Good luck to them and I'll keep an attentive eye on the project.

## MobiRuby

[MobiRuby](http://mobiruby.org/) is being developed by [Yuichiro MASUI](https://github.com/masuidrive) who works for [Appcelerator](http://www.appcelerator.com/) the company behind the popular [Titanium platform](http://www.appcelerator.com/platform/titanium-sdk) to write native iOS, Android apps in JS.

MobiRuby is built on top of mruby making it the first demonstration of
what motivated developers can do with Matz new implementation. Very much
like mruby, MobiRuby will be released under an OSS license but unlike
mruby, the [Apache license](http://www.apache.org/licenses/LICENSE-2.0.html) was chosen.

So far this was just an announcement with a code sample and a
screenshot. That was enough to make the front page of [HackerNews](http://news.ycombinator.com/item?id=3866418). Apparently the author is planning on releasing a first version in a few months.


 ![MobiRuby](http://mobiruby.org/screenshot1.jpg "MobiRuby screenshot")


It might surprise some, but I'm quite glad to see this kind of projects
even though, they compete to some extent against MacRuby. It proves two
things: 

* there is a strong interest in having Ruby on mobile devices.
* it's technically possible to do so.

Now, this is not something new either, Lua developers have been able to
write iOS apps for a while, yet the majority of the iOS developers still
use Objective-C. What are the challenges facing implementations trying
to replace Objective-C?

### The replacement language might not fit the Cocoa design.

Developing an iOS/OS X app means that you spend your time using provided
libraries (called frameworks in Apple's jargon). These frameworks have
specific patterns, a well defined syntax and usually work in a very
consistent/constraining way. Or your language is quite similar (like Ruby) and the
transition is easy, or you need to start writing and maintaining
wrappers (titanium).

### Bridged runtimes.

Having 2 runtimes running at the same time is quite challenging and not
efficient. That's one of the reasons why Apple pushed MacRuby to move from [RubyCocoa](http://en.wikipedia.org/wiki/RubyCocoa) being a bridge and to have a Ruby implementation running in Objective-C runtime itself.
This allows something else, in MacRuby all objects are actually
Objective-C objects which means you don't need to convert anything and
Cocoa APIs can be extended from Ruby code by just reopening them.

### Support.

This one is critical for many. Often, you don't want to have your next big
project rely on a technology that doesn't have a good backing and
support. What happens if you build your app using an alternate
implementation and all a sudden the developer(s) get bored and move on,
or take another job?
What about the updates needed as Apple/Google update their platforms?
It might not be the best reason to not choose an alternative, but it's
a reasonable reason especially for companies who want to be "safe".

### Cocoa

Cocoa APIs represent probably 90% of the challenge when writing iOS/OS X
applications. The APIs, while powerful and efficient, are often a pain
to get used to and to learn.

You have the challenge of the documentation and the examples that
are only in Objective-C, requiring that someone [writes a book](http://www.amazon.com/gp/product/1449380379/ref=as_li_ss_tl?ie=UTF8&tag=merbist-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=1449380379) and/or that
you convert and maintain an enormous amount of documentation.

You also have all the tools provided by Apple which, you often can't
fully use because you aren't using their toolchain.

To be honest, after so many years using MacRuby, I think that the real
value of such a project isn't in the easier syntax but instead in the
fact that you can easily build wrappers and higher level interfaces
around repetitive tasks. Having a mix of a well designed DSL and yet
access to the native object is something extremely powerful.

### Objective-C is evolving.

Objective-C is evolving, with the introduction of [ARC](http://developer.apple.com/library/ios/#releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html), memory management became much easier. The latest version of clang also granted Objective-C with a nicer syntax thanks to new literals and object subscripting ([read more](http://clang.llvm.org/docs/ObjectiveCLiterals.html)). As a matter of fact, Objective-C syntax is getting closer and closer to Ruby's. Making the choice to use an alternate harder and harder to make.

``` objective-c
// character literals.
NSNumber *theLetterZ = @'Z';          // equivalent to [NSNumber numberWithChar:'Z']

// integral literals.
NSNumber *fortyTwo = @42;             // equivalent to [NSNumber numberWithInt:42]

// floating point literals.
NSNumber *piDouble = @3.1415926535;   // equivalent to [NSNumber numberWithDouble:3.1415926535]

// BOOL literals.
NSNumber *yesNumber = @YES;           // equivalent to [NSNumber numberWithBool:YES]

// Container literals
NSArray *array = @[ @"Hello", NSApp, [NSNumber numberWithInt:42] ];
id value = array[idx];

NSDictionary *dictionary = @{
  @"name" : NSUserName(),
  @"date" : [NSDate date],
  @"processInfo" : [NSProcessInfo processInfo]
};
id oldObject = dictionary[key];
dictionary[key] = newObject;	// replace oldObject with newObject
```

### Performance.

Even though devices are more and more powerful, performance is often
critical and Apple optimized the performance of their solution for their
language. If you have ever developed a Titanium app, you know that it
can be an issue and you might have to find workarounds to get decent
performance.


**Summary:** Based on all these things, once MobiRuby will be released, I will be
able to make a better judgement. But based on what I have seen so far,
I'm quite concerned by the syntax and the performance we will get out of
the box. But time will tell and things can always be improved.
Ruby on iOS/Android is something exciting and I'm looking forward to
testing the first betas.


