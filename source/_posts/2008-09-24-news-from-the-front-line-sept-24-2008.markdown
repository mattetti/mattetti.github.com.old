---
date: '2008-09-24 08:47:24'
layout: post
legacy_url: http://merbist.com/2008/09/24/news-from-the-front-line-sept-24-2008/
slug: news-from-the-front-line-sept-24-2008
source: merbist.com
status: publish
title: News from the front line - Sept 24 2008
wordpress_id: '36'
categories:
- News
- merbist.com
- blog-post
tags:
- authentication
- bundling
- edge
- head
- merb
- News
- performance
- router
---

Dear Merbivores/Merbists/Merbians,

It's hard to believe that in [less that 20 days](http://merbcamp.com), Merb 1.0 will be released! We are all really happy to to be almost there but we have to be honest and admit that we are also under pressure.

We are all dreaming of a post 1.0 world but in the meantime we have to focus on last minutes bugs and optimization.

[![](http://farm1.static.flickr.com/128/355242291_40cf729cd9_m.jpg)](http://flickr.com/photos/16596714@N00/355242291/)

During the last week or so, we made a lot of progress, the API is now "almost" frozen and [General Katz](http://yehudakatz.com/) is focusing on making sure everything will be fine for D Day.

That reminds me that Katz showed me something amazing yesterday! I shouldn't really talk about it but I'm sure it will stay between us. He was been working on optimizing the general memory consumption and my testing app (real app) went from 120MB of Private Memory used, to 70MB (using 4 processes). I can't wait to use that on the field. I also hope my old Rails comrades will realize that running ~100Mb processes (x4) really isn't efficient and event dangerous for the free Ruby world!

I also heard rumors that the higher officers are now using a new strategic tool called [http://www.pivotaltracker.com](http://www.pivotaltracker.com) which should help us streamline the process. We are still using [LightHouse](http://merb.lighthouseapp.com) to track bugs and patches though. I'm not sure if this new "agile" tool will help, but I thought the approach is pretty interesting. What do you think?

[![](http://farm4.static.flickr.com/3138/2556999427_546c5004f6_m.jpg)](http://flickr.com/photos/celtico/2556999427/)

You probably also saw my early report on [bundling Merb apps](http://merbist.com/2008/09/23/deploying-a-bundled-merb-app-merb-097/), I'm quite happy about the process. Do you think you will deploy bundled/frozen apps or just use the system-wide gems?

[
](http://farm4.static.flickr.com/3097/2884670329_15385e8516.jpg)

Finally I hear a lot of talk about the [new Route](http://github.com/carllerche/merb-core-enterprise-edition/wikis/whats-new-with-the-router)r that's available on [Edge/HEAD](http://github.com/wycats/merb-core/tree/master). Officer Lerch wrote a [nice wiki article](http://github.com/carllerche/merb-core-enterprise-edition/wikis/whats-new-with-the-router) covering the changes, you might want to [read it](http://github.com/carllerche/merb-core-enterprise-edition/wikis/whats-new-with-the-router).

Ohh before I forget, some courageous privates went to [HEAD](http://github.com/wycats/merb-core/tree/master) and use the 3rd party plugin called [merb-auth](http://github.com/hassox/merb-auth/tree/master). What they don't know is that they need to change their routes to use the slices with the new router. (the new router requires no block variable) Also, if they look at the merb-auth branches they will notice a new [mauth branch](http://github.com/hassox/merb-auth/tree/mauth) which is the new version of merb-auth, even better, more flexible than the previous version.

I hope everything is well for you, say Hi! to our friends for me.

- Merbist
