---
date: '2008-11-15 19:07:46'
layout: post
slug: rails-vs-merb-drama
status: publish
title: Rails vs Merb Â¿drama?
wordpress_id: '245'
categories:
- merb
- Misc
tags:
- drama
- merb
- philosophy
- rails
---

That's official, according to Twitter the fight has started.

In this corner, weighing in at XXX LOC, from 37signals in the white trunks: **Rails** and in this other corner,Â  weighing in at YYY LOC from Engine Yard: **Merb**!

[caption id="attachment_247" align="alignright" width="150" caption="Yehuda Katz"][![Yehuda Katz](http://merbist.com/wp-content/uploads/2008/11/yehuda-katz-150x150.jpg)](http://merbist.com/wp-content/uploads/2008/11/yehuda-katz.jpg)[/caption]

[caption id="attachment_246" align="alignleft" width="150" caption="David Heinemeier Hansson"][![David Heinemeier Hansson](http://merbist.com/wp-content/uploads/2008/11/dhh-150x150.jpg)](http://merbist.com/wp-content/uploads/2008/11/dhh.jpg)[/caption]


I wish I had a more aggressive photo of Yehuda, but oh well...

So people love drama, from American Idol to the Elections without forgetting Survivor, Heroes and other TV shows. Few weeks ago there was a tentative from Giles to create some controversy but he did not really work so well. The last big drama we had in the Ruby community was Zed and his infamous blog post...


## The plot


Once upon a time, in a world where .NET, JAVA and PHP were ruling the internet development world, a not so well known programming language was getting more and more traction in the far east. Ruby, as it was known in Japan, only had very little documentation in English and real ninjas had to translate docs from _obscure_ Japanese writings in order to become enlightened. Nevertheless, a courageous Danish student named David decided to build an application called [basecamp](http://www.basecamphq.com/) using this "exotic" language. [David](http://en.wikipedia.org/wiki/David_Heinemeier_Hansson) decided to extract a framework from his work for [37signals](http://en.wikipedia.org/wiki/37_signals). He called his framework "[Ruby on Rails](http://rubyonrails.org/)" and offered it to the world under the MIT license. Pretty quickly, as [Ruby](http://www.ruby-lang.org/en/) was getting more popular in the West, people looked at "Rails" and loved the [MVC architecture](http://en.wikipedia.org/wiki/Model-view-controller), the conventions over configuration, testing framework and especially the language: [Ruby](http://www.ruby-lang.org/en/).

As Rails was getting popular, a man from Polish descent known as [Ezra Zygmuntowicz](http://brainspl.at/) and who was working for [Engine Yard](http://www.engineyard.com/) got frustrated with some of Rails limitations. He needed something simpler, easier and faster. In fact he just needed a Mongrel handler, a way to process a request really quickly without going through then entire stack and without slowing down the main app. That's how Ezra started [Merb](http://merbivore.com).

Let's fast forward 2 years. The year is 2008, the place is Orlando, Florida. [Yehuda Katz](http://yehudakatz.com), Merb's lead developer announces the 1.0 release of a Ruby web framework started 2 years earlier by Ezra. The framework's selling points are: performance, modularilty, flexibilty.Â  In the mean time other Ruby frameworks were developed such as [camping](https://code.whytheluckystiff.net/camping/), [Sinatra](http://sinatra.rubyforge.org/), [mack](http://www.mackframework.com/), [waves](http://rubywaves.com/)...




## The plot twist


Now that Merb 1.0 is out, some people want to know what they should use for their next project. Merb or Rails? What's the difference, after all they seem to be doing the same thing. Someone starts a [Reddit thread](http://www.reddit.com/r/ruby/comments/7d5u6/so_why_should_i_choose_merb_over_ruby_on_rails/) asking this very same question to the community.... and the drama begins!



	
  * **part 1:**


Someone mentions that [Merb has less code so it's easier to understand](http://www.reddit.com/r/ruby/comments/7d5u6/so_why_should_i_choose_merb_over_ruby_on_rails/c06ckgv) the "underpinnings of your framework". [Jeremy Kemper aka Bitsweat](http://bitsweat.net/) from Rails core team reply by saying that [Rails code base is only 1/4 bigger than Merb's](http://www.reddit.com/user/bitsweat/).



	
  * **part 2:**


Yehuda Katz, jumps to his shell, runs some commands and [replies that Jeremy](http://www.reddit.com/r/ruby/comments/7d5u6/so_why_should_i_choose_merb_over_ruby_on_rails/c06d0f3) did not count right and really, Merb is half the code size has Rails. He even writes a [blog post to explain what he did](http://yehudakatz.com/2008/11/14/mythbusting-merb-style/) his main point being that Merb can run on subset of his code (merb-core) while Rails can't.



	
  * **part 3:**


At this point, DHH already [started addressing](http://www.loudthinking.com/posts/29-the-rails-myths) some of the reproaches the Merb community has brought on Rails. Interesting enough, DHH carefully never mentions Merb though. However just after wycats/Yehuda's post, DHH releases a post entitled: [Rails is a monolith](http://www.loudthinking.com/posts/33-myth-4-rails-is-a-monolith). In his post, DHH calls LOC comparison an "inverse dick measurement match". He goes even further and gives the size of some of Rails' components, mentions that there is a way in Rails to remove parts you don't want but that anyway "Why would you bother? And that's an answer I [DHH] don't quite have for you."



	
  * **part 4 (I told you it was a real drama)**


Yehuda considers that even though, DHH didn't mention him or Merb, "they" means the Merb community. Unfortunately, DHH doesn't allow comments on his site so Yehuda decides to write yet [another blog post](http://yehudakatz.com/2008/11/15/mythbusting-rails-is-not-a-monolith/). In his post, Yehuda deconstructs DHH arguments and tries to explain the philosphical difference between the two frameworks.


## Drama or no drama?


There were a lot of comments on reddit and yehuda's blog, a lot of tweets were sent, Â but is there a real drama?

My answer is **no**.

The truth is that Merb was developed because some people like Ezra, Yehuda, myself and others were frustrated by some aspects of the Rails framework. We believed in the framework main concept but we were not sharing some of the core values or some of the details of the implementation.

We have nothing but respect for DHH and the Rails core team.** To be honest, without Matz and without DHH, I wouldn't do the job I do today and my job would probably not be as much as what it is**. Most, if not all of the Merb core team members contributed to Rails and are very familiar with its architecture. We just have a different philosophy/approach of problems.

There is also a lot of collaboration between the two teams. Ezra brought rack to Rails, Merb pushed rails to become thread-safe, during RubyConf, [Josh Peek](http://joshpeek.com/) mentioned [porting run_later](http://podcast.rubyonrails.org/programs/1/episodes/rubyconf-2008) (a Merb only feature) to Rails now that it's thread safe. On the other end, we based a lot of our framework on concepts proven by Rails.

**So what is it all about?**

It's just people arguing ideas. It's actually a very healthy thing for a community to do.

I'm not oviously talking about the Rails overall community. I do realize it might shake the Rails community a little bit, but I think it's for the best.

I also hope Sinatra, Waves, camping will join the debate. Sinatra for example is an awesome tiny framework. Merb has flat/very-flat structures which do something similar to Sinatra but with some differences. I think it's great we can talk freely about the philosophies behind each framework.

By putting arguments out there and arguing ideas without fighting, I strongly believe that each framework will just become better and it would be easier for developers to choose what framework fits their needs best.

**No drama then?**

Well, maybe a little bit, let me quote a quote from [37signals book, getting real](http://gettingreal.37signals.com/ch02_Have_an_Enemy.php):


> **Don't follow the leader**
Marketers (and all human beings) are well trained to follow the leader. The natural instinct is to figure out what's working for the competition and then try to outdo it â€” to be cheaper than your competitor who competes on price, or faster than the competitor who competes on speed. The problem is that once a consumer has bought someone else's story and believes that lie, **persuading the consumer to switch is the same as persuading him to admit he was wrong**. And people hate admitting that they're wrong.


â€”[Seth Godin](http://sethgodin.typepad.com/), author/entrepreneur (from [Be a Better Liar](http://www.moveahead1.com/articles/article_details.asp?id=33))

Godin makes a good point, some people feel threatened by a new solution competing with the one they currently use. Godin and 37 signals suggest that instead of competing on the same ground as the leader (in this case Rails), you should position yourself differently and that's what Merb has been doing since theÂ beginning.

That's probably why Yehuda reacted when Jeremy and David said that basically, Merb was no different than Rails.

Merb's point is not to be a faster Rails, instead Merb brings a different approach to the Ruby web development world. And that's the true identity of Merb.


## Conclusion


No need for trolling or making a big deal about what happened. Rails and Merb are both lead by opinionated/passionate people who have a different view on what their framework should do and how it should do it. Â Let's keep the dialogue open but let's not forget that all of us spend hours and hours offering software and the goal is to server the community no to fight.

Both Rails and Merb have pros and cons, learn more about the difference between the two frameworks, look at other solutions and more importantly evaluate your needs.
