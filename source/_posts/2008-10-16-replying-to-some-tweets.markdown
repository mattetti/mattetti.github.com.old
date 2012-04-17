---
date: '2008-10-16 22:26:50'
layout: post
legacy_url: http://merbist.com/2008/10/16/replying-to-some-tweets/
slug: replying-to-some-tweets
source: merbist.com
status: publish
title: Replying to some tweets
wordpress_id: '178'
categories:
- News
- merbist.com
- blog-post
tags:
- merb
---

I've been following what people say on [twitter](http://twitter.com) about [Merb](http://search.twitter.com/search?q=merb).

I simply used [twitter search engine](http://search.twitter.com/search?q=merb).

Because I only have a private twitter account I can't reply to everyone.


> freels: [I keep expecting merb to have a view helper for random things...](http://twitter.com/freels/statuses/962887213)


@freels, feel free to let us know about the view helpers you miss. We might not be able to port them all to merb-helpers but we can certainly work something out and maybe come up with more gems so you don't have to repeat yourself.


> 

> 
> [nmeans](http://twitter.com/nmeans): [Ye gods!  The **Merb** meta-gem installs an insane number of packages!](http://twitter.com/nmeans/statuses/961422604)


@nmeans, as you mentioned it, Merb is a meta-gem installing merb-core, merb-more, dm-core, do_sqlite3, dm-timestamps, dm-types, dm-aggregates, dm-migrations, dm-validations, dm-sweatshop + their dependencies. The reason behind this choice is that merb gem is just a package. You can just use merb-core if you wish and that's one gem. Installing many gems isn't a problem, we could even hide it so you don't see it, but you really care how many gems are installed?


> 

> 
> [cfisk](http://twitter.com/cfisk): [Oops, now I've gotten sucked into reading **Merb** stuff on Slideshare.  The whole day got burned up lateralizing over links.](http://twitter.com/cfisk/statuses/960200526)




@cfisk we have videos from MerbCamp coming up next week. The wiki is also growing fast and 2 books are being written.







> 

> 
> [sco](http://twitter.com/sco): [we relaunched packrat today, completely re-written in **merb**. hit a few snags, but still managed to push out 5 million requests in half a day.](http://twitter.com/sco/statuses/960137912)







@sco wow, I didn't know [PackRat](http://www.facebook.com/apps/application.php?id=2431403991) was built on Merb now. Please let us know if you need help.







> 

> 
> [brycethornton](http://twitter.com/brycethornton): Installing the **merb** gem a while back has led to an unimaginable number of related gems.  Wow.  I probably only need a few of these.







@brycethornton you don't have to install the merb gem itself. The Merb gem is a metagem, feel free to only install gems you want. You can generate lighter app by doing merb-gen core app_name or merb-gen flat app_name or a sinatra like app by doing merb-gen very-flat app_name







> 

> 
> [downtowncartel](http://twitter.com/downtowncartel): [We are really excited and proud of the relase of **Merb** 1.0 RC1. Great work to all of those who made it possible.](http://twitter.com/downtowncartel/statuses/959906735)







Thanks, wait for 1.0 and you will enjoy it even more :) Thanks to Ben for merb-cache btw!







> 

> 
> [rafaelbandeira3](http://twitter.com/rafaelbandeira3): **Merb** is like the factory of inovative concepts and concepts usage... ruby is nicier now, although ugly, bad formatted and noisy...







I'm confused, it's probably the first time I hear Ruby us ugly, bad formatted and noisy. I wonder if @rafaelbandeira3 didn't get confused and looked at a different framework/language. Ruby has its pros/cons but ugly/bad formatted and noisy??? really???







> 

> 
> [rarepleasures](http://twitter.com/rarepleasures): [**Merb** is a big fat FAIL](http://twitter.com/rarepleasures/statuses/959580219)







@rarepleasures Ok, this is not really fair because you then [twittered](http://twitter.com/rarepleasures/statuses/960240532) and blamed mongrel. Two things about this tweet tho, mongrel is buggy and you probably should be use thin (or fix Mongrel).Â  If you do think Merb is a big fat FAIL or if there is something you don't like about Merb, please let us know so we can try to fix it.
