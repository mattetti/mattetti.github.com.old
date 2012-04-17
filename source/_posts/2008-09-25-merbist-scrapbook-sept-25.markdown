---
date: '2008-09-25 08:34:17'
layout: post
slug: merbist-scrapbook-sept-25
status: publish
title: merbist scrapbook (sept-25)
wordpress_id: '50'
categories:
- News
tags:
- caching
- datamapper
- merb
- News
- ruby
---

[caption id="" align="alignright" width="182" caption="Fennel, one of the primary ingredients of absinthe helps developers' creativity."][![Fennel, one of the primary ingredients of absinthe helps developers creativity.](http://farm4.static.flickr.com/3220/2818914854_1a1f01b3a3_m.jpg)](http://www.flickr.com/photos/marty_2007/2818914854/)[/caption]



	
  * [Good news](http://merbunity.com/news/42) regarding Merb slices: the new router allows you to mount a slice directly as: /the-slice-action (previously it had to be /the-slice/something)

	
  * On IRC, Nagash came up with an [interesing snippet](http://gist.github.com/12755) allowing you to easily serve static views (like Django's generic views) (Merb::Template.template_for is PRIVATE so use it at your own risk) The Core team is investigating simpler/cleaner ways of achieving the same result and a built-in solution should be available in Merb 1.1

	
  * Merb 0.9.8 will be optimized for [Ruby Enterprise Edition](http://www.rubyenterpriseedition.com/) and will be supporting a [new way](http://github.com/fabien/minigems/tree/master) of handling gems without wasting so much memory. (more news coming up soon)

	
  * Merb 0.9.8 has a new efficient way of dealing with clusters (more news coming soon)

	
  * [Merb's new caching system](http://merbunity.com/tutorials/15) will make it to 1.0

	
  * Merb is going into a feature freeze and the team will focus solely on bug fixes and making the Merb experience more pleasant.

	
  * A turn key deployment solution is planned for Merb 1.x (deployment recipes plugin)

	
  * [DataMapper](http://datamapper.org/) [benchmarks show it's now way faster](http://gist.github.com/10735) than [ActiveRecord](http://ar.rubyonrails.com/) (on average) Benchmark scripts available in the dm-core repo.


