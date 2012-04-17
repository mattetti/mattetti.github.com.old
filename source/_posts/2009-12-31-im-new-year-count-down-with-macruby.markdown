---
date: '2009-12-31 10:25:50'
layout: post
slug: im-new-year-count-down-with-macruby
status: publish
title: IM new year countdown with MacRuby
wordpress_id: '662'
categories:
- blog-post
- macruby
tags:
- ichat
- macruby
- scriptingbridge
---

Here is the geekiest way I found to wish Happy New Year to my IM contacts:

![](http://img.skitch.com/20091231-c9t2n9889rxiinux6qsfhqtxfq.jpg)

    
    framework 'ScriptingBridge'
    app = SBApplication.applicationWithBundleIdentifier("com.apple.iChat")
    original_status = app.statusMessage
    new_year = Time.mktime(2010, 1, 1, 0, 0)
    
    loop do
      now = Time.now
      time_left = (new_year - now).ceil
      if time_left > 0
        app.statusMessage = "#{time_left} seconds left until 2010 (EST)"
      else
        app.statusMessage = "Happy New Year 2010!"
        exit
      end
      sleep(1)
    end


If you are alone at home playing WOW,  you can also trigger iTunes to play a mp3 file with crowd noise and people shouting '**Happy New Year 2010**'!

![](http://img.skitch.com/20091231-c5j9dhhy46t76gh26fu6a26h7c.jpg)
