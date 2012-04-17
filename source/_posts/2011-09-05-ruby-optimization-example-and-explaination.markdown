---
date: '2011-09-05 14:58:21'
layout: post
slug: ruby-optimization-example-and-explaination
status: publish
title: Ruby optimization example and explanation
wordpress_id: '1124'
categories:
- blog-post
- Misc
- Ruby
tags:
- performance
- ruby
- scalability
---

Recently I wrote a small DSL that allows the user to define some code that then gets executed later on and in different contexts. Imagine something like Sinatra where each route action is defined in a block and then executed in context of an incoming request.

The challenge is that blocks come with their context and you can't execute a block in the context of another one.

Here is a reduction of the challenge I was trying to solve:

    
    class SolutionZero
      def initialize(origin, &block;)
        @origin = origin
        @block = block
      end
    
      def dispatch
        @block.call
      end
    end
    
    SolutionZero.new(42){ @origin + 1 }.dispatch
    # undefined method `+' for nil:NilClass (NoMethodError)


The problem is that the block refers to the @origin instance variable which is not available in its context.
My first workaround was to use instance_eval:

    
    class SolutionOne
      def initialize(origin, &block;)
        @origin = origin
        @block = block
      end
    
      def dispatch
        self.instance_eval &@block
      end
    end
    
    SolutionOne.new(40){ @origin + 2}.dispatch
    # 42


My workaround worked fine, since the block was evaluated in the context of the instance and therefore the @origin ivar is made available to block context. Technically, I was good to go, but I wasn't really pleased with this solution. First using instance_eval often an indication that you are trying to take a shortcut. Then having to convert my block stored as a block back into a proc every single dispatch makes me sad. Finally, I think that this code is probably not performing as well as it could, mainly due to unnecessary object allocations and code evaluation.
I did some benchmarks replacing [instance_eval](https://github.com/ruby/ruby/blob/trunk/vm_eval.c#L1323) by [instance_exec](https://github.com/ruby/ruby/blob/trunk/vm_eval.c#L1355) since looking at the C code, instance_exec should be slightly faster. Turns out, it is not so I probably missed something when reading the implementation code.

I wrote some more benchmarks and profiled a loop of 2 million dispatches (only the #disptach method call on the same object). The GC profiler report showed that the GC was invoked 287 times and each invocation was blocking the execution for about 0.15ms.
Using Ruby's [ObjectSpace](http://ruby-doc.org/core/classes/ObjectSpace.html#M001526) and [disabling the GC](http://ruby-doc.org/core/classes/GC.html#M001373) during the benchmark, I could see that each loop allocates an object of type T_NODE which is more than likely our @block ivar converted back into a block. This is quite a waste. Furthermore, having to evaluate our block in a different context every single call surely isn't good for performance.

So instead of doing the work at run time, why not doing it at load time? By that I mean that we can optimize the #dispatch method if we could "precompile" the method body instead of "proxying" the dispatch to an instance_eval call. Here is the code:

    
    class SolutionTwo
      def initialize(origin, &block;)
        @origin = origin
        implementation(block)
      end
    
      private
    
      def implementation(block)
        mod = Module.new
        mod.send(:define_method, :dispatch, block)
        self.extend mod
      end
    end
    
    SolutionTwo.new(40){ @origin + 2}.dispatch
    # 42
    


This optimization is based on the fact that the benchmark (and the real life usage) creates the instance once and then calls #dispatch many times. So by making the initialization of our instance a bit slower, we can drastically improve the performance of the method call. We also still need to execute our block in the right context. And finally, each instance might have a different way to dispatch since it is defined dynamically at initialization. To work around all these issues, we create a new module on which we define a new method called dispatch and the body of this method is the passed block. Then we simply our instance using our new module.

Now every time we call #dispatch, a real method is dispatched which is much faster than doing an eval and no objects are allocated. Running the profiler and the benchmarks script used earlier, we can confirm that the GC doesn't run a single time and that the optimized code runs 2X faster!



Once again, it's yet another example showing that you [should care about object allocation](http://merbist.com/2010/07/29/object-allocation-why-you-should-care/) when dealing with code in the critical path. It also shows how to work around the block bindings. Now, it doesn't mean that you have to obsess about object allocation and performance, even if my last implementation is 2X faster than the previous, we are only talking about a few microseconds per dispatch. That said microseconds do add up and creating too many objects will slow down even your faster code since the GC will stop-the-world as its cleaning up your memory. In real life, you probably don't have to worry too much about low level details like that, unless you are working on a framework or sharing your code with others. But at least you can learn and understand why one approach is faster than the other, it might not be useful to you right away, but if you take programming as a craft, it's good to understand how things work under the hood so you can make educated decisions.




### Update:


@apeiros in the comments suggested a solution that works & performs the same as my solution, but is much cleaner:


    
    class SolutionTwo
      def initialize(origin, &block;)
        @origin = origin
        define_singleton_method(:dispatch, block) if block_given?
      end
    end
    
