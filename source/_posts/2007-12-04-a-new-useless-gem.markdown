---
date: '2007-12-04 08:28:00'
layout: post
legacy_url: http://railsontherun.com/2007/12/04/a-new-useless-gem/
slug: a-new-useless-gem
source: railsontherun.com
status: publish
title: a new useless Gem
wordpress_id: '297'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- activation code
- DrNic
- gem
- generator
- lorem ipsum
- newgem
- ruby
- sqlite3
---

During Thanksgiving break I had fun with a friend of mine working on a Ruby challenge while digesting the traditional turkey.





![http://content.screencast.com/media/a088950c-c9d1-4655-9b6e-b917e04dd6ec<em>74569570-772f-4886-b2ea-f305d1ede3aa</em>static<em>0</em>0_00000026.png](http://content.screencast.com/media/a088950c-c9d1-4655-9b6e-b917e04dd6ec_74569570-772f-4886-b2ea-f305d1ede3aa_static_0_0_00000026.png)





The challenge was quite simple, create a small library that can generate random words from the English dictionary. 





But of course there was a twist. One should be able to choose the total amount of characters, the amount of words and the separator between the words. However we both had a [word list](http://wordlist.sourceforge.net/).





I personally decided to use [SQlite3](http://www.sqlite.org/) to store the words after parsing the text file if the database is empty.





It was a good exercise and it got me to play with SQLite and one of the [Ruby adapter library](http://rubyforge.org/projects/sqlite-ruby/). Once I was done, I decide to play with [DrNic](http://drnicwilliams.com/) cool [Gem generator](http://newgem.rubyforge.org/).





![http://static.flickr.com/50/130749539<em>89959dd059</em>t.jpg](http://static.flickr.com/50/130749539_89959dd059_t.jpg)





Nic is my favorite Aussie's Rubyist (ok, I don't know many) and I've been wanting to check on this lib for a very long time. And I have to say he did an awesome job! Writing a Ruby Gem has never that easy! And on top of that the generator creates RSpec examples (or test/unit tests), a basic website for your gem and has a bunch of rake tasks to deploy your newly created gem.





Feel free to check the source code:





[http://random-word-gen.rubyforge.org/svn/](http://random-word-gen.rubyforge.org/svn/)





[And the Rubyforge site](http://rubyforge.org/projects/random-word-gen/)





By the way, I did find an almost useful use for this gem. Activation code generator. You know, the kind of string your receive on by email or SMS to activate a feature or an account. It's always a pain to type a MD5Hash string, while, when using the word generator, the string is made of existing words, making the process way more user friendly.





I also plan on adding some random copyleft text to the sqlite db so the Gem will be able to generate titles, paragraphs and random quotes. I'm just tired of reading [lorem ipsum](http://en.wikipedia.org/wiki/Lorem_ipsum) and on top of that, I get to it, I might had text in various languages so you check if your app breaks when using another char set, or if your layout can't handle too much text.   





Honestly, I don't expect you to use this gem, but I jut wanted to encourage people to start writing their own gem, the process is super easy and rewarding. And actually, feel free to try the challenge and post a link to your implementation in the comments. (That's seriously, the best way to learn)





p.s: I'm sorry about my RSS feed constantly being reset, it seems to be a problem with Mephisto, my blog engine and we are trying to figure out what's going on.
