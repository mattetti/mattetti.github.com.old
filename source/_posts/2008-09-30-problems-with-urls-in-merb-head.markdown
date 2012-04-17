---
date: '2008-09-30 22:24:29'
layout: post
legacy_url: http://merbist.com/2008/09/30/problems-with-urls-in-merb-head/
slug: problems-with-urls-in-merb-head
source: merbist.com
status: publish
title: problems with urls in Merb HEAD?
wordpress_id: '101'
categories:
- Misc
- Tutorial
- merbist.com
- blog-post
tags:
- merb
- merb-assets
- router
---

I actually run into a small problem when updated an older Merb app. Here was how my router looked like:

    
    Merb::Router.prepare do |r|
      r.resources :channels do |channels|
        channels.resources :shows do |shows|
          shows.resources :episodes
        end
      end
    end


But after updating to the latest version of Merb, I got links looking like:

    
    http://localhost:4000/channels/#<Channel:0x27b7300>/shows


The first thing to do is to read [Carl's wiki about the latest Router changes](http://github.com/carllerche/merb-core-enterprise-edition/wikis/whats-new-with-the-router).

Carl explains that things got cleaned up in the router code and my routes should now look like:

    
    Merb::Router.prepare do |r|
      r.resources :channels do
        resources :shows do |shows|
          resources :episodes
        end
      end
    end


However that won't be enough..  You see my url used to look like that:

    
    url(:channel_shows, :channel_id => channel)


Now I can simplify it to:

    
    url(:channel_shows, channel)


That still won't fix the problem, since the real problem comes from the fact that I was on Merb HEAD but not DataMapper HEAD. Updating DM clears things up. That's the price to pay to be on HEAD ;)

FYI the problem comes from the fact that DM doesn't add a to_params method to its objects. Rails users might recognize that method used to convert an object into a string to create a route, something not really ORM agnostic and frowned upon by the DM/Merb teams.

Merb lets you specify the param to use for your routes using the identify method. Read [Carl's wiki page ](http://github.com/carllerche/merb-core-enterprise-edition/wikis/whats-new-with-the-router)for more cool stuff and see how to create some cool stuff like url slugs etc..

Note that even if you are using ActiveRecord, you'll need to update merb_activerecord as the new identify rules were updated in the ORM plugins.
