---
date: '2009-10-05 23:58:53'
layout: post
legacy_url: http://merbist.com/2009/10/05/macruby-soon-to-reach-a-new-milestone/
slug: macruby-soon-to-reach-a-new-milestone
source: merbist.com
status: publish
title: MacRuby soon to reach a new milestone
wordpress_id: '556'
categories:
- macruby
- merbist.com
- blog-post
---

Laurent just posted a [MacRuby status update](http://bit.ly/2YPwRT) on the mailing list and the first official beta of MacRuby 0.5 should be released pretty soon.

Let's quickly look at Laurent's report:



	
  * Early backtracing support.

	
  * Much better AOT compilation. Parts of the standard library are now pre-compiled for testing.

	
  * Migrated to LLVM top of tree.

	
  * Dispatcher performance is now back to normal (we lost about 30% due to gcc not inlining code).

	
  * Many bug fixes.


In lay terms, backtracing is what you see when your app crashes or has a problem, it's the list of methods called before the exception was raised and the line where the error happened. Currently the backtrace is similar to what you would have with Ruby 1.9, however objective-c exceptions are not supported and there is still some work to do.

AOT compilation or Ahead Of Time compilation is the process of compiling a script into machine code. I already covered that [feature earlier](http://merbist.com/2009/07/12/compiled-hello-world-with-macruby/). Progress has been made and now some Ruby standard libraries are now pre-compiled in MacRuby. The two main advantages of doing AOT compilation are startup speed and obfuscation. Two important features for desktop applications or for when you want to license your server app.

Updating LLVM doesn't mean much for end users. With the support of the MacRuby team, [Claudio](http://www.icoretech.org/) setup a nightly build bot [http://macruby.icoretech.org/](http://macruby.icoretech.org/) allowing you to download nightly builds for SnowLeopard. The app is a [Sinatra app](http://sinatrarb.com) that could can checkout on github:  [http://github.com/masterkain/macruby-nightlies-web](http://github.com/masterkain/macruby-nightlies-web) What's interesting is that Apple is sponsoring both LLVM and MacRuby which will hopefully bring some synergy and help both projects.

Fixed dispatcher, this is a just a perf bug fix only affecting people on trunk.

I personally used MacRuby quite a lot recently. I have been writing a 2D video game for my [RubyConf talk](http://rubyconf.org/talks/153-writing-2-d-games-for-the-osx-platform-in-ruby).

![](http://img.skitch.com/20091006-kj64ix4up5q8dh4yin38hjrjcp.jpg)It's a simple game only using Ruby and Cocoa. It's not a fancy game and, no, it doesn't run on the iPhone yet ;)

I never wrote a game in Cocoa and I decided not to use an existing framework like Gosu or cocos2d, instead I decided to write everything from scratch. Using CoreAnimation instead of OpenGL, the task wasn't that hard at all. in less than 1,000 LOC (before refactoring), I have a fully working game.

I will be previewing the game at [RailsSummit](http://www.railssummit.com.br/) and will show the code and explain how to get there at [RubyConf](http://rubyconf.org/talks/153-writing-2-d-games-for-the-osx-platform-in-ruby).

When I started getting involved with MacRuby, I really did not think I would write a video game in Ruby and actually enjoy it. At the end of the day, I will more than likely use MacRuby for desktop/mobile/server apps more than games, but it's awesome to be able to use your favorite language to do other things than what you are usually paid to do.

MacRuby is coming along very nicely, and I'm really excited about the multitude of new options offered to Ruby developers on OSX and can't wait for 0.5 final and see what people will do with it.
