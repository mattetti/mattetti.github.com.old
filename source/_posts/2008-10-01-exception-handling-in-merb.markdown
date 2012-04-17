---
date: '2008-10-01 09:56:02'
layout: post
slug: exception-handling-in-merb
status: publish
title: Exception handling in Merb
wordpress_id: '108'
categories:
- News
tags:
- exceptions
- merb
---


	
  * 

[caption id="" align="alignright" width="224" caption="Mint was originally used as a medicinal herb to treat stomach ache and chest pains. (Something developers eating too much pizza know a bit too well)"][![Mint was originally used as a medicinal herb to treat stomach ache and chest pains. (Something developers eating too much pizza know a bit too well)](http://farm1.static.flickr.com/42/124218131_5c16c08798_m.jpg)](http://flickr.com/photos/kali-ma/124218131/)[/caption]

[Paul Sadauskas aka Rando](http://theamazingrando.com/blog/) posted a [very interesting article](http://www.theamazingrando.com/blog/?p=46) showing how he handles exceptions in his JSON webservices built on top of Merb.

	
  * After discussing with [NewBamboo guys](http://newbamboo.co.uk/) it was decided to move their [merb_exceptions plugin](http://github.com/wycats/merb-plugins/tree/master/merb_exceptions) to [merb-plugins](http://github.com/wycats/merb-plugins/tree/master). The notifier currently supports two interfaces, Email Alerts and Web Hooks. Emails are formatted as plain text and sent using your Merb environment's mail settings. Web hooks as sent as post requests. Railists moving over to this side of the fence will not have to develop their own [exception_notification plugin](http://github.com/rails/exception_notification/tree/master) anymore. Note the NewBamboo team will keep on maintaining the plugin, we just moved it the set of official plugins.

	
  * We got some reports of problems killing workers when using DataMapper and HEAD from yesterday, this is a known problem which should be fixed in the next few days at the latest. (use `merb -K all` to kill your workers instead of ctrl+c)


