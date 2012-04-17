---
date: '2009-04-14 09:02:45'
layout: post
slug: merb-11-delayed
status: publish
title: Merb 1.1 delayed
wordpress_id: '472'
categories:
- Misc
---

We made the decision to slightly delay the release of Merb 1.1 as we ended up changing the scope of what we wanted to make available in the 1.1 release. If you have been following our releases, you know that this is not something we usually do, but we strongly believe that this is actually something that will save us time for the next release.

The big themes for 1.1 are full Ruby 1.9 support and Rails3 compatibilty: action-orm(previously called active-orm) and the new router.

While on one hand, Ruby 1.9 work is 99% done (we still have a couple of failing specs with action-args) and action-orm just needs to be merged in, on the other hand, the new router currently does more than what we initially planned for. It actually covers stuff we scheduled for 1.2.

Here is a quick preview of what Carl has been working on:

Merb's router is now extracted into a rack middleware library and a bunch of features to try to get "mountable apps" working in Merb 1.1 have been added.

The proof of concept has been submitted to the Rack development mailing list and the draft is available at:
[http://github.com/carllerche/rack-router](http://github.com/carllerche/rack-router/tree/master)

Merb, CloudKit, Sinatra and more than likely Rails3 should be using this new rack based router. This is a huge step for the Ruby community!

Here is the abstract explained by carl:


> Conceptually, rack-router allows you to create a two way map between HTTP requests and Rack applications. It is built as a piece of middleware that takes in a set of routes.
When a request comes in, the router will compare that request against the set of routes until it finds one that matches. It then calls the associated rack app.
It can also generate URL's that you can use to link to other mountable apps.
It also goes quite a bit further and attempts to make reusing rack applications completely painless (what we are tentatively calling "mountable apps").


For more information, check the [mailing list thread](http://groups.google.com/group/rack-devel/browse_thread/thread/41334bce83cb173f).

Now that the proof of concept has been accepted, the new implementation needs to be optimized to match the speed of the previous router. Currently the new router is pretty slow compared to 1.x router.
