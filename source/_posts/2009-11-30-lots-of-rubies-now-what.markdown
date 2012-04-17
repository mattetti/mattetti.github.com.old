---
date: '2009-11-30 13:38:53'
layout: post
legacy_url: http://merbist.com/2009/11/30/lots-of-rubies-now-what/
slug: lots-of-rubies-now-what
source: merbist.com
status: publish
title: Lots of rubies, now what?
wordpress_id: '648'
categories:
- ruby
- merbist.com
- blog-post
---

If you were at RubyConf 2009 or looked at the schedule, you saw that the big thing happening in the Ruby scene is the maturation of a many of the Ruby implementations:
BlueRuby, HotRuby, IronRuby, JRuby, MacRuby, Maglev, MRI, REE, Rubinius, SmallRuby...
Ruby developers really have plenty of choice when it comes to choosing an alternative Ruby implementation.

Is that a bad thing?

Turns out, Matz , Ruby author mentioned one ore time during RubyConf that it was actually a good thing for the Ruby ecosystem.

But maybe we should take a step back and look at the big picture. So here are some of my thoughts on the topic. _(disclaimer: being part of the MacRuby team, I'm certainly biased)_

**Stop comparing alternative Ruby implementations against Ruby 1.8.x.**

Even though a lot of people are still using Ruby 1.8.x, it's totally unfair to compare against such an old version of Matz's Ruby.
Ruby 1.9 is what people should compare against and believe me, Ruby 1.9.2 is going to change the game (much faster, cleaned up version).
Hopefully, all the Ruby alternative implementations will quickly become 1.9 compatible.

(On a site note, [Rails 2.3.5 was released](http://weblog.rubyonrails.org/2009/11/30/ruby-on-rails-2-3-5-released) a few days ago which fixes some of the last few remaining Ruby 1.9 bugs)

**Don't trust the benchmarks**

As we all know, [there are lies, damn lies and statistics](http://en.wikipedia.org/wiki/Lies,_damned_lies,_and_statistics). Evan Phoenix explained that very clearly with lots of examples during his [RubyConf talk](http://rubyconf2009.confreaks.com/) and I totally agree with him.
Micro benchmarks only help implementers know where they are standing and find performance issue.
Let's take a very simple example: [JRuby](http://jruby.org/).
What you might not know is that most JRuby benchmarks are usually 'warmed up'.  Basically, to get the results you see in most reported benchmarks, you need to run the benchmark in a loop X amount of times and then run the benchmark. The reason for that is that JRuby uses HotSpot which needs to process the code to optimize the JITting and finally get your code to run faster.
The problem is that it is not a fair comparison. I was benchmarking Rails3 for instance and JRuby starts way slower than Ruby 1.8.6 and takes a little while to get as fast/faster than 1.9.1.
This is something you need to take in consideration since each deployment will slow down your app and you probably shouldn't use JRuby if you are not using long running processes.
Another example would be MacRuby, even though the MacRuby project never released any benchmarks, it's known to be a _fast_ implementation.
Currently, the allocation of objects in MacRuby is actually still a bit costly and if you benchmark that, you can _show_ that MacRuby is slow. Other operations are super optimized and both Rubinius and MacRuby will be way faster than anyone else. (check Phoenix's talk at Ruby conf for more examples)
So, before choosing an implementation for performance reasons, benchmark against the code you are going to use.

**There is only one real original Ruby**

I think nobody will disagree with the fact that there is only one Ruby reference and that's Matz.
Therefore is only one "real" Ruby and that is Matz's. (Matz actually disagreed with me when I said that, since all the other implementations are also "real" but you get the point)
There is nothing wrong with Ruby's alternatives but they stay alternatives to the original
MRI might not be the best option for every single case out there but it is and will stay the reference.

During RubyConf, I was joking with Matz and Koichi that maybe they should claim a Ruby tax to the Ruby implementations.
The reality is that Ruby is maintained by 5 to 10 contributors, most of them doing that on the side of their full time job.
Most of the other implementations have full time staff working hard on their implementations. That's exciting but sad at the same time.

During the different talks given by the Ruby core team, it was clear that they really want to bridge the West/East gap and asked for help. It's very honorable on their part and I hope we will see more contributions coming from outside Japan.

Finally, I think that even if people work hard to write the best Ruby implementation ever, let's not forget to respect Matz's project, team and achievements. People spent hours/weeks/months creating the Ruby language we love and even though there are some aspects that could be and are being improved, let's not forget that when criticizing/comparing their work.

**There is not only one way to do things**

I don't know where that's coming from, but some people in our community seem to believe that there is only one way to do things and if you don't do it their way, you do it wrong.
I personally have a hard time believing that and I even think it goes against what Ruby was designed for. (However, I must admit that sometimes I did/do think that some approaches were totally wrong :p)
It reminds me of people trying to convince you that God exists, other people trying to convince you that their religion is the only true one, or finally the other people trying to convince believers that they are wrong.
Maybe it is just that as humans we have a hard time dealing with people coming to a different conclusion than us and therefore we have to convince them that we are right and they are wrong.

The way I see it, for any new Ruby project, you should start be using with Ruby1.9. If it doesn't fit your needs, then, consider an alternative. Here is how I look at the other alternatives (feel free to disagree):



	
  * [BlueRuby](https://wiki.sdn.sap.com/wiki/display/Research/BlueRuby) - use if you are writing a SAP app

	
  * [HotRuby](http://hotruby.yukoba.jp/) - use if you want to use Ruby to write a Flash app

	
  * [IronRuby](http://ironruby.net/) - use it if you want to integrate with .NET and/or write a silverlight app

	
  * [JRuby](http://jruby.org/) - use if you want to integrate with the Java world

	
  * [MacRuby](http://macruby.org) - use it if you want to integrate with Cocoa/want a native OSX application

	
  * [Maglev](http://maglev.gemstone.com/) - use if you want to use an object persistence layer and a distributed shared cache

	
  * [REE](http://www.rubyenterpriseedition.com/) - use if you have a ruby 1.8 web app that uses too much memory

	
  * [Rubinius](http://rubini.us) - use if you want to know what's going on in your code

	
  * [SmallRuby](http://smalltalk.felk.cvut.cz/projects/smallruby) - don't use it yet, but keep an eye on it


This is the way I see the various existing implementations and I think this is where they shine and why they were created in the first place. They are all complimentary and help bring the Ruby language further. The goal is always the same.

**The future is exciting**

Ruby is getting everywhere or almost. Matz's Ruby is going to get multiVM support, various other improvements and get even faster and faster.
Alternative implementations are getting more and more mature and they grow the community by bringing people from different communities (.NET, java, obj-c/cocoa)

At the end of the day, we are all beneficiaries of the work done by all the implementers, thank you guys and keep up the good work!
