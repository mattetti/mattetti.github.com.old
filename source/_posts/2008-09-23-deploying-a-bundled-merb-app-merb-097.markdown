---
date: '2008-09-23 15:32:41'
layout: post
legacy_url: http://merbist.com/2008/09/23/deploying-a-bundled-merb-app-merb-097/
slug: deploying-a-bundled-merb-app-merb-097
source: merbist.com
status: publish
title: Deploying a bundled merb app (merb 0.9.7+)
wordpress_id: '23'
categories:
- Tutorial
- merbist.com
- blog-post
tags:
- bundle
- capistrano
- deployment
- freezer
- merb
- ruby
---

Since Merb 0.9.7 the Merb team decided to change the way you can bundle an app. Until 0.9.7 you would use the merb-freezer plugin which was supporting git submodules and gems. The only problem was that you still had to install merb-freezer on your server and it had to stay in sync with your app... kinda lame :(

[caption id="" align="alignleft" width="240" caption="Ginger roots, great for deployment issues"][![Ginger](http://farm3.static.flickr.com/2373/1987820964_bc54df0d81_m.jpg)](http://flickr.com/photos/vieuxbandit/1987820964/)[/caption]

Instead, after a lot of discussions, we decided to add this feature to merb-core and let you bundle all your dependencies in a bundled gem folder. No more support for git submodules as they are hard to keep track of and don't handle dependencies very well (not at all).

The new freezing strategy is very well described in [this merbunity article](http://merbunity.com/tutorials/18). However it doesn't really explain how to deploy a bundled app.

So let's imagine for a second that we bundle our app, generated the scripts needed to start merb/rake etc...



The key thing to do is to have a decent deployment recipe. Let's look at my cap recipe:



As you can see there are few key elements:



	
  * recompile native gems for the web server

	
  * call bin/merb and not merb directly


One thing you need to be aware of is that your bundled gems need to be up to date and play well alone. When you test your bundled app locally, you might endup loading system wide gems available which you don't have on your deployment server.

If when you deploy your app everything seems fine but you can't access your app, check that you are recompiling native gems during your deployment and make sure you have bundled all the deps you need and that they work well alone.

One more advise, if you can't figure out what's going, try ssh'ing to your server, go to your current folder and try bin/merb -iÂ  to see what's going on.
