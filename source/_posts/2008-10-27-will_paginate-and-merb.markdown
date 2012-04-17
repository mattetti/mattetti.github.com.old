---
date: '2008-10-27 00:06:41'
layout: post
legacy_url: http://merbist.com/2008/10/27/will_paginate-and-merb/
slug: will_paginate-and-merb
source: merbist.com
status: publish
title: will_paginate and Merb
wordpress_id: '193'
categories:
- News
- merbist.com
- blog-post
tags:
- merb
- pagination
---

[Mislav](http://github.com/mislav) & [Pjhyett](http://github.com/pjhyett)'s [will_paginate plugin](http://github.com/mislav/will_paginate) has been a reference in the Rails world for a little while now. The need for Pagination is arguable, but that's not the point of this post.

[![](http://farm1.static.flickr.com/26/56107224_a4120dd201_m.jpg)](http://flickr.com/photos/jacqueline-w/56107224/)

There are few existing pagination solutions for Merb, but people moving from Rails and porting a Rails app will be happy to hear that will_paginate is now Merb compatible (DataMapper and ActiveRecord supported).

Mislav worked hard making sure that his next release will be framework agnostic and hopes to support both Rails 2.2 and Merb 1.0. You can see where is at by checking on his [agnostic branch](http://github.com/mislav/will_paginate/tree/agnostic).

I did some work over the week end to make sure I could use will_paginate in one of my project. You can see my work [there](http://github.com/mattetti/will_paginate/tree/merb). I also built a [temp gem](http://github.com/mattetti/will_paginate/tree/merb/tmp_release) for you to test WP with Merb until mislav releases the final version of the gem (especially if you are lazy and don't want to deal with git).
