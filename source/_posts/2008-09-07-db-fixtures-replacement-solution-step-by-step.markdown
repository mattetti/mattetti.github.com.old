---
date: '2008-09-07 16:06:00'
layout: post
legacy_url: http://railsontherun.com/2008/09/07/db-fixtures-replacement-solution-step-by-step/
slug: db-fixtures-replacement-solution-step-by-step
source: railsontherun.com
status: publish
title: db fixtures replacement solution step by step
wordpress_id: '1370'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- datamapper
- factory_girl
- fixtures
- merb
- RSpec
- spec
- testing
---

Like most people who started with Rails a while back, I first loved Rails fixtures and ended up hating them (slow, a pain to maintain etc...).

I went through different experiments, trying different existing libs, writing my own solutions etc... I wasn't quite satisfied until I found [factory_girl](http://github.com/thoughtbot/factory_girl) from [thoughtbot](http://www.thoughtbot.com/).

You might not feel the need for a decent fixtures solution if you do a lot of mocking/stubbing, but I recently came back from my "mock everything you can outside of models" approach and I'm getting closer to the [mock roles, not objects](http://snipr.com/3nwry) approach. So, I'm loosing my model/controller testing separation but I'm gaining by not having to maintain "dumb mocks" which don't always represent the real API behind. I mean, how many times did I change a Model, messing up my app but all my specs were still passing. Anyway, that's a long discussion, which will be covered by [wycats](http://yehudakatz.com/) during [merbcamp](http://merbcamp.com)

So here is a simple example of how I use [factory girl](http://github.com/thoughtbot/factory_girl) in a Merb + DataMapper app. (you can do the same in a Rails app, there is **nothing** specific to Merb in factory_girl).



	
  * I. create an empty app, set the ORM etc...

	
  * II. git pull and install factory_girl from [http://github.com/thoughtbot/factory](http://github.com/thoughtbot/factory_girl/tree/master)_[girl/tree/master](http://github.com/thoughtbot/factory_girl/tree/master). Or install thoughtbot-factory_girl gem using [GitHub gem server](http://gems.github.com).

	
  * **III.** create a spec/factories.rb file. (You might prefer to create a folder called spec/factories and add a factory per model)

	
  * **IV.** modify spec_helper.rb and add the following




    
    
      require 'factory_girl'
      require File.dirname(__FILE__) + '/factories'
    






	
  * V. write some specs against a Client model







	
  * **VI.** Create the Model







	
  * **VII.** create a factory







	
  * 
**IIX.** run your specs
 
![failing specs](http://img.skitch.com/20080907-tf8yy6fi82b23t78stqii3mpbe.jpg)


	
  * 
**IX.** fix the model (note that I set `dependencies "dm-validations"` in my init.rb)








	
  * X. run the specs

![passing specs](http://img.skitch.com/20080907-m7p2r6q1qau4k3qsmeadwu2tur.jpg)

	
  * **XI.** add more specs



 
As you can see, Factory.build(:client) only creates a new instance of the Object, while Factory(:client) creates, saves and loads the instance.




	
  * **XII.** get them to pass





Factory Girl makes fixtures simple and clean. Here is another example for creating associations:



Factory Girl also supports sequencing, check out FG [read me](http://github.com/thoughtbot/factory_girl)


## In conclusion, Factory Girl is a mature and solid factory solution which will take you less than 15 minutes to get used to. It will offer you loads of flexibility and less frustration than good old yaml fixtures. You can also use it with existing fixtures if you want to start using it in an existing app.
