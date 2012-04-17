---
date: '2008-12-25 20:11:02'
layout: post
slug: merb-rails-merge-or-why-should-merbists-be-happy
status: publish
title: Merb/Rails merge, or Why should merbists be happy?
wordpress_id: '398'
categories:
- merb
- rails
tags:
- merb
- merge
- rails
- rails 3.0
---

[![](http://farm4.static.flickr.com/3211/2478140383_8e2e4ab7a3_m.jpg)](http://flickr.com/photos/xrrr/2478140383/)December 23, 2008, was an important day for the Ruby community. People who used to argue and not get along, have decided to sit down, talk and evaluate their differences. The end result is a strong collaboration of two teams who share the exact same goal.

Overall, the news was very well received, just look at the tweets out there and the comments on [Yehuda's](http://yehudakatz.com/2008/12/23/rails-and-merb-merge/) and [Rails' blogs](http://weblog.rubyonrails.org/2008/12/23/merb-gets-merged-into-rails-3).

Interestingly enough, the Rails community really embraced this decision and just got totally excited about the perspective of merging merb's philosophy and expertise into Rails. If you re-read David's blog post you can see that it's a **real homage to Merb**.

The rest of the internet sounds pretty happy about the news, here are few posts:



	
  * [arstechnica.com](http://arstechnica.com/news.ars/post/20081224-ruby-on-rails-and-merb-to-merge-for-rails-3.html)

	
  * [internetnews](http://www.internetnews.com/dev-news/article.php/3793296/Merb+Merges+With+Rails.htm)


Even [Zed the entertainer](http://www.zedshaw.com) thought it was a good thing:


> ["I honestly didnâ€™t think that would ever happen. I just assumed that Merb
would eventually wipe out Rails by being the better framework, or theyâ€™d
wipe each other out soon.](http://www.zedshaw.com/blog/index.html)

[So, congrats are in order. You guys are finally grown-ups and now have
the chance to make something better."](http://www.zedshaw.com/blog/index.html)


Some [merbists like Jade Meskill](http://iamruinous.com/2008/12/23/why-merb-becoming-rails-3-is-a-good-thing/) really understood what we are trying to do while some other ones got really [mad at us](http://www.mr-eel.com/archives/158).

I think a pattern emerged from the negative reactions:



	
  * Why merge when we are about to win?!

	
  * What does Merb win by being merged into Rails?

	
  * Why not merge Rails into Merb?

	
  * You are killing innovation by killing the competition

	
  * You screwed us over and now I have to go "back" to Rails

	
  * Rails 3.0 won't even be as good as Merb 1.x

	
  * The Rails team won't let you do what you have to do to merge Merb into Rails

	
  * DHH is a jerk


Let me quote Yehuda:Â  **"Calm yourselves ;)"**.


## Why merge when we are about to win?!


This is probably the most rational argument. This is also something the Merb core team considered for a little bit.
Merb is gaining huge momentum and the target audience was very reactive to what we did.
[![](http://farm2.static.flickr.com/1087/1317815300_095983272e_m.jpg)](http://flickr.com/photos/mhaithaca/1317815300/)People such as Yellow Pages, Wikipedia and even Adobe started using or looking seriously at Merb because of its focus on modularity and performance.
We started building an elite community and we were pretty proud of that.

But take a moment to think about it. Our goal has never been to hurt or compete with Rails. Our goals were to get modularity, performance and a strong API. If the Rails team really wants that, and will work on it, why should we work against each other?
It's not about winning or losing. It's all about the long term plan of your framework, about the people involved and the community behind it. We had to take ourselves out of the equation and consider what would be good for the Ruby community.


## What does Merb win by being merged into Rails?


People seem to easily find things we might lose but have a hard time finding things we are gaining.
![](http://merbist.com/wp-content/uploads/2008/12/rails.png)Looking at it on the very short term, this is probably correct. The merb team will have to learn how to work with the Rails team.
We need to understand the reasons behind every single aspect of the code and find ways of merging things nicely.
On top of that, we still need to work on merb as promised. (see Wycats' post)

However, in the long term we get all the benefits of Rails:
- stability
- community
- traction
- experience
- don't have to always justify why use Merb instead of Rails

But more importantly, we extend our team.

Most people using a framework might not realize what it is to work on a big Open Source project.
When you work on an OSS project, people come and go, and that's why you usually have a core team of people you can rely on.
Merb has a lot of contributors but a small core team of 5 people (Yehuda, Carl, Daniel, Michael and myself).
Managing a project, such as merb, requires a tremendous amount of time and patience. Rails has the same problem with its core team of 6 people. People have lives, business, projects etc...

Joining two teams of experts in developing web frameworks in Ruby is like if in the next Soccer World Cup, the French and Italian teams would go on the fields at the same time to beat other teams. 22 players on 1 side, training and learning together. American readers, please imagine the Giants and the Colts facing the Green Bay Packers (I was told I don't like the Packers).

Long term, you will get better quality and more frequent releases. We also have different world views and backgrounds which means we will learn a lot from each other, again that means better code for you guys :)


## Why not merge Rails into Merb?


That's actually a good question. We discussed maybe using merb-core as a base for Rails 3.0
The truth is that Merb 2.0 would probably be as big as Rails but more modular.
So we have the choice to keep on building on top of Merb 2.0 or deconstruct Rails.
As the Russians say: 'Ð»Ð¾Ð¼Ð°Ñ‚ÑŒ â€” Ð½Ðµ ÑÑ‚Ñ€Ð¾Ð¸Ñ‚ÑŒ', it's always easier to take things apart ;)

![](http://farm4.static.flickr.com/3033/2814371262_04b9876490_m.jpg)Rails already has a test suite, it has already been tested on the wild for a while and its internals are known to many developers. Taking apart the code to make it faster, cleaner and more modular is arguably easier than reinventing the wheel. At the end of the day it doesn't really matter from which end you start as long as you end up with the same result.

Some people even asked to come up with a new name for the merged project. Meails, Mr Ails, Reverb, Reab were suggested. While I thought it was a joke, some people were really serious. Can you imagine if during the recent bailout, banks changed names every time they were purchased by another bank? People would be super confused and would not even know where to send their mortgage payments.
On top of that, Rails has a huge user base compared to Merb and has an immense brand recognition in the world at large, it would be foolish to throw that away.

Let's not be chauvinistic and see what's best for all.


## You are killing innovation by killing the competition


Again, a good point but my question to you is: Should we stay in the competition just for the sake of it?

Rails clearly told us: we want what you have and we would love you to work with us. So the options were:



	
  * tell them to go to hell and let them try to redo what we already did and know how to do.

	
  * accept to work with them and make Rails a better framework.


_Option 1_ would keep the competition alive, but now you have 2 groups of people trying to do the same thing and being better at different aspects. The community gets confused and communication breaks.

_Option 2_: we lose the Merb vs Rails competition, but we double the amount of people working on Rails and therefore increase the change to make it better faster.

We went for option 2 and we know there is already some competition out there and there will be more coming up soon. I don't think it's realistic to expect us not to merge because we want to keep the competition going.


## You screwed us over and now I have to go "back" to Rails


Yehuda will blog about some more detailed examples as we are making progress, but you need to stop thinking that Merb will just be absorbed into Rails.
If Rails just wanted to add some "Merb flavor" to Rails, they would have just taken whatever they needed and would not be interested in a merge.
See Rails 3.0 as a new generation of Rails, whatever we promised you for merb 2.0 plus all the goodies from Rails. Rails users will still have their default stack and all choices will be made for them (like in the current Merb stack). The difference for standard Rails user will be better performance, a static API and an option to go off the "golden path".
Merbists won't lose the stuff they like in Merb, stacks, full support for DataMapper, Sequel and other ORMS, jQuery or other JS library, opt-in solution using just rails-core, better core isolation, built-in RSpec and webrat support, slices, merb-cache, merb-auth and all the other key plugins that will be ported over.

To be able to achieve all of that, we will have to make some infrastructure and code modifications.

Rails internals should end up:

* way less magical (even Merb uses some magic, but we'll make sure to keep it to a minimum)
* returning shorter and cleaner stack traces
* cleaner (required for the new API)
* better isolated (required to increase the modularity)
* alias_method_chain won't be available at the public API level (we probably still will need some chaining mechanism internally, but that's a different story)

So, no, you don't have to go "back" to Rails. In fact, imagine you could do exactly what Rails does but with the performance and modular architecture of Merb. That's what you will get with Rails 3.0.


## Rails 3.0 won't even be as good as Merb 1.x


I think I mainly replied to this question in my previous answer.
There is no reason why Rails 3.0 won't be better than Merb 1.x.Â  Actually, I believe our API will actually be even better. With the help of David, the existing Rails core team, and the Rails community, we will be able to define an awesome API that will change the way ruby web development will be done.
The merb API is great but we already know some of its limitations and we don't have as many plugin developers to work with. Working with plugin developers is something I'm personally excited about. As a Rails developer I have been really frustrated when using 3rd party plugins and trying to develop my own.
Having a well tested, developed against, public API will make all the Rails 3.0 plugins so much better. And because Merb plugins already use an API, we will be able to port all the plugins over, so it will be at least as good as 1.x
Also, we are going to work on real app benchmark tests to make sure the performance gain is at least as good as what we have with Merb.

Migration will be easy and well documented. We are not giving up on the Merb book and it will be very useful to explain the Merb way to new comers wanting an idea of the new stuff in Rails 3.0. It will also be the best source of information to migrate your app to Rails 3.0.

Talking about migration, we promised to give you a sane migration path when it will be time to migrate.
Again, don't freak out because we are changing the name, the spirit of Merb will keep on living.


## The Rails team won't let you do what you have to do to merge Merb into Rails


In all honestly, I was worried about that. I was wondering if all of that was not an evil scam planned by DHH to kill Merb as it was getting a good momentum. I like conspiracy theories and it sounded pretty good.
To my surprised, after a few private conversations with David, I realized that he was genuinely interested in making Rails better for people and fulfill the needs of people who need more flexibility.
Just re-read his blog post [http://weblog.rubyonrails.org/2008/12/23/merb-gets-merged-into-rails-3](http://weblog.rubyonrails.org/2008/12/23/merb-gets-merged-into-rails-3). After what he said, how can he back up and just keep Rails the way it is? And why in the world would he want that to happen?
It's a team effort and we have already spent hours and hours discussing some details. I can promise you that the Rails team is very excited about the new stuff that's coming up. But don't forget that it's a merge and we are reconsidering some of the stuff we did in Merb to better re-implement them in Rails 3.0.
So far, I haven't seen any of the Rails core team member tell us, no you can't do that because that's not the way it's done in Rails or because we just don't like it.


## DHH is a jerk


Recently, in an interview I gave to rubylearning.com [http://rubylearning.com/blog/2008/12/18/matt-aimonetti-why-on-earth-would-you-ignore-merb/](http://rubylearning.com/blog/2008/12/18/matt-aimonetti-why-on-earth-would-you-ignore-merb/) I mentioned that a big difference between Merb and Rails was the way we were dealing with the user base.
I quoted David from an Interview he gave to InfoQ [http://www.infoq.com/interviews/David-Hansson](http://www.infoq.com/interviews/David-Hansson) back in 2006.

As part of the merging evaluation process we literally spent hours talking back and forth. I had a seriously hard time believing that Rails and David honestly wanted to change their world views. How can you go from saying what you said in 2006 to adopting what Merb is pushing for: letting the framework bend to make what each developer wants if he doesn't want to follow the "golden path"?

Interestingly enough, David recently addressed this point on his blog. [http://www.loudthinking.com/posts/36-work-on-what-you-use-and-share-the-rest](http://www.loudthinking.com/posts/36-work-on-what-you-use-and-share-the-rest)

"I wanted to take a few minutes to address some concerns of the last 4%. The people who feel like this might not be such a good idea. And in particular, the people who feel like it might not be such a good idea because of various things that I've said or done over the years."

First thing first, David addresses the minority of people worried about his image and what it means for them.
So, David actually cares about hard core merbists and he wants them to join the fun. I personally see this as something very encouraging!

A recurring theme we hear a lot is that Rails becomes whatever DHH/37signals needs/wants. If DHH needs something new, it will make it to Rails, if he doesn't need it, it won't.

David has a very simple and almost shocking response: DHH != Rails

David is really happy with Rails. Rails satisfies his needs but he knows that some people out there need/want something more/different.

"I personally use all of those default choices, so do many other Rails programmers. The vanilla ride is a great one and it'll remain a great one. But that doesn't mean it has to be the only one. There are lots of reasons why someone might want to use Data Mapper or Sequel instead of Active Record. I won't think of them any less because they do. In fact, I believe Rails should have open arms to such alternatives."

So, you can still think whatever you want about David. What's important is that Rails is more than David. It's an entire team of people with different needs and different views.

DHH isn't a dictator and based on concrete examples such as the "respond_to vs provides" discussion, I can reassure you that David has been very receptive to arguments and never tried to force any decision because he thought it was better.
