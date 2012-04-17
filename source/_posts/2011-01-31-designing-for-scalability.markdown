---
date: '2011-01-31 13:30:55'
layout: post
legacy_url: http://merbist.com/2011/01/31/designing-for-scalability/
slug: designing-for-scalability
source: merbist.com
status: publish
title: Designing for scalability
wordpress_id: '912'
categories:
- Software Design
- merbist.com
- blog-post
tags:
- architecture
- design
- scalability
---

Designing beautiful and scalable software is hard. Really hard.

It's hard for many reasons. But what makes it even harder is that software scalability is a relatively new challenge, something only really done in big companies, companies that are not really keen on sharing their knowledge. The amount of academic work done on software design is quite limited compared to other types of design, but shared knowledge about scalable design is almost nonexistent _(Don't expect to find detailed information about scaling online video games either, the industry is super secretive. And even if this is a niche market where finding skilled/experienced developers is really challenging, information is not shared outside a game project)._

I don't pretend to have the required knowledge to cover this topic at length. However, I do have some exposure and figured I should share what I learned so others can benefit from my experience and push the discussion further.

Designing scalable software is just like any other type of software design, with a few unique constraints. If I had to define the key requirements of a great design I would have to quote Frederick P. Brooks:


> "Great designs have conceptual integrity - unity, economy, clarity"


This is true for any type of design and one should always start by that.
Don't just jump on your keyboard and start writing tests/code right away. Take a minute to think about your design.
That will save you hours of refactoring and headaches.


## You're a designer and might not even know it


You might not be designing the next NASA engine but you are more than likely designing an API that you and others will use. As a matter of fact, unless you write code that will never be seen again, you are writing an Application Programming Interface (API). Every single class, method, function you write is an API that you and others will use. Remember that every time you write code, you are the implementer of a design, and therefore you are a designer.

