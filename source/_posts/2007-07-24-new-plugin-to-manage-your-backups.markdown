---
date: '2007-07-24 06:18:00'
layout: post
legacy_url: http://railsontherun.com/2007/07/24/new-plugin-to-manage-your-backups/
slug: new-plugin-to-manage-your-backups
source: railsontherun.com
status: publish
title: new plugin to manage your backups
wordpress_id: '70'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- active record
- ar
- backup
- database
- plugin
- restore
- sake
---

I just released a simple plugin that I use to backup and restore databases instead of using a mysql dump.





[check out the plugin page](http://code.google.com/p/ar-backup/)





File structure will look like that:





![ar-backup](http://farm2.static.flickr.com/1054/882951034_343db15424_o.jpg)





I use this plugin to backup my apps running in production. When I need to debug an application, I just need to restore the db locally and debug. The fact that it uses fixtures helps me finding data quickly and I can easily decide to only restore few tables. 





I also have a Capistrano recipe to create backups before each deployment.





oohh and the plugin is [Sake](http://errtheblog.com/post/6069) compatible, give it a try :)





Enjoy
