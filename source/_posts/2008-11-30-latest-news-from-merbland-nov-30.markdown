---
date: '2008-11-30 15:23:12'
layout: post
slug: latest-news-from-merbland-nov-30
status: publish
title: latest news from Merbland (Nov 30)
wordpress_id: '290'
categories:
- merb
- News
tags:
- merb
- News
- ruby
---

Wow, It's been two weeks since my [last post](http://merbist.com/2008/11/16/merb-news-nov-16-2008/). I'm really sorry about that but at the same time, I really enjoyed [Qcon](http://qconsf.com/sf2008/conference/) and [Thanksgiving](http://en.wikipedia.org/wiki/Thanksgiving) which did not give me much time to work on a blog post.

[caption id="attachment_291" align="alignleft" width="200" caption="Mr Bean vs X-mas turkey"]![Mr Bean vs X-mas turkey](http://merbist.com/wp-content/uploads/2008/11/mr-bean-turkey.jpg)[/caption]

Thanksgiving is a big thing in America, and after few years I got used to it and I even learned to enjoy this special holiday.

For non North American readers, Thanksgiving is a non-religous/family oriented celebration where people get together, eat a lot of food and usually watch an American football game. It's followed by "black-friday" the first day of X-mas shopping celebrated by huge sales and more recently "cyber-monday", the black firday for the web.

Anyways, I've been visiting family in Florida and didn't have much time to work on side projects, including Merb :(

[Qcon](http://qconsf.com/sf2008/conference/) was also an awesome event where I met a lot of very interesting people and could measure the Merb interest in the "Enterprise" community.


## Merb and the enterprise


[37signals](http://37signals.com), the company known for giving Rails to the Ruby community often talked about the "Enterprise" world and the fact that they want to stay far away from it, here is a [quote](http://www.37signals.com/svn/posts/669-why-enterprise-software-sucks) from 37signals blog:


> This is one of the reasons we think enterprise is a dirty word. Itâ€™s also why itâ€™s an absolute pleasure to design products for what we call the Fortune 5,000,000.


On the other hand, Merb was mainly written by people working at [EngineYard](http://engineyard.com), a company dealing with a lot of so call "enterprisey" companies. Because we believe we all have different needs we are willing to help you out with your own needs. That's also the reason why, even though none of the core team members use [NetBeans](http://www.netbeans.org/) for their day to day coding, we decided to help [NetBeans](http://www.netbeans.org/) add Merb support in their next release.



Interesting enough, during the preparation of my talk, I did a simple "hello world" benchmark and got some really interesting results:

![](http://merbist.com/wp-content/uploads/2008/11/benchmarks.png)

Note that Django was tested with mod_python and I was told I could get slightly better results with mod_wsgi.

It was interesting to hear [Tim Bray](http://en.wikipedia.org/wiki/Tim_Bray), known for his implication with XML and ATOM, mention Merb, DataMapper and CouchDB during his keynote!

Actually, the day before, Yehuda and I had supper with Tim Bray and [Nick Sieger](http://blog.nicksieger.com/) from JRuby to discuss few topics but particaluarly how could we improve the framework and of course take advantage of JRuby.


## Merb and the Brazilian community


[![](http://merbist.com/wp-content/uploads/2008/11/beachgirls-229x300.jpg)](http://merbist.com/wp-content/uploads/2008/11/beachgirls.jpg)Brazil is well known for his beautiful beaches, girls and music. It's also known for its soccer team and its BBQ. However it's not really known for its software developers and IT community.

Nevertheless, there was a lot of Brazilians during Qcon and especially a team from [LocaWeb](http://www.locaweb.com.br/), the biggest Brazilian web hosting!

You might not know LocaWeb, but but you might have heard of [Fabio Akita](http://akitaonrails.com/). Fabio interviewed a lot of people for his blog and I guess he was bored so he decided to [interview me](http://akitaonrails.com/2008/11/21/rails-podcast-brasil-qcon-special-john-straw-yellowpages-com-and-matt-aimonetti-merb) ;)

We also sat down for a little while and went through creating a new Merb app, I think Fabio was pretty impressed ;) He was so impressed he mentioned the possibility of aÂ  Merb book in Portuguese!

LocaWeb also mentioned their interest in offering better Merb hosting with the possibilty to have their own stack for people who decide to host their apps there.

[Here](http://merb-br.org/) is the site of the Merb community: [http://merb-br.org/](http://merb-br.org/)


## Rails 2.2


Congrats to the Rails team for releasing Rails 2.2! The new features are:



	
  * i18n

	
  * HTTP validators

	
  * Thread safety

	
  * JRuby/1.9 compatibility


I heard a lot of questions about the new Rails features and where does Merb stand. So let me take them address them one by one:

	
  * **Internationalization.** I, myself was involved with the Rails i18n project so you might be surprise to hear that Merb doesn't have a built-in i18n solution.Â  Well, the fact of the matter is that Merb is modular and we don't want to force anyone to have to use a i18n solution which will slow down your app. Instead we offer modules to do that. Of course, Merb itself offers hooks to l10n helpers and other stuff you might need. By default, Merb doesn't offer UTF-8 string manipulation like ActiveSupport does, however, few months ago, [I extracted AS's feature](http://github.com/mattetti/multibyte) and you can use the [extracted gem](http://github.com/mattetti/multibyte) without the rest of AS while we wait for 1.9 to save the world ;)
Merb makes everything easy for i18n/l10n plugin developers, [merb_babel](http://github.com/mattetti/merb_babel/tree/master) and [merb_global](http://github.com/myabc/merb_global/) are just 2 of the Merb localization plugins, you'll find [many more](http://r18n.rubyforge.org/) and some more are coming up.

	
  * **HTTP validators/etags**. Merb already had this feature for a little while, I put a [quick example in the wiki](http://wiki.merbivore.com/howto/cache/etag), check it out [](http://github.com/wycats/merb/tree/ad378ce413769bc1c3d03aefac02e8e32a5432e4/merb-core/lib/merb-core/controller/mixins/conditional_get.rb) to [see how to use etag and last-modified tags](http://wiki.merbivore.com/howto/cache/etag). Note that you can easy add other custom headers by just doing: headers["[`Cache-Control"] = "max-age=N"`](http://tools.ietf.org/html/rfc2616#section-14.9)

	
  * **Thread safety**. Well, unlike Rails which added thread safety as an after thought, Merb was built with the concept of thread safety in mind. What does that mean? Well, by default, Merb requests don't go through any locks. How? simply because we do not share any data between requests. thread safety is really hard to deal with. However remember that even though your framework is thread safe, your plugins and your code also need to be thread safe. Watch [Jim's talk about threads](http://rubyconf2008.confreaks.com/what-all-rubyist-should-know-about-threads.html) if you want to know more.




## Merb bug fix releases


You might have noticed that Merb is at 1.0.3. We fixed few tiny bugs as well as bumped the generated app dependency to the latest version of DM. (DataMapper's do_sqlite3 had a conflict with ruby-sqlite3 because of the windows dll pacakged with the gem. Everything has been fixed since.)


> Yehuda Katz, explained the release plan in an email to the mailing list:

We plan to release point-releases to 1.0 as fixes become available, so there may be more such releases than in Rails. The goal is to release often enough to keep the list of changes in each release relatively small and understandable, and you can feel free to upgrade to the latest point release every 2 weeks if you don't want to go through the upgrade process every week (or more frequently). Keep an eye out for point releases that reflect security fixes, because those upgrades should be considered mandatory.


To see [1.0.4](http://merb.lighthouseapp.com/projects/7433-merb/tickets/bins/11568) and [1.1](http://merb.lighthouseapp.com/projects/7433-merb/tickets/bins/11569) tickets, check [LightHouse](http://merb.lighthouseapp.com/projects/7433-merb/overview). We are planning on a 1.0.4 release for next week.


## Better documentation


Merb's documentation is getting better and better, here is a selection of few blog posts I think you might want to read. (hopefully all the info are or will be available in the wiki)



	
  * [Testing your controller from atmos.org](http://atmos.org/index.php/2008/11/29/merb-10-controller-testing/)

	
  * [Merb and Glassfish from java.net](http://weblogs.java.net/blog/arungupta/archive/2008/11/totd_53_scaffol.html)

	
  * [Merb Auth from angryamoeba.com](http://singlecell.angryamoeba.co.uk/post/60951656/an-introduction-to-merb-auth-and-the-wonderful-secrets)

	
  * [Merb Mailer from scottmotte.com](http://scottmotte.com/archives/tag/merb-mailer)


