---
date: '2012-02-23 09:46:49'
layout: post
legacy_url: http://merbist.com/2012/02/23/quick-dive-into-ruby-orm-object-initialization/
slug: quick-dive-into-ruby-orm-object-initialization
source: merbist.com
status: publish
title: Quick dive into Ruby ORM object initialization
wordpress_id: '1225'
categories:
- rails
- ruby
- Software Design
- Tutorial
- merbist.com
- blog-post
- Popular
tags:
- activerecord
- ORM
- performance
- profiling
- ruby
---

Yesterday I did some quick digging into how ORM objects are initialized and the performance cost associated to that. In other words, I wanted to see what's going on when you initialize an ActiveRecord object.

Before I show you the benchmark numbers and you jump to conclusions, it's important to realize that in the grand scheme of things, the performance cost we are talking is small enough that it is certainly not the main reason why your application is slow. Spoiler alert: ActiveRecord is slow but the cost of initialization isn't by far the worse part of ActiveRecord. Also, even though this article doesn't make activeRecord look good, and I'm not trying to diss it. It's a decent ORM that does a great job in most cases.

Let's get started by the benchmarks number to give us an idea of the damage (using Ruby 1.9.3 p125):

 

    
                                                                 | Class | Hash  | AR 3.2.1 | AR no protection | Datamapper | Sequel |
    --------------------------------------------------------------------------------------------------------------------------------------
    .new() x100000                                               | 0.037 | 0.049 | 1.557    | 1.536            | 0.027      | 0.209  |
    .new({:id=>1, :title=>"Foo", :text=>"Bar"}) x100000          | 0.327 | 0.038 | 6.784    | 5.972            | 4.226      | 1.986  |


 

You can see that I am comparing the allocation of a Class instance, a Hash and some ORM models. The benchmark suite tests the allocation of an empty object and one with passed attributes. The benchmark in question is available [here](https://github.com/mattetti/benchmarks/blob/master/init_objects.rb).

As you can see there seems to be a huge performance difference between allocating a basic class and an ORM class. Instantiating an ActiveRecord class is 20x slower than instantiating a normal class, while ActiveRecord offers some extra features, why is it so much slower, especially at initialization time?

The best way to figure it out is to profile the initialization. For that, I used [perftools.rb](https://github.com/tmm1/perftools.rb) and I generated a graph of the call stack.

Here is what Ruby does (and spends its time) when you initialize a new Model instance (click to download the PDF version):

 

[![Profiler diagram of AR model instantiation by Matt Aimonetti](http://merbist.com/wp-content/uploads/2012/02/AR-model-instantation-by-Matt-Aimonetti.jpg)](http://github.com/mattetti/benchmarks/blob/master/ar_init_profile.pdf?raw=true)

 

This is quite a scary graph but it shows nicely the features you are getting and their cost associated. For instance, the option of having the before and after initialization callback cost you 14% of your CPU time per instantiation, even though you probably almost never use these callbacks. I'm reading that by interpreting the node called ActiveSupport::Callback#run_callbacks, 3rd level from the top. So 14.1% of the CPU time is spent trying to run callbacks. As a quick note, note that 90.1% of the CPU time is spent initializing objects, the rest is spent in the loop and in the garbage collection (because the profiler runs many loops). You can then follow the code and see how the code works, creating a dynamic class callback method on the fly (the one with the long name) and then recreating the name of this callback to call it each time the object is allocated. It sounds like that's a good place for some micro optimizations which could yield up to 14% performance increase in some cases.

Another major part of the CPU time is spent in ActiveModel's sanitization. This is the piece of code that allows you to block some model attributes to be mass assigned. This is useful when you don't want to sanitize your incoming params but want to create or update a model instance by using all the passed user params. To avoid malicious users to modify some specific params that might be in your model but not in your form, you can protect these attributes. A good example would be an admin flag on a User object. That said, if you manually initialize an instance, you don't need this extra protection, that's why in the benchmark above, I tested and without the protection. As you can see, it makes quite a big difference. The profiler graph of the same initialization without the mass assignment protection logically ends up looking quite different:

 

[
](https://github.com/mattetti/benchmarks/blob/master/ar_init_no_protection.pdf?raw=true)[![Matt Aimonetti shows the stack trace generated by the instantiation of an Active Record model](http://merbist.com/wp-content/uploads/2012/02/AR-model-instantiation-without-mass-assignment-by-Matt-Aimonetti.jpg)](https://github.com/mattetti/benchmarks/blob/master/ar_init_no_protection.pdf?raw=true)

 

**Update:** My colleague [Glenn Vanderburg](https://twitter.com/#!/glv) pointed out that some people might assuming that the shown code path is called for each record loaded from the database. This isn't correct, the graph represents instances allocated by calling #new. See the addition at the bottom of the post for more details about what's going on when you fetch data from the DB.

I then decided to look at the graphs for the two other popular Ruby ORMs:

[Datamapper](http://datamapper.org/)

[![](http://img.skitch.com/20120223-txs4wa7b5rdpg45aj6354xg1wt.jpg)](https://github.com/mattetti/benchmarks/blob/master/dm_init_profile.pdf?raw=true)

 

and [Sequel](http://sequel.rubyforge.org/)


[![](http://img.skitch.com/20120223-p2jx6ypk35ucsgtx7p1tcabpes.jpg)](https://github.com/mattetti/benchmarks/blob/master/sequel_init_profile.pdf?raw=true)


 

 

While I didn't give you much insight in ORM code, I hope that this post will motivate you to sometimes take a look under the cover and profile your code to see what's going on and why it might be slow. **Never assume, always measure**. Tools such as perftools are a great way to get a visual feedback and get a better understanding of how the Ruby interpreter is handling your code.


## UPDATE:


I heard you liked graphs so I added some more, here is what's going on when you do Model.first:

[![](http://img.skitch.com/20120224-f23s8xctghi8mj6ax3cdw9aq25.jpg)](https://github.com/mattetti/benchmarks/blob/master/ar_first_profile.pdf?raw=true)

 

Model.all

[![](https://img.skitch.com/20120224-q29q4n7bj3i96erk1enxdqxb5e.jpg)](https://github.com/mattetti/benchmarks/blob/master/ar_all_profile.pdf?raw=true)

 

And finally this is the code graph for a call to Model.instantiate which is called after a record was retrieved from the database to convert into an Object. (You can see the #instantiate call referenced in the graph above).

 

[![](http://img.skitch.com/20120224-8scmun9n1c9ufdnxa8rq2961bq.jpg)](https://github.com/mattetti/benchmarks/blob/master/ar_instantiate_profile.pdf?raw=true)
