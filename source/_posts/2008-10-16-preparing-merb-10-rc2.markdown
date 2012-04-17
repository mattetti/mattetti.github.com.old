---
date: '2008-10-16 11:15:27'
layout: post
slug: preparing-merb-10-rc2
status: publish
title: Preparing Merb 1.0 RC2
wordpress_id: '172'
categories:
- News
tags:
- merb
- rc
- wiki
---

[1.0RC1 was released](http://merbist.com/2008/10/13/merb-10-rc1/) less than a week ago and we are already planning to release RC2 next week or so.

Thanks to all the contributors who reported RC1 bugs and submitted patches. RC2 should iron out most of the small bugs and annoyances encountered.

[caption id="" align="alignright" width="180" caption="Rosemary has a very old reputation for improving memory."][![Rosemary has a very old reputation for improving memory.](http://farm1.static.flickr.com/56/133095382_75068e1a32_m.jpg)](http://flickr.com/photos/geishaboy500/133095382/)[/caption]

However, RC2 top priority is to get Merb and DataMapper properly working on Windows. Some members of the .net community offered their help to compile windows native DM drivers. Supporting Windows is not an easy task but we think it is a critical thing to do.

We are also discussing setting up a nightly gem server so people could test Merb edge without having to use git etc..Â  It's also a great way for us to test gem dependencies before an official release. The only problem with nightly gems is that you need to have a way to track what version is being installed or what version a user reporting a bug is using. We do have some ideas, but please feel free to add a comment with your suggestions.

On a different note, the [new wiki](http://wiki.merbivore.com/) is growing nicely, but we do need more content, as you are experimenting with Merb 1.0RC, please think about sharing your knowledge with others by using the [wiki](http://wiki.merbivore.com/).

Finally, people on Edge are reporting issue with gems not loading properly. The problem is that you have generated an app with hardcoded dependencies (check your config/dependencies.rb file). Your app won't load newer gems and therefore migth crash. Make sure to define the proper dependencies. We are working onÂ  documenting this behavior and making the error message clearer.