[caption id="attachment_916" align="alignright" width="150" caption="Giana and I, discussing design patterns"][![Giana and Matt Aimonetti](http://merbist.com/wp-content/uploads/2011/01/giana_and_moi_lowres-150x150.jpg)](http://merbist.com/2011/01/31/designing-for-scalability/giana_and_moi_lowres/)[/caption]

When thinking about your design, focus on design concepts instead of implementation details. A design concept must be clear, simple to both explain with words and draw on a whiteboard. If you can't draw and explain your design on a whiteboard, you have failed one of the great design requirement: clarity. If you work alone, or your coworkers are tired of hearing you, try rubber ducking your design ideas. It's the same concept as rubber ducking debugging, where a programmer would force himself to explain his code, line-by-line, to a rubber duck on his desk but instead of talking about the code, explain your design and why it's awesome (I've recently done this with my baby girl and it's been really helpful).


## Keeping the design integrity


One of the challenges of designing scalable software is that your constraints are often very unique to your product. Off the shelf solutions don't work for you, and the specific solution used by another project can't be transposed to your project because the cause and the effect of what you need to scale are different. The problem is that you really quickly lose design integrity.

Let's take a look at a concrete example to see how the design integrity can be lost or even not defined at all.
Let's pretend we want to write a suite of web APIs for video games.

We can look at this task from different perspectives:[![the shout posted by Matt Aimonetti](http://merbist.com/wp-content/uploads/2011/01/the-shout1-150x150.jpg)](http://merbist.com/2011/01/31/designing-for-scalability/the-shout-2/)



	
  * Video game deadlines are crazy, let's find a way to release as many APIs ASAP.

	
  * We're going to get a huge amount of traffic, let's make sure we don't crash and burn.

	
  * We need to make sure our APIs are simple to use for the dev teams integrating them.


Each of these perspectives reflects a facet of the challenge. Other facets exist that I didn't mention but that a business person might have listed right away, one of which being: How can we do that for the least amount of money?

To design our API suite, we first need to understand the different perspectives. Gaining this understanding will help us design something better but it will also help us communicate better with the different stakeholders. Once we have a decent understanding of the constraints and expectations, someone needs to explicitly define the design values and their priorities. This is a crucial step in the design process. Systems nowadays are too complicated to be handled by only one person and keeping design integrity requires clear communication.


## Design goal and values


The best way to communicate the design is to write a simple sentence defining the primary goal:
"Build a robust, efficient and flexible middleware solution leveraged by external teams to develop online video game features."

This is a bit like the mission statement of your project, or the elevator pitch you give someone that asks you what you are working on.

Associated with the primary goal are a host of desiderata, or secondary objectives. These are the key objectives used to weigh technical decisions. It's important for the design to highlight a scale of values so one can refer to them to decide if his/her idea fits the design or not. Here is an example:



	
  1. Stability

	
  2. Performance / Scalability

	
  3. Encapsulation / Modularity

	
  4. Conventions

	
  5. Documentation

	
  6. Reusability / Maintainability


Often these desiderata are applied to most of your projects and reflect your team/company's technical values. The list might seem simple and unnecessary but, believe me, it will reduce the arguments where John tells Jane that her idea sucks but his is better because he "knows better". Having an objective reference to refer to when trying to decide which is the best way to go is greatly valuable and will reduce the amount of office drama.


## Constraints


Finally, make sure to explicitly define all the major constraints and to acknowledge the team's concerns. Here is a small example of what could be listed (which also reflect the previously mentioned perspectives):



	
  * hard deadlines

	
  * external teams involved

	
  * huge load expected

	
  * limited support available

	
  * requirements changing quickly

	
  * limited budget

	
  * unknown hosting architecture/constraints

	
  * ...


Remember that design is always iterative because the constraints keep changing. That's just the way it is and a lot of technical constraints only appear as you implement or test your design. That's also why the design needs to be clear but the implementation needs to be flexible.


## Reads vs writes


Most of the web apps out there are read heavy, meaning that the stored data gets more accessed than modified. Scaling these type of systems is easier as one can introduce a cache layer, an intermediary storage, which acts as a fast buffer that avoids putting load on the backends. The cost reduction is huge because if you architected your app properly, the data is read from the data store only once (or once every X minutes) after being created/modified.

Caching is so important that it's even built into the HTTP protocol, making caching trivial.
Speaking of HTTP, a common problem I often see when serving http content to a browser is that even though the backend calls are the same, some information needs to be customized for the current visitor. This prevents it from caching the entire page. An easy solution in this case is to still cache the entire page but to use javascript to fetch the custom data from the backend and to modify the cached http at the client's browser level directly. As part of your design, you will more than likely need to implement multiple layers of caching and use technologies such as query caching, Varnish, Squid, Memcached, memoization, etc...

The problem is that, as your system gets more traffic, you will notice that the volume of DB/network writes becomes your bottleneck. You will also notice a reduction of your cache/hit ratio because only a small part of your cached data is often retrieved by many clients. At this point, you will need to denormalize to avoid contention, shard your data in silos, or write to cache and flush from cache when the data store is available and not overwhelmed.


## Asynchronous processing


One way to avoid write contention is to use async processing. The concept is simple. Instead of directly writing to your datastore after your backend receives a request, you put a message in a queue with all the information needed to run the operation later. On the other side, you have a set number of workers receiving messages and operating on them one after the other.

The advantage of such an approach is that you control the amount of workers and therefore the amount of maximum concurrent writes to your datastore. You can also process the queue before it gets worked and and maybe coalesce some messages or remove outdated/duplicated message. Finally, you can assign more workers to some message types, making sure the important messages get processed first.

Another advantage of this design includes not letting the client hang while you're processing the data and potentially timeout. You can also process a long queue faster by starting more workers to catch up and retire them later.
You app is more resilient to errors and failed async jobs can be restarted.


## Load test, monitor and be proactive


Even the best designs have weak spots and will have to be improved once they are released. Don't wait for your system to fall apart before looking for solutions. Monitor your app. Every single part of your app. Look for patterns showing signs of potential problems and imagine what you could do to resolve them if they would start manifesting.

Of course before getting there, you will need to understand each part of your system and benchmark/load test/profile your app so you can be ready to face the storm.

Benchmarks and load tests are both super important and, too often, not reflective of what you will really face later on. They are usually great at identifying major problems that should be resolved right away, but fail to show the one big problem you will see on day one when you have to deal with 20k concurrent requests. Use them as indicators, rely on your experience and learn about problems other have faced. This will help you build a knowledge of scalability challenges, their root causes, and their potential solutions.

For benchmarking Ruby code, I use the[ built-in benchmark tool available in the standard lib](http://ruby-doc.org/stdlib/libdoc/benchmark/rdoc/classes/Benchmark.html).
For simple load testing, I use [httperf](http://www.hpl.hp.com/research/linux/httperf/)/[autobench](http://www.xenoclast.org/autobench/) and [siege](http://freshmeat.net/projects/siege/).
For anything more complicated, I use [JMeter](http://jakarta.apache.org/jmeter/).
In the video game industry, we also often use sims using the client's code to create load.

Benchmarking without profiling is often useless. Unlike other programming languages, Ruby doesn't yet have awesome profiling tools easy to use, but things are evolving quickly. Here are some tools I use regularly.

The [Ruby wrapper](https://github.com/tmm1/perftools.rb) around [google perftools](http://code.google.com/p/google-perftools/) is really good.
Before using perftools as often as I do now, I frequently used [ruby-prof](http://ruby-prof.rubyforge.org/) with [kcachegrind](http://kcachegrind.sourceforge.net/html/Home.html).
Ruby 1.9 lets you inspect its garbage collector as explained in a [previous post](http://merbist.com/2010/07/29/object-allocation-why-you-should-care/).
And when using [MacRuby](http://macruby.org), I often use [DTrace](http://en.wikipedia.org/wiki/DTrace).


## Other misc. things I learned




### Documentation


Documentation is critical. It doesn't matter how you do it but you need to make sure you document what you want to build, how you build it, and why you build it. Documenting will help you and the others working on the project, and will keep you in check. I have started documenting an API and then realized that the design was flawed. Maybe it's just the way you name a method, or a class, or it can be a weird method signature or even the entire workflow being wrong, but when you document things, design errors appear more obviously.

To document Ruby code, I use [yard](http://yardoc.org/) which is quite similar to [javadoc](http://en.wikipedia.org/wiki/Javadoc). Code documentation, when writing [duck typed language](http://en.wikipedia.org/wiki/Duck_typing), is, for me, very important since it makes the API designer's expectations much clearer. I also often add English documentation, written in markdown files and compiled by yard. If you say that your code is simple and that it doesn't require documentation because anyone can just read it and understand ... then you have totally miss the point. Yes, it's more work to keep documentation and code in sync. But people using web APIs don't have access to the implementation details. The people distributing compiled APIs don't give access to their implementation. And honestly, the API should be decoupled from the implementation. I shouldn't have to guess how to use your API based on how you implemented the code underneath, otherwise my assumptions might be totally wrong.


### Simplicity


With great power comes great responsibility. The law of system entropy says that systems become more disorganized over time, so don't start with complicated code if you can avoid it! It's not because your programming language lets you do crazy stuff that you have to use it. In 90+% of the time, your code can be written without voodoo and be easier to read, easier to understand, easier to maintain and faster to execute.

If you can't figure out how to ***not*** use metaprogramming or weird patterns, take a step back and look at your design, did you miss something?
Also, don't reinvent the wheel. Use the language the way it was designed to be used. Keep your APIs as small as possible, don't expose too much as it will be virtually impossible to remove it later on.

As an example, look to what extent Rails modified the Ruby language:

In Rails' console (Rails 2, Ruby 1.8.7)

    
    
    >> Array.ancestors
    => [Array, ActiveSupport::CoreExtensions::Array::RandomAccess,
     ActiveSupport::CoreExtensions::Array::Grouping, ActiveSupport::CoreExtensions::Array::ExtractOptions,
     ActiveSupport::CoreExtensions::Array::Conversions, ActiveSupport::CoreExtensions::Array::Access,
     Enumerable, Object, ERB::Util, ActiveSupport::Dependencies::Loadable, Base64::Deprecated, Base64,
     Kernel]
    >> [].methods.size
    => 233


In irb:

    
    
    >> Array.ancestors
    => [Array, Enumerable, Object, Kernel]
    >> [].methods.size
    => 149


Removing any of these added methods is virtually impossible since some piece of code somewhere might rely on it.


### Abstraction & its dangers


Often when designing an API, it's preferable to offer a well defined  public API which will delegate the work to a private implementation shared between  multiple public APIs. This approach avoids duplication, makes maintenance easy, and  allows for more flexibility. As an example, we can have a public  matchmaking API which will delegate most of the work to a private  matchmaking interface. If required, swapping the private interface would be  totally transparent to the public API. This approach has a downside, however. Having a shared private implementation does create a duplication of APIs. It leaves us with both a public and a private API because we need an API for public access and a private API for the public API to connect to. But when we weigh the  benefits and look at what is duplicated, we realize that this trade off  is worth it.

Keeping a certain level of abstraction is important to maintaining the separation of concerns as clear as possible. You want to layer your design so that each  layer is responsible for itself, only knows about itself, and has limited  interactions with other layers. By factoring/isolating the different  modules, you can keep a simple, elegant, easy to maintain system. This  is a key element of design but one needs to be careful not to obfuscate  the design by over abstracting his/her code. This is particularly important when designing a scalable app because you will often need to be able to easily swap parts  to optimize each part of your system.

That said, a lot of code out there is unnecessarily complicated. I sometime wonder if the authors of such code try to show that they know some cool language tricks. Or maybe this is due to the fact that, too often, people are impressed by code they don't understand. The problem with overly complicated or magical code is that it creates yet another abstraction layer between the end user and API. It makes the API more opaque, and that's a cost you have to take into consideration. Every time you abstract something you have a cost associated with the abstraction. This cost can be calculated in terms of performance loss, clarity loss and maintainability cost.

This is exactly the same problem encountered when trying to normalize data in a database.
Normalizing is a great concept which makes a lot of sense ... until you realize that the cost of keeping your data normalized is too great and it becomes a major bottleneck, not letting you scale your application.
At this moment (and probably only then) that you need to denormalize your data.

It's the same thing with code abstraction. It's fine to abstract, unless the abstraction is such that it requires too much work to understand what is going on. A bit of duplication is often worth it, but be careful to not abuse it.


### Debugging


Ruby has a decent debugger called [ruby-debug](http://bashdb.sourceforge.net/ruby-debug.html) and I'm amazed by the amount of people who haven't heard about it.
I don't know what I would do if I couldn't use breakpoints and get an interactive shell to debug Ruby code.
Please people! This is 2011, stop using print statement as a means of debugging!


## Conclusion


That's is for this post. It was longer than expected and I feel I didn't really cover anything in depth, but hopefully you learned something new or at least read something that piqued your interest. I look forward to reading your comments and, hopefully, your blog posts sharing your experience in designing scalable software.
