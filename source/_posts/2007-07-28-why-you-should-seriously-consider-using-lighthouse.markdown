---
date: '2007-07-28 23:55:00'
layout: post
legacy_url: http://railsontherun.com/2007/07/28/why-you-should-seriously-consider-using-lighthouse/
slug: why-you-should-seriously-consider-using-lighthouse
source: railsontherun.com
status: publish
title: Lighthouse
wordpress_id: '74'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- activereload
- agile
- basecamp
- bug
- bug tracking
- lighthouse
- project
- project management
- subversion
- svn
- tools
---

[![lighthouse logo](http://farm2.static.flickr.com/1290/931895653_92ab9b4282_o.jpg)](http://www.lighthouseapp.com/) is one of my favorite web application of the moment. It got publicly released last April (2007) by the [activereload](http://activereload.net/) ninjas.  





Since then, they also released [![warehouse](http://farm2.static.flickr.com/1338/932922548_576ef7fb11_m.jpg)](http://warehouseapp.com/)  

but I'll keep that for another post.





I've been using Lighthouse for a little while and I have to admit that I was a bit confused at first. I'd like to share with you **why I'm really pleased with [Lighthouse](http://www.lighthouseapp.com/)** and **why I think you should consider using it too**.





You have probably figured out, I'm a little biased. So let me give you my conclusions right away.







  * [Lighthouse](http://www.lighthouseapp.com/) is **simple** and **efficient**.


  * [Lighthouse](http://www.lighthouseapp.com/) is **more than a bug tracker**, it's also a **project management application**.


  * [Lighthouse](http://www.lighthouseapp.com/) helps you **communicating better** with you clients.


  * [Lighthouse](http://www.lighthouseapp.com/) **doesn't lack any features** I used in [trac](http://trac.edgewall.org).


  * [Lighthouse](http://www.lighthouseapp.com/) helps you **keeping it simple**.


  * [Lighthouse](http://www.lighthouseapp.com/) has a **nice UI**.


  * [Lighthouse](http://www.lighthouseapp.com/) is really cheap (it's not free but almost).





The 2 things that threw me off when I switched from [trac](http://trac.edgewall.org) to [Lighthouse](http://www.lighthouseapp.com/) were:







  * the lack of **integration with Subversion**


  * the weird way of **organizing tickets**





**SVN integration:**





So, no [Lighthouse](http://www.lighthouseapp.com/) doesn't have a repository browser, you would need to use [warehouse](http://warehouseapp.com/), [trac](http://trac.edgewall.org), [retrospectiva](http://retrospectiva.org/blog) or another tool. I personally don't think there's a real need for such a tool, but others might disagree.  However, Lighthouse has a subversion beacon(Beacons are services that can be used to interact with Lighthouse). You can really easily install the beacon on your subversion server and committing code will leave a comment in your ticket, add a tag, change the status etc.. check [this video](http://lighthouseapp.com/tour/source-control-integration) to see how it works. That's something I never did with Trac and I spent a lot of time editing tickets to mention the changesets related to issue related in the ticket.





**SVN integration conclusion: better than expected**  





**Organizing tickets:**





I like having my tickets well organized. When i switched to Lighhouse and I only had tags to organize my tickets, I felt a bit lost. Actually Lighthouse doesn't only have tags, it also has milestones. My milestones are usually really simple: iteration 1, iteration 2, iteration 3, investor demo 1, iteration 4, beta etc... Milestones aren't used to classify tickets but to give them a deadline. So I was still facing an issue since I wanted to organize and prioritize all my user story tickets, bug reports, feature requests etc... using tag is 'ok' but finding your tickets later on is a pain. I decided to do something I've been doing with trac for a while: adding a keyword in the topic line. For instance I would create a new ticket like that **[bug] a user can't use utf-8 characters in his name**. It worked ok but I was still missing something. What I didn't know was that Rick and Justin already thought about that and came up with a simple/cool solution: [ticket bins](http://lighthouseapp.com/tour/ticket-bins). Basically, a ticket is just a pool of tickets sharing the same criteria. Creating a ticket bin is dead simple, search for tickets, save the search criteria. That's it.





![ticket bins](http://farm2.static.flickr.com/1280/933132130_839046bcd3_o.jpg)  





I could now start using my critical, essential, nonessential tags and create bins for them. What I love with bins is that they are not _folders_. A ticket can be inside many bins. For instance, my critical UI open bug will be in my current bugs bin, critical bugs bin and in my UI bin. (I don't actually have a current bugs bin, but you see my point.) 





User stories/bug reports/new features can all be entered via the same interface and you don't have to worry since they can be easily sorted using your ticket bins.





**Organizing tickets conclusion: awesome**





* * *





**What about [Basecamp](http://basecamphq.com/)?**





[Basecamp](http://basecamphq.com/) is great but even DHH says it is not meant for managing development projects. Itâ€™s really meant for marketers and managers, in other words: my clients. But you still need a good way of tracking your code, [Lighthouse](http://www.lighthouseapp.com/) isn't perfect and I wish it had a better support of user stories but it's by far the best tool I found so far. Regarding the lack of user stories/criteria support, maybe I'll just write my own application and use their beacon interface to integrate both apps together...
