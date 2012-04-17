---
date: '2008-10-04 13:00:01'
layout: post
slug: get-on-merb-edge-pre-10
status: publish
title: Get on Merb Edge pre 1.0
wordpress_id: '114'
categories:
- Tutorial
tags:
- datamapper
- edge
- merb
- thor
---

Merb 1.0 is almost ready to be pushed out and you might be impatient to start playing with some of the goodies not yet available in the latest stable release. Before getting started, you should know that not everything has been ironed out yet so don't expect to have a fully stable Edge.

The easiest way to get started requires that you have [git](http://git.or.cz/) installed as well as a gem called [thor](http://github.com/wycats/thor/tree/master).

I let you take care of installing git on your machine, Ruby dev without git became quite challenging since [GitHub](http://github.com) started ruling the Ruby OSS world.

    
    <code class="shell">sudo gem install wycats-thor -s http://gems.github.com</code>


[caption id="" align="alignright" width="240" caption="Hops, used primarily as a flavoring and stability agent in beer, and also in other beverages and in herbal medicine."][![Hops, used primarily as a flavoring and stability agent in beer, and also in other beverages and in herbal medicine.](http://farm2.static.flickr.com/1418/1438235253_5b10c24732_m.jpg)](http://flickr.com/photos/fturmog/1438235253/)[/caption]

[Thor](http://yehudakatz.com/2008/05/12/by-thors-hammer/) is a sort if mix between rake, [sake](http://errtheblog.com/posts/60-sake-bomb) with a better argument parser and based on Ruby classes.

Thor on its own won't be very helpful, we need some thor tasks.

Create a folder where you want to store Merb's source code and cd in it.

Once there download the latest merb thor tasks:

    
    <code class="shell">curl -L http://merbivore.com/merb.thor > merb.thor
    </code>


You can now look at the available task by doing

    
    <code class="shell">thor -T</code>



At this point you be overwhelmed by the multitude of options, we are working on making things a be nicer for 1.0. But anyways, let's get started:

    
    <code class="shell">sudo thor merb:edge --install</code>


The command above will install extlib, merb-core and merb-more from git HEAD. You now have the latest version of Merb's libs installed locally as gems.

You might also want to install Merb extra plugins, if that's the case do:

    
    <code class="shell"> sudo thor merb:edge:plugins --install</code>


If you decided to use DataMapper instead of ActiveRecord or Sequel, you will also need to install the gems you might need:

Install data_objects and an adapter: (replace mysql by the adapter you want to install)

    
    <code class="shell"> sudo thor merb:edge:do mysql --install</code>


Install DataMapper core from Git HEAD

    
    <code class="shell"> sudo thor merb:edge:dm_core --install</code>


Install DataMapper more from Git HEAD

    
    <code class="shell"> sudo thor merb:edge:dm_more --install</code>


You should be all setup by now.

**
Next time you want to update, just come back to the same folder and do the same thing. Hopefully by then we will have a 1 task solution and will offer a merb-stack version of our gems to let you install everything at once with all setup done.**

p.s: this post is dedicated [Derek Neighbors](http://derekneighbors.com/) ;)
