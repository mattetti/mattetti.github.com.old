---
date: '2008-12-24 08:47:00'
layout: post
legacy_url: http://railsontherun.com/2008/12/24/rails-and-merb-core-team-working-together/
slug: rails-and-merb-core-team-working-together
source: railsontherun.com
status: publish
title: Rails and Merb core team working together
wordpress_id: '1709'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- announcement
- merb
- merge
- rails
---

### This is huge!


![](http://merbist.com/wp-content/uploads/2008/12/surprise-296x300.jpg)

While people still try to find some drama an in hypothetical war between rails and merb.

The Rails team and the Merb team announced working together on a joined version of the 2 frameworks. This is so exciting, nobody believed it could ever happen (I personally, had my serious doubt).

Yehuda had a great post laying down the plan and explaining things in details. Check out David’s post explaining why he wants us to work together and his vision of a better Ruby world.

I have to say that I have been impressed by the Rails core team and especially David (DHH).

I’ve known David for few years now and we had long/heated discussions on topics like i18n/l10n for Rails. David is known to be a very opinionated person. But if you come up with the right arguments, he can be convinced and when that happens, he is willing to move forward and shake things up.

This merge is a concrete example that David and the rest of the Rails team cares about Rails and the Ruby community more than we usually give them credit for. As a unified team, we are going to change the way web development in Ruby is done!

But what does it mean for you?

I put together a FAQ video here is the transcript:

Hi, I’m Matt Aimonetti from the merb core team and as you might have heard, a big announcement was made earlier today.

I did this video to hopefully answer the questions you might have.


### Q: So what’s the big news?





	
  * merb and rails team will work together on the next version of their frameworks

	
  * merb 2.0 and rails 3.0 share the same common endpoint

	
  * we realized we now have the same objectives and agreed on all the key principles.

	
  * merb will provide Rails with agnosticism, modularity, improved performance and a public API.

	
  * The end product will be called Rails 3.0 but what real matters is that it’s a huge gain for the entire community.




### Q: What??? I thought there was a war going on between Rails and merb, what happened?





	
  * There was no war between rails and merb

	
  * We created merb because rails was not fitting us

	
  * We wanted better performance, more choices/ more modularity and a Public API.

	
  * The Rails team now embraces these concepts and want Rails to follow them, so why not work together?




### Q: Wait, does that mean that merb is dead?





	
  * Absolutely not!

	
  * merb development won’t stop, we are going to keep on releasing updates until rails 3.0

	
  * clear migration path, and upgrading to rails 3.0 will be as trivial as upgrading from rails 2.x to Rails 3.0




### Q: What does the timeline look like?


We just started getting together and discuss the technical details. We are shooting for a release at RailsConf 2009. However it’s hard to estimate this kind of things so again, that’s just an estimate ![:)](http://merbist.com/wp-includes/images/smilies/icon_smile.gif)


### Q: I just started a merb project, so what now?


I’m sure you had valid reasons to use merb, you needed modularity, performance and a solid API.

Keep on using Merb, we won’t let you down. The good news is that the next version of merb (rails 3.0) will be even awesomer!


### Q: What about my client who was looking at using merb for his new project?


If your client is going to be using merb for valid reasons (and not, just because it’s not rails) he should still use merb, but with the full understanding that he/she will end up using Rails in 6 months or so. Again, Rails 3.0 will have what pushed you to use merb.


### Q: I’ve been involved with the merb-book, what will happen with this project?





	
  * rails 3.0 won’t get released right away

	
  * still need awesome documentation

	
  * if we look at rails 3.0 as merb 2.0, we can easily imagine how the current work can be extended to the new version.

	
  * rails team will also include an evangelism team which I will be part of, so will be able to focus more on projects like the book.




### Q: I’ve been working on a merb plugin, what should I do?


Keep working on it! We’ll assist you with the migration process and the new solid API.


### Q: What if I still have questions?


Come talk with me, or any members from the new team. We’ll be open to hear your questions, worries, frustrations.

merb always valued its developers and we will continue to do so but at a bigger scale.



* * *

Concretely, nothing changes for Merb users. People loving Merb should not worry. The way you build merb apps won’t change until merb2.0/rails3.0. We will still work on your favorite framework and improve it.

Lori Holden worked on merb_sequel and we should release a 1.0 release of the plugin in a few days.

I’m sure this news comes as a shock for many of you, but try to not see Rails 3.0 as the way Rails is right now. Imagine a version of Rails with true modularity and agnosticism (sequel, DM and AR will still be supported) and the same type of performance as what you get with Merb. In other words, the rails world is about to discover the power of merb!

Here is what Yehuda explicitly says in his blog post:



	
  * Rails will become more modular, starting with a rails-core, and including the ability to opt in or out of specific components. [...]

	
  * We will port all of our performance improvements into Rails. This includes architectural decisions that are big performance wins.[..]

	
  * As of Rails 3, Rails will have a defined public API with a test suite for that API. [..]

	
  * Rails will still ship with the “stack” version as the default (just as Merb does since 1.0), but the goal is to make it easy to do with Rails what people do with Merb today.

	
  * Rails will be modified to more easily support DataMapper or Sequel as first-class ORMs. [..]

	
  * Rails will continue their recent embrace of Rack, which [..] will improve the state of modular, sharable logic between applications.

	
  * In general, we will take a look at features in Merb that are not in Rails (the most obvious example is the more robust router) and find a way to bring them into Rails.




## Personal perspective


I’m personally really excited about this opportunity. I had a hard time believing that we could work together but I was proved wrong. We have many challenges in front of us, but watching the two teams working together is very reassuring.

I’m also glad to see that we will have a Rails Evangelism team that I will be part of. I strongly believe that one of the many reasons why merb has been so successful is because we work and listen to our community. We have put a tremendous amount of energy trying to understand what you guys need and what you like and dislike. In return, we saw many people working hard on the wiki and the merb-book.

Can you imagine doing that with almost 4 Million Rails developers?

I’m also looking forward to working with a team and reaching to even more people.


## Other news related to the merge:





	
  * The RubyOnRails website will keep a trace of this historical moment: [http://rubyonrails.org/merb](http://rubyonrails.org/merb)

	
  * The [merb training scheduled for Jan 19-21](http://merbclass.com/) in partnership with [Integrum](http://integrumtech.com/) will still take place, and if you want to get a head start and learn about the things that will make Rails 3.0 totally kick ass, I’d suggest you join us.


If you have any questions, or if you want me to publicly answer questions on your blog please contact me. I’ll do my best to get back to everyone.
