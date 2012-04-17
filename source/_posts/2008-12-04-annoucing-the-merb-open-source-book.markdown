---
date: '2008-12-04 23:18:31'
layout: post
slug: annoucing-the-merb-open-source-book
status: publish
title: Annoucing the Merb open source book
wordpress_id: '315'
categories:
- merb
- News
tags:
- book
- django
- merb
- merb book
- symfony
---

![Matt Aimonetti compares the search growth between Rails and Merb](http://merbist.com/wp-content/uploads/2008/12/google-insights-worldwide-jul-dec-2008.jpg)


Here is a graph representing the growth of of web searches related to the programming category. This is not a comparison of the amount of searches made.

We can see a huge peak around the time Merb 1.0 got released. More and more people are looking for information about what's already being called the "Ruby web framework for the enterprise". This is obviously a very interesting and encouraging trend. The problem is that unfortunately, the reality is that when you search for Merb documentation you don't get very useful results.

Worse still, the main complaint we get is the lack of end user documentation. No fancy screensavers, no demo applications, just a simple, well organized but not expansive [wiki](http://wiki.merbivore.com). Don't get me wrong, I'm really proud of our wiki. The wiki was restarted from scratch during the 1.0 RC era and we have some really decent documentation including a step by step BDD example with code on GitHub. The problem is that wikis, in general, are not easy to read when you want to learn a new technology. But they are great when you look for answers to a specific question, like: how do I use authentication, how do use transaction specs with RSpec...

The Ruby community is used to not having great open source documentation and to purchasing books written by experts. I remember when I joined the Ruby community, the company I was working for was looking at three frameworks: one written in PHP, the other in Python and the last one in Ruby. The Python framework was the fastest but didn't have a testing harness. The PHP framework had great documentation but was PHP (no comment).Â  And the Ruby framework almost did not have any documentation but had great momentum. It also had a book released which kind of compensated for the lack of documentation. (I don't think we would have picked the Ruby framework if all the engineers did not fall in love with the language :p)

Merb has 4 books in the process of being written, 2 of them already available in Beta. This is really exciting but it doesn't answer the need of two types of audiences:



	
  * People who decide to give Merb a try and don't have much time/patience/money ;)

	
  * People outside of the English speaking world (yes, even though according to Hollywood, even aliens speak English, the reality is that a lot of people prefer to read documentation in their native language)


Looking at other frameworks, a lot of the well known ones offer free books. [Django](http://www.djangoproject.com/) has its famous [Django Book](http://djangobook.com/), [Symfony](http://www.symfony-project.org) has its [book](http://www.symfony-project.org/book/1_2/)...

To address the needs of people who want to get started quickly, we probably need to write some great tutorials. I think we will soon focus on that. People have already started writing awesome tutorials on getting started with glassfish and Merb and other cool things. However, most of these tutorials are designed for people who already have some Ruby knowledge. So the core team will probably need to help the community get a bit more organized and write some good simple tutorials for total newbies. However this is not really hard to do.

The real challenge for me is to reach the foreign audience. As you have probably noticed by the amount of English mistakes I make, I'm not a native English speaker. I remember learning English by asking my mom to translate computer messages I was getting when using my cousin's [Amstrad CPC](http://en.wikipedia.org/wiki/Amstrad_CPC) and my dad's [Amstrad PC 1512.](http://en.wikipedia.org/wiki/PC-1512) The first English words I remember learning were: "Game Over" and "insert the floppy disk" :)

![](http://farm1.static.flickr.com/69/206957476_e74acf006e_m.jpg)Talking with Brazilian Ruby evangelist Fabio Akita about his books, he reminded me how hard it is to get up to date IT documentation in your mother tongue. People talk about Globalization but in a time were knowledge is power, people outside of the US are still getting IT books one to two years after their US release.

PDF books make things a bit easier but you still need to be able to understand and of course you need to hope the online shop will accept your "foreign" card.


## Merb book


The Merb team has decided to support the documentation effort. At first we thought about updating Matthew Ford's [Merb 4 ninja book](http://merb.4ninjas.org/). But the project was started when Merb 0.3.x was released and needed a huge amount of work to be brought up to date.

After talking with Matthew, we decided it was probably better to start a new project backed by the core team. I quickly put together a simple localized Merb app rendering markdown files using awesome [maruku](http://maruku.rubyforge.org/).

The app is simple to use, simple for editors to contribute content, simple for translators to do their job.

We gathered three Merb book authors: Yehuda Katz, Foy Savas and Matthew Ford and worked on an initial [table of contents](http://github.com/mattetti/merb-book/tree/master/book-content/en/toc.markdown). At the same time, I got in contact with Fabio Akita, Mathieu Fosse and Makoto Kuwata to see if they were interested in leading the translation in Portuguese, French and Japanese. Everybody got really excited and wanted to start as soon as possible. Here is a short list of goals for the book:



	
  * From the community, for the community

	
  * Creative Commons share alike type copyright

	
  * Simple, organized and up to date documentation of the Merb stack

	
  * Focus on the common use cases

	
  * Early localization to allow centralized, up to date documentation in various languages

	
  * Export to static HTML and PDF


The initial work is available in my [GitHub repo](http://github.com/mattetti/merb-book/tree/master) and we are planning on publishing an updated version of the book daily. The book will be tagged at the same time as Merb releases, to allow people to go back in time and check the book for a previous version of the framework.

If you are interested in helping, it's very simple:

add/modify content:



	
  * clone the book repo

	
  * modify/add content

	
  * rebase to avoid conflicts

	
  * send me a pull request


translate content:

	
  * Find out who the translator leader is for your language (check the readme file in the repo)

	
  * clone the book repo

	
  * add translations

	
  * send the leader your pull request


If there is no translator leader for your language, contact me.

I really hope to be able to add Spanish, German and Chinese pretty soon. If you are willing to lead the translation work for these languages, please contact me via email (mattaimonetti at Gmail or via github).

Join the mailing list if you want to get the latest news or discuss the content.








![Google Groups](http://groups.google.com/groups/img/3nb/groups_bar.gif)






**Subscribe to the merb book mailing list **






Email:






[Visit this group](http://groups.google.com/group/merb-book)




Finally, Yehuda has been working on another very simple Merb app to let you find available methods and their documentation in context. RDoc is fine, but how do you know what methods are available when you are in a view, and what about in a Model? Once again, the concept is to make your life as developer easier. For a long time we have focused on the framework itself. It's now time we focused on making it easy to use so more people can enjoy the power and flexibility of Merb.

Of course, all of this is done for you. So if you have any comments, concerns, advise, feel free to contact us directly or leave a comment.
