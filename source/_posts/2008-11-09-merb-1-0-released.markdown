---
date: '2008-11-09 22:47:25'
layout: post
slug: merb-1-0-released
status: publish
title: Merb 1.0 released
wordpress_id: '204'
categories:
- merb
- Misc
- News
tags:
- matz
- merb
- release
- ruby
- support
- training
---

[![](http://farm1.static.flickr.com/47/120000855_c7d39781f7_m.jpg)](http://flickr.com/photos/absolutely_loverly/120000855/)


[![](http://merbist.com/wp-content/uploads/2008/11/nihon-japan-48.png)](http://d.hatena.ne.jp/kwatch/20081112/1226448192)
[Japanese translation](http://d.hatena.ne.jp/kwatch/20081112/1226448192)


  


On November 7, [Yehuda Katz](http://yehudakatz.com/) gave a talk at [RubyConf](http://rubyconf.org) and made 3 major announcements:

- [Engine Yard](http://engineyard.com) to offer [Merb support](http://engineyard.com/merbsupport)

- [m|a agile](http://ma-agile.com/) to offer [professional training](http://ma-agile.com/training/)

- **Merb 1.0 released!**

The first announcement is pretty substantial. [Engine Yard](http://engineyard.com) has been financially supporting [Merb](http://merbivore.com) by letting some of their staff work on Merb, fly them to different conferences and sponsoring events like [Merb Camp](http://merbcamp.com). Engine Yard didn't yet announce the price structure but **having the option for enterprise level support for Merb is just awesome**.

[John Nunemaker](http://railstips.org) made an interesting comment during RubyConf [Pivotal](http://pivotallabs.com/) Party. Something special about Merb is that it was designed to fit the needs of an audience instead of trying to create a series of tools to build a specific type of website. Having Engine Yard help to finance Merb dev and offer support is very reassuring. It's something already done by many other OSS projects such as Ubuntu, MySQL etc...

[![](http://merbist.com/wp-content/uploads/2008/11/merb-training-small-238x300.gif)](http://ma-agile.com/training)The second announcement Yehuda made was about [Merb Training](http://ma-agile.com/training/). I'm probably pretty biased since I am at the origin of this project. I know for a fact that a lot of people were waiting for 1.0 to get started with Merb. We are also working on getting more documentation out, and 3 books are coming up. Still, the best way to learn is to sit down with people who know Merb who can teach you the way its intended to be used.

Training will allow you to benefit a lot from being with other people who also share the same desire to **master Ruby's most powerful and flexible web framework**.

What's also really exciting is that [Yehuda Katz](http://yehudakatz.com), Merb's lead developer, agreed to be a tutor for the course. I can't imagine a better way to learn. Check [this page](http://ma-agile.com/training/) for more information about the next training session or get in contact with me if you want to organize a training session for your company.


### Finally, the **big news was the announcement of Merb 1.0**!


Merb 1.0 went through 5 release candidates and was finally marked as final. In the last few months, the Merb team worked hard to make things easier for people who want to get started in no time.

Let's quickly look at why Merb is awesome:



	
  * **Merb is Modular**. Merb is not a monolithic framework. You can pick and choose what you need. Create a 1 file app "Ã  la Sinatra" or a rich web app "Ã  la Rails". Merb has many components. Only requires the ones you need and save precious resources. (Merb doesn't believe in 1 size fits all)

	
  * **Merb is agnostic** (kinda). Because people have different needs and different believes, Merb won't force you to use one ORM or another. Same thing goes for the template engine or for the JavaScript library you want to use. ActiveRecord, DataMapper, Sequel, RelaxDb, Haml, Erb, Prototype, jQuery... choose which one you want and change whenever you want.

	
  * **Merb _can_ be opinionated**. Merb offers a default stack using DataMapper and jQuery, authentication, exceptions and caching setup for you. However, creating your own stack is dead easy. As a matter of fact, the guys at [yellowpages.com](http://yellowpages.com) are using Merb and were talking about creating their own stack using Sequel.

	
  * **Merb let you reuse your code**. Borrowed from [Django](http://www.djangoproject.com/), Merb has something called "slices". Slices are mini apps you can run standalone or mounted within another app. A slice is a great way to write code you can reuse. Unlike plugins which extend the framework features, slices are a way to provide encapsulated content. ([David Chelimsky,](http://blog.davidchelimsky.net/) RSpec's author and maintainer actually said that slices where his favorite feature in Merb 1.0)

	
  * **Merb has an API**. You might be wondering why having an API is awesome. Well, the truth is that the Merb Team spent time marking methods public, which are guaranteed to not break until the next major release (any change to the public API will be well documented). There is also a plugin API meaning that plugin developers won't have to worry about upgrades if they stick to the plugin API.

	
  * **Merb is fast.** Even though Ruby the language isn't really fast and contrary to popular opinion, Ruby for the web is one of the fastest solutions out there. (Even Rails is [way faster than all the mainstream PHP frameworks](http://www.slideshare.net/wycats/merb-camp-keynote-presentation)) And that's what Merb is proving by being one of the fastest web framework available on the market. [Jason Seifer](http://railsenvy.com/) will be interested to know that Merb isn't scared to scale ;) As a matter of fact, Merb is going to scale even better in the next few months as we are planning to integrate [Swiftiply](http://swiftiply.swiftcore.org/index.html) and do some totally awesome stuff to spawn/reap workers based load. (more about that in few weeks).

	
  * **Matz likes Merb**. Ruby's daddy, Yukihiro Matsumoto told us he likes the flexibility of Merb and the fact that the framework doesn't create a DSL on top of Ruby. He even told us that he's going to introduce his company to Merb! We were obviously very honored and for us, it validates months of work by dozens of contributors. Here is a transcript of Matz comments about Merb and the Ruby web world:

[caption id="attachment_213" align="alignleft" width="125" caption="Yukihiro Matsumoto aka Matz"][![Yukihiro Matsumoto aka Matz](http://merbist.com/wp-content/uploads/2008/11/yukihiro_matsumoto_125px.jpg)](http://ruby-lang.org)[/caption]


> "Everyone outside of the Ruby community understands that we only have 1 web application framework, named Rails, but it's not true in any sense. We have several post-Rails frameworks, which is very good, and I believe in diversity.

Merb has a bright future for the people who are not satisfied by the fixed ways in Rails.Â  I think that Merb will give users more freedom in a Ruby-ish way of programming.

I'm not really a web-guy, so i don't judge any of them [frameworks], but Rails does some kind of drastic change on the language itself like in Active Support.Â  But Ruby has its own culture and technological atmosphere in the language so that keeping that makes me feel easier."




	
  * **Merb is memory friendly and therefore cheap**. Merb is Open Source and free, but hosting an application costs money. Merb memory footprint is tiny compared to other solutions and that means that hosting will cost you less. (interesting when you think that EngineYard help developing Merb :p) Using [Ruby Entreprise Edition](http://www.rubyenterpriseedition.com/), you will even use less memory, meaning you save even more money ;) By the way, Matz told me this morning that the Ruby core team is working on their own solution for a better GC and it should be available soon. (Ruby 1.9.x)

	
  * **Merb source code is easy to read**. Because [Merb code](http://github.com/wycats/merb/tree/master) is modular and because Merb has a concept of an API, reading Merb's source code is pretty easy. On top of that, Merb itself uses RSpec making tests really easy to read and understand.Â  What's great when the source code is easy to read is that developers can quickly check the source code if they want to understand how things work. We also get better patches from contributors and we keep our code clean. We believe in the theory of the "Broken Windows" by James Q. Wilson and George L. Kelling:


> "Consider a building with a few broken windows. If the windows are not repaired, the tendency is for vandals to break a few more windows. Eventually, they may even break into the building, and if it's unoccupied, perhaps become squatters or light fires inside.

Or consider a sidewalk. Some litter accumulates. Soon, more litter accumulates. Eventually, people even start leaving bags of trash from take-out restaurants there or breaking into cars."





I guess I could keep on going with other reasons why Merb is so awesome, but let's keep some for another post ;)

In conclusion, on behalf of the Merb core team, I'd like to thank all the Merb contributors, Ezra Zygmuntowic (creator of Merb), Yehuda Katz (Merb lead developer), Matz (Ruby creator) and finally DHH & the Rails core team. One more thing:

`
$ sudo gem install merb
$ merb-gen app my-awesome-merb-app
`
