---
date: '2007-06-01 03:53:00'
layout: post
legacy_url: http://railsontherun.com/2007/06/01/globalite-has-moved/
slug: globalite-has-moved
source: railsontherun.com
status: publish
title: Globalite has moved
wordpress_id: '29'
categories:
- Ruby
- railsontherun.com
- blog-post
---

I’ve been struggling with finding a good place to host my Globalite Rails plugin. After trying [rubyForge](http://rubyforge.org/projects/globalite) and then hosting the plugin [myself](http://svn.aimonetti.net/public/plugins/globalite/trunk/)





I finally decided to move the project to [Google code](http://code.google.com/p/globalite/)





install the plugin:




    
    <code>script/plugin install http://globalite.googlecode.com/svn/trunk/
    </code>





rename the vendor/plugins/trunk => vendor/plugins/globalite or from your vendor/plugins folder:




    
    <code>svn checkout http://globalite.googlecode.com/svn/trunk/ globalite
    </code>





Feel free to report issues [there](http://code.google.com/p/globalite/issues/list) or to submit patches.
