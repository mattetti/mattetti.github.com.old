---
date: '2008-11-14 00:39:27'
layout: post
slug: merb-10-tips-part-1
status: publish
title: merb 1.0 - tips part 1
wordpress_id: '228'
categories:
- Misc
- News
tags:
- faq
- merb
- rubykaigi
---

Â 

As you must know by now, last week,[ Merb 1.0 got released](http://merbist.com/2008/11/09/merb-1-0-released).

[![](http://merbist.com/wp-content/uploads/2008/11/nsa_logo.gif)](http://merbist.com/wp-content/uploads/2008/11/nsa_logo.gif)

Because we wanted to assure the release was fine, we asked [Engine Yard](http://engineyard.com), [Apple](http://apple.com) and [YellowPages.com](http://yellowpages.com) to help us by hiring the NSA to monitor Merb users. After all, we need to know what people complain about so we can fix it ASAP.

Turns out all the [echelon monitoring sites](http://en.wikipedia.org/wiki/ECHELON) were already busy. So we had to go back to good old IRC and [twitter search](http://search.twitter.com/search?q=merb) ;)

The release looks pretty solid, thanks to all the people reported bugs during the release candidate cycle. Thanks guys!

However there are still few points of confusion I'd like to address.




## How do I require/request/load a plugin?


I saw this question few times on IRC and someone after my [ORUG](http://orug.org) merb presentation tonight asked me exactly the same thing. I promised [Geoffrey Grosenbach](http://peepcode.com/) that I would update the RDoc but in the meantime here is quick explanation.

You should almost never use the require method to load a gem. Rubygems has a method called gem that takes a version as an argument:

` require "rubygems"
gem "merb-core", "~> 1.0"
`

Using gem you can specify a version of the gem you want to load. Therefore you avoid loading an unexpected version of a gem. A few tricks to know (for regular Ruby code; keep reading for the Merb technique):

You can use "=1.0" to limit to version 1.0, or ">= 1.0" to load 1.0 and anything newer. My favorite trick is to use "~> 1.0" which means load latest version between 1.0 and 2.0

However when using Merb you don't want to load gems just whenever. Merb's bootup process starts by setting up the directory structure, and you typically want to load plugins after this point. To make this transparent to you, we have the #dependency method. The dependency method uses the gem method internally to load specific versions at the right time. Additionally, we may use it in the future to improve Merb's built-in bundling or dependency resolution. As a result, you should always use the dependency method, and not try to use require directly for Merb plugins.

It also comes with two tricks you should know about. Let's say you want to require a gem from github:

` dependency "mattetti-awesomeâ€, "~>1.0", :require_as => â€œawesomeâ€
`

The example above is the same as:
`
gem "mattetti-awesome", "~1.0"
require "awesome"
`
Apart that it gets loaded when it's appropriate.

You can also pass a block after your dependency call:

` dependency('jchris-couchrest', ">= 0.5", :require_as => "couchrest") do
Â Â require "lib/custom_hack.rb"
endÂ `

The block will be executed after the gem is loaded and required. Use a block if you want to perform some action after the dependency is loaded.

Finally the last trick lets you load a gem right when you declare the dependency:

` dependency "nokogiriâ€, "~>0.5", :immediate => true`


## I have issues with merb -i and can't run the specs


Few things here, first off, Merb forces you to use rubygems 1.3 because 1.2 is very buggy and the Rubygems maintainer suggested we do so. Rubygems let you define development-only dependencies, dependencies you don't need to have to run your library in production. These dependencies were designed to be used when developing a library, but in the case of Merb, they are used to define dependencies that you only need at development-time, but not in production.

In our case we defined webrat as a development dependency. That means you need Webrat to run your specs, but won't need it in production. Now here is something you should know, whenever you install a gem to develop it, you should use:

`gem install "merb-core" --development
`

The --development flag will install all the dev dependencies declared by the gem.  Now merb -i loads the code needed to make the request object available in the console, the problem is that this code also loads some test helpers which require webrat. We have a ticket open for that and will get to that soon.

In the mean time, make sure you have webrat and nokogiri installed. (Nokogiri requires you have a somehow recent version of libxml2, which comes with OSX, but not all Linux distributions).

People reported some issues updating rubygems, Robby Colvin wrote a [nice article](http://robbycolvin.com/archives/merbrubygems-upgrade-errors/) explaining how to get pass this issue.


## Issues with action-args not loading


Some people reported issues with loading action-args. Turns out it's some sort of weird issue with hoe (the library) and a some of action-args deps. If that happens to you, update hoe:`
`

`$ sudo gem install hoe
`


## I generated an app and it doesn't work with JRuby


If you generate a merb using merb-gen app my-app-name it generates a merb stack app. It's an opinionated bundle with everything you might need including jQuery, DataMapper and a bunch of plugins. The problem is that DataMapper doesn't run on JRuby yet so it won't work.

Instead you should generate a core application:

`merb-gen core app-name`

This way you create a blank app with no dependencies. Then add what you need. [Here is a list of dependencies](http://wiki.merbivore.com/faqs/merb_components) for a merb stack app and what they do.


## Thanks


Finally I would like to thank the Ruby community from Japan. First because many people came to RubyConf all the way from Japan. I'm personally sorry I couldn't meet you all. Thankfully some people started a new chat channel on lingr: [merb.jp](http://www.lingr.com/room/merb.jp) and I could meet more Japanese Merbists. Because the Merb team thinks is important that Merb is being used outside of the English speaking world and because we need feedback from other countries, I decided to make the trip to RubyKaigi 2009. Hopefully we will be able to have some talks about Merb :)Â  I would also want to thank Erubis author, Makoto Kuwata for translating the[ Merb 1.0 annoucement post](http://merbist.com/2008/11/09/merb-1-0-released) in Japanese.


## Finally


Here are the slides from my talk at [Orug](http://orug.org), a quick presentation of Merb. I'll be at [QCon](http://qconsf.com/sf2008/conference/) next week to talk about [Ruby for the Enterprise](http://qconsf.com/sf2008/tracks/show_track.jsp?trackOID=172). Thanks [Gregg](http://railsenvy.com/) for thinking of me!


[Merb presentation at ORUG](http://www.slideshare.net/mattetti/merb-presentation-at-orug-presentation?type=powerpoint)Â 


View SlideShare [presentation](http://www.slideshare.net/mattetti/merb-presentation-at-orug-presentation?type=powerpoint)



