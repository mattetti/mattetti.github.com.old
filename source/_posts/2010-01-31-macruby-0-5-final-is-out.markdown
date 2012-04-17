---
date: '2010-01-31 19:13:36'
layout: post
legacy_url: http://merbist.com/2010/01/31/macruby-0-5-final-is-out/
slug: macruby-0-5-final-is-out
source: merbist.com
status: publish
title: MacRuby 0.5 final is out
wordpress_id: '709'
categories:
- macruby
- merbist.com
- blog-post
---

After going through two betas, MacRuby 0.5 final is now released and can be downloaded by clicking on the icon below:

[![](http://macruby.org/images/zip.png)
MacRuby 0.5](http://macruby.org/downloads.html)

Don't worry about having MacRuby and Ruby 1.8.x or 1.9 installed, MacRuby is namespaced and won't affect your current Ruby installations, just download and launch the installer. (Note: The build was compiled for SnowLeopard only)

You can read all the [details of the release on the MacRuby website](http://www.macruby.org/blog/2010/01/31/macruby05.html).

So what changed since 0.4? Too many things for me to list them here but basically 0.5 uses LLVM to compile code and make MacRuby faster and integrate better with the Obj-c runtime. However since the last beta, here is what changed:



	
  * HotCocoa is now a separate gem

	
  * improved AOT compilation

	
  * Grand Central Dispatch support - use all your cores without the pain of threads. Read [this post](http://www.macruby.org/documentation/gcd.html) for more info.


0.5 is a solid release which I consider production ready, I personally wrote a few of small Cocoa apps in MacRuby and everything has been working very well. Of course, I'm also excited about the new stuff in 0.6 trunk like the debugger previewed a few weeks ago: [http://merbist.com/2010/01/18/how-to-detect-cylons-with-macruby/](http://merbist.com/2010/01/18/how-to-detect-cylons-with-macruby/) but also some drastic changes in the primitive classes that I might cover later on.

Finally, people are asking if the iPad will be able to run apps running in MacRuby. Unfortunately, the current answer is: no. The two issues with the IPhone/iP*d OS are the lack of Garbage Collector and support for BridgeSupport (needed to define CocoaTouch constants available from MacRuby). However, this matter is being discussed on the mailing list and progress is made by contributors (the core team primarily focusing on the desktop).

That is going to be an exciting Ruby week as MacRuby 0.5 is now out and Rails 3 beta/RC0 is expected really soon.
