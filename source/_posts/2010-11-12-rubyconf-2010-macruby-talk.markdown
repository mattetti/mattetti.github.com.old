---
date: '2010-11-12 16:00:51'
layout: post
slug: rubyconf-2010-macruby-talk
source: merbist.com
status: publish
title: "RubyConf 2010 - MacRuby: Why and How"
wordpress_id: '832'
author: Matt Aimonetti
categories:
- Presentation
- merbist.com
- blog-post
slides: http://www.slideshare.net/mattetti/macruby-rubyconf-presentation-2010
---

##Description of the talk:

This year I gave the traditional Apple's MacRuby talk at RubyConf.
My presentation focused on 2 axis:

  * What's new since last RubyConf
  * Show some examples of how fun it is to hack with MacRuby

##Video

{% video http://cdn.confreaks.com/system/assets/datas/768/original/448-rubyconf2010-macruby-why-and-how-small.mp4 640 360 /images/matt_aimonetti_rubyconf2010.jpeg %}

Other video formats are available [here](http://www.confreaks.com/videos/448-rubyconf2010-macruby-why-and-how)

##Slides

<script async class="speakerdeck-embed" data-id="4f90697149bc25001f023143" data-ratio="1.299492385786802" src="http://speakerdeck.com/assets/embed.js"></script>

**[Presentation slides available on Speakerdeck](http://speakerdeck.com/u/matt_aimonetti/p/rubyconf-2010-macruby-why-and-how)**

##Details of the talk content

MacRuby is currently at version 0.7.1 and version 0.8 is in preparation.
Since last new a lot of things happened, here is a quick summary:
	
  * Cocoa dev is now considered stable.  Apple gave its seal of approval, most of the bugs are fixed and it's currently used on production projects and some apps were even submitted to the Mac App Store.
	
  * Ahead of Time compilation. This is quite a major improvement with many repercussions. Being to compile your Ruby source code means faster boot time and source obfuscation. Two major things to consider when you want to ship a desktop app.
	
  * Debugger. MacRuby now ships with its own debugger. Of course you can still use GDB and DTrace, but MacRuby's debugger is really easy to use and very powerful.
	
  * Grand Central Dispatch support and wrapper API. Having to manage threads can be hard for both developers and for the machines having to run the developer code. GCD is an abstraction layer allowing developers to only focus on business logic without having to worry about the underlying details if you don't want to. The end result is an optimum use of all the cores available on a machine and truly concurrent code. Two important notions when writing desktop apps.
	
  * ControlTower - A webserver for Rack apps written in MacRuby and using GCD for concurrency.
	
  * New rewritten and more efficient dispatcher. Practically that means that the dispatcher is now thread safe with a per thread cache.
	
  * RegExp lib switch from Oniguruma to ICU. This was quite a big change and it was required because Oniguruma isn't thread safe and while C Ruby uses a Global Interpreter Lock. MacRuby on the other hand uses native POSIX threads running on their own non-locking, reentrant VM making thread safety critical.
	
  * More solid foundations - Some key classes we rewritten to improve performance and flexibility.
	
  * Support for C blocks - Objective-C has been supporting C blocks for a while and some Cocoa APIs require the use of blocks. MacRuby now allows you to use Ruby blocks and to pass them as C blocks. (new BridgeSupport required)
	
  * Sandboxing - For safety reasons, MacRuby allows you to now sandbox your applications. You can restrict your app from doing some potential dangerous things such as writing to disk, calling the system, accessing internet etc...

	
  * Mac App Store - This is not something done by the MacRuby team. But it directly affects MacRuby developers wanting to distribute their applications. Think about the exposure that you can app can have. Even if you have a web app, you can use WebKit to wrap it up, hook up a notification, add to that a local backup solution and geo location. Brilliant way to provide an even better user experience and better product exposure. All that really easily if you already know Ruby.


To give an idea of what can be done I showed some code samples hopefully showing the cool hacking things one can play with:

	
  * The first demo shows how to use the speech recognizer. The example is really sample you speak the name of someone and his/her picture shows up on screen. Just a few lines of code and you can start screaming commands to your TV to get it to change channel.
	
  * The second demo is an extremely simple Gowala client using CoreLocation. Yes, Mac desktop and laptops support geo location.
	
  * Another example of what you do is a sample letting you import all your twitter followers to your address book.
	
  * You can also use some of the low level OS features such as the Tokenizer. The next example showed how to extend Ruby and use a C function to detect the language of a string.
	
  * Finally, the last demo shows how to integrate a bluetooth device to control a HTML view via MacRuby and Javascript. All that in just a few lines of code!


Basically, MacRuby is now mature and it's time for hackers and people trying to get exposure to give it a try.

##Presentation's website

A page was setup on the confreaks website and available [here](http://www.confreaks.com/videos/448-rubyconf2010-macruby-why-and-how).
