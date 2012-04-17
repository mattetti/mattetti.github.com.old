---
date: '2011-10-03 21:23:54'
layout: post
slug: about-concurrency-and-the-gil
status: publish
title: About concurrency and the GIL
wordpress_id: '1146'
categories:
- blog-post
- Ruby
tags:
- concurrency
- fibers
- gil
- jruby
- macruby
- python
- rubinius
- ruby
- rubyconf
- threads
---

During RubyConf 2011, concurrency was a really hot topic. This is not a new issue, and the JRuby team has been talking about true concurrency for quite a while . The Global Interpreter Lock has also been in a subject a[ lot of discussions in the Python community](http://wiki.python.org/moin/GlobalInterpreterLock) and it's not surprising that the Ruby community experiences the same debates since the evolution of their implementations are somewhat similar. (There might also be some tension between [EngineYard](http://engineyard.com) hiring the JRuby and Rubinius teams and [Heroku](http://heroku.com) which [recently hired Matz](http://blog.heroku.com/archives/2011/7/12/matz_joins_heroku/) (Ruby's creator) and [Nobu](https://github.com/nobu), the #1 C Ruby contributor)

The GIL was probably even more of a hot topic now that [Rubinius](http://rubini.us/) is about the join [JRuby](http://jruby.org) and [MacRuby](http://macruby.org) in the realm of GIL-less Ruby implementations.

During my RubyConf talk ([slides here](http://rubyconf11.merbist.com/)), I tried to explain how C Ruby works and why some decisions like having a GIL were made and why the Ruby core team isn't planning on removing this GIL anytime soon. The GIL is something a lot of Rubyists love to hate, but a lot of people don't seem to question why it's here and why Matz doesn't want to remove it. Defending the C Ruby decision isn't quite easy for me since I spend my free time working on an alternative Ruby implementation which doesn't use a GIL (MacRuby). However, I think it's important that people understand why the MRI team (C Ruby team) and some Pythonistas feels so strongly about the GIL.

**What is the GIL?**

Here is a quote from the [Python wiki](http://wiki.python.org/moin/GlobalInterpreterLock):


> In CPython, the **global interpreter lock**, or **GIL**, is a mutex that prevents multiple native threads from executing Python bytecodes at once. This lock is necessary mainly because CPython's memory management is not thread-safe. (However, since the GIL exists, other features have grown to depend on the guarantees that it enforces.) [...] The GIL is controversial because it prevents multithreaded CPython programs from taking full advantage of multiprocessor systems in certain situations. Note that potentially blocking or long-running operations, such as I/O, image processing, and [NumPy](http://wiki.python.org/moin/NumPy) number crunching, happen _outside_ the GIL. Therefore it is only in multithreaded programs that spend a lot of time inside the GIL, interpreting CPython bytecode, that the GIL becomes a bottleneck.


The same basically applies to C Ruby. To illustrate the quote above, here is a diagram representing two threads being executed by C Ruby:


[![Fair thread scheduling in Ruby by Matt Aimonetti](http://rubyconf11.merbist.com/images/thread_scheduling.023.jpg)](http://rubyconf11.merbist.com/#44)


Such a scheduling isn't a problem at all when you only have 1 cpu, since a cpu can only execute a piece of code at a time and context switching happens all the time to allow the machine to run multiple processes/threads in parallel. The problem is when you have more than 1 CPU because in that case, if you were to only run 1 Ruby process, then you would most of the time only use 1 cpu at a time. If you are running on a 8 cpu box, that's not cool at all! A lot of people stop at this explanation and imagine that their server can only handle one request at a time and they they rush to sign Greenpeace petitions asking Matz to make Ruby greener by optimizing Ruby and saving CPU cycles. Well, the reality is slightly different, I'll get back to that in a minute. Before I explain "ways to achieve true concurrency with CRuby, let me explain why C Ruby uses a GIL and why each implementation has to make an important choice and in this case both CPython and C Ruby chose to keep their GIL.




### Why a GIL in the first place?





	
  * It makes developer's lives easier (it's harder to corrupt data)

	
  * It avoids race conditions within C extensions

	
  * It makes C extensions development easier (no write barriers..)

	
  * Most of the C libraries which are wrapped are not thread safe

	
  * Parts of Ruby's implementation aren't threadsafe (Hash for instance)




As you can see the arguments can be organized in two main categories: data safety and C extensions/implementation. An implementation which doesn't rely too much on C extensions (because they run a bit slow, or because code written in a different language is preferred) is only faced with one argument: data safety.





### 




### Should C Ruby remove its GIL?





	
  * No: it potentially makes Ruby code unsafe(r)

	
  * No: it would break existing C extensions

	
  * No: it would make writing C extensions harder

	
  * No: it's a lot of work to change make C Ruby threadsafe

	
  * No: Ruby is fast enough in most cases

	
  * No: Memory optimization and GC is more important to tackle first

	
  * No: C Ruby code would run slower

	
  * Yes: we really need better/real concurrency

	
  * Yes: [Rubber boots analogy (Gustavo Niemeyer)](https://plus.google.com/107994348420168435683/posts/993U42yVbfk)




Don't count the amount of pros/cons to jump to the conclusion that removing the GIL is a bad idea. A lot of the arguments for removing the GIL are related. At the end of the day it boils down to data safety. During the Q&A section of my RubyConf talk, Matz came up on stage and said data safety was the main reason why C Ruby still has a GIL. Again, this is a topic which was discussed at length in the Python community and I'd encourage you to read arguments from the [Jython](http://www.jython.org/) (the equivalent of JRuby for Python) developers, [the PyPy](http://codespeak.net/pypy/dist/pypy/doc/faq.html#does-pypy-have-a-gil-why) (the equivalent of Rubinius in the Python community) and CPython developers. (a good collection of arguments are actually available in the comments related to the [rubber boots post mentioned earlier](https://plus.google.com/107994348420168435683/posts/993U42yVbfk))





### How can true concurrency be achieved using CRuby?








	
  * Run multiple processes (which you probably do if you use Thin, Unicorn or Passenger)

	
  * Use event-driven programming with a process per CPU

	
  * MultiVMs in a process. Koichi presented his plan to run multiple VMs within a process.  Each VM would have its own GIL and inter VM communication would be faster than inter process. This approach would solve most of the concurrency issues but at the cost of memory.




Note:  forking a process only saves memory when using REE since it implements a GC patch that makes the forking process Copy on Write friendly. The Ruby core team worked on a patch for Ruby 1.9 to achieve the same result. [Nari](http://twitter.com/#!/nari_en) & [Matz](http://twitter.com/#!/yukihiro_matz) are currently working on improving the implementation to make sure overall performance isn't affected.





Finally, when developing web applications, each thread spend quite a lot of time in IOs which, as mentioned above won't block the thread scheduler. So if you receive two quasi-concurrent requests you might not even be affected by the GIL as illustrated in [this diagram from Yehuda Katz](http://yehudakatz.com/2010/08/14/threads-in-ruby-enough-already/):

![](http://yehudakatz.com/wp-content/uploads/2010/08/Untitled.002.png)

This is a simplified diagram but you can see that a good chunk of the request life cycle in a Ruby app doesn't require the Ruby thread to be active (CPU Idle blocks) and therefore these 2 requests would be processed almost concurrently.




To boil it down to something simplified, when it comes to the GIL, an implementor has to chose between data safety and memory usage. But it is important to note that context switching between threads is faster than context switching between processes and data safety can and is often achieved in environments without a GIL, but it requires more knowledge and work on the developer side.




### Conclusion




The decision to keep or remove the GIL is a bit less simple that it is often described. I respect Matz' decision to keep the GIL even though, I would personally prefer to push the data safety responsibility to the developers. However, I do know that many Ruby developers would end up shooting themselves in the foot and I understand that Matz prefers to avoid that and work on other ways to achieve true concurrency without removing the GIL. What is great with our ecosystem is that we have some diversity, and if you think that a GIL less model is what you need, we have some great alternative implementations that will let you make this choice. I hope that this article will help some Ruby developers understand and appreciate C Ruby's decision and what this decision means to them on a daily basis.



