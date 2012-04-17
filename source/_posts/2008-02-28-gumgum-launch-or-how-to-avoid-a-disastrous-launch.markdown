---
date: '2008-02-28 08:42:00'
layout: post
legacy_url: http://railsontherun.com/2008/02/28/gumgum-launch-or-how-to-avoid-a-disastrous-launch/
slug: gumgum-launch-or-how-to-avoid-a-disastrous-launch
source: railsontherun.com
status: publish
title: GumGum launch, or how to avoid a disastrous launch.
wordpress_id: '633'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- ec2
- engineyard
- gumgum
- hosting
- joyent
- launch
- rails
- scale
- startup
- techcrunch
---

For the last few months I worked on a Rails app called [GumGum](http://gumgum.com). [GumGum](http://gumgum.com) is a licensing and distribution platform for online content. [GumGum](http://gumgum.com) believes paying a flat rate to license content for online use is illogical. Offline, the flat rate model works because distribution is finite. Online, thanks to Google and other search engines, a story lives forever. [GumGum](http://gumgum.com) has developed two usage models to fairly monetize a license: pay-per-use and ad supported. Pay-per-use allows publishers to license content on a CPM basis. Ad supported subsidizes the licensing revenue through advertisements, providing the publisher a free license.





Simple concept and great team. When Ari Mir and Ophir Tanz approached me to work on their app I was impressed by their understanding of Web Business. It was not their first start up and as a consultant, you usually prefer to work with some experienced clients. Ari, GumGum's CPO was great at defining realistic expectations, and limit the scope of the application which was great since I'm an Agile/XP fanboy. I believe that really helped us meeting our dead line without feature creeping the app.





We were finally ready to enter a public beta phase, we were running our production server on a godaddy VPS (no comment) and were on the process of migrating to [slicehost](http://www.slicehost.com/) where we were running our staging environment. The plan was to go public on Tuesday 12 and have a feel for the load. We also setup an [EC2](http://www.amazon.com/gp/browse.html?node=201590011) image to handle some of the background tasks and were thinking of moving to [Engine Yard](http://engineyard.com) within few months.





Everything was ready for a launch, when the day before D Day, Ari calls me telling me: "Good news Matt, [TechCrunch](http://www.techcrunch.com/) is going to publish a blog post about us!". While he sounded really excited, I couldn't believe me ears. 







  1. The app wasn't optimized, darn it all, I hadn't even properly indexed the database, no caching, nothing. (it was in the backlog, things I was planning on doing after the public beta launch)


  2. We would never be able to handle the load. Running an app like ours definitely needs more than a 256Mb shared CPU shared host.


  3. Ari asked me if they could embed one of our license object in [TechCrunch](http://www.techcrunch.com/) home page.


  4. I had 12 hours to get stuff ready.





# Trying to be ready:





[TechCrunch](http://www.techcrunch.com/) has 688k [RSS subscribers](http://feeds.feedburner.com/Techcrunch)! Even though I asked Ari not to embed one of our protected object inside TC's home page, I knew he couldn't say no to [Mike Arrington](http://www.techcrunch.com/about-michael-arrington/). (little did I know, they would end up embedding 2 licenses)





I hurried up and started properly indexing the database, ask advice to some of my [cabooser](http://blog.caboo.se/) friends, did some quick re-factoring but obviously didn't have enough time to rewrite the way we handle tags and tooltips (the overlay popup window that displays when you rollover a picture in our browse page). [Rob](http://notch8.com) helped me getting ready and we transformed our staging environment into a secondary production server hoping we would split the load.





Since we didn't have a load balancer, we tried to cheat by setting up two A records for gumgum.com. The idea was that browsers would pick an ip randomly and since we were only using 1 database, everything should be fine. That was the theory, during our tests we realized that our Flash object was not using the same IP than the webpage and that was creating random issues with our licensing system.





We were running out of time, I turned on page caching on few actions (home page) so even though my mongrels would die, I knew we would at least be able to display something. Because of our complex authentication system, I couldn't cache much and I didn't have time to re-factor a lot of my code or to setup [memcached](http://www.danga.com/memcached/). (on top of that I knew the product was stable and I didn't want to mess up with it).





# The Storm:





[The article got published](http://www.techcrunch.com/2008/02/13/gumgum-launches-new-image-licensing-platform/) and in seconds we saw the load on the main box going from 0.2 to 2.0! We were still dealing with the DNS issues and some pictures were not getting loaded. When Rails got above 60 req/s we started dropping some requests and returning 500s. That was bad, really bad. Our browse page (not optimized) was down and we handled around 80k requests in a matter of few hours. What you don't know is that each of our flash object generates a couple of requests, and that [TechCrunch](http://techcrunch.com) decided to embed 2 objects on their home page.





Remember, it was our launch, which was not meant to be a big launch, we were not prepared for that!





# Dealing with the load:





The good thing is that my code didn't leak memory, Rails was stable and it was a good sign. We all know that "Rails CAN scale", but we also know that you need to have a proper hosting to do that. Godaddy was not handling the load and it was just the beginning. So what do you do when you have to scale and that your code isn't total junk? You call a real hosting company like [EngineYard](http://engineyard.com) or [Joyent](http://joyent.com). I was lucky [Ezra](http://www.workingwithrails.com/person/5421-ezra-zygmuntowicz) was online. I quickly explained to him the situation, we discussed the reason why our site was not doing so great and defined the bottlenecks. It was simple, we needed more humph. The normal waiting time to get a slice on EY was around 2 weeks back then. Ezra knew that we couldn't wait 2 weeks :p He was simply awesome, managed to find us 2 slices, set them up for us and setup our app in less than 2 hours. He even helped us configuring our old server to get NginX to bounce the requests to EY while the DNS was propagating. 





As soon as the switch was done, everything loaded perfectly, not a single request dropped, it was simply great.





Since we are talking about scaling, hosting and facing issues, I'd like to make a simple point: $349.00 a month for a slice is NOT expensive for what you get. First off, EY slices are setup superbly, you get awesome performances out of them, they come ready for your app and you don't have to do much. Secondly, I'm not a sys admin, I really hate dealing with servers, packages, configuration, compiling crap that won't compile because I'm lame... etc...  

EY has a bunch of experts (both Ruby and Sys admin experts) available 24/7 that always go the extra mile to help you. Last time I checked I couldn't hire a good sys admin for, let's say the price of 3 or 4 slices. Now, we get a team of guys, available, knowing their stuff and helping me for way cheaper than hiring a sys admin dude. The other thing is, I'm a consultant, I don't want to deal with servers and charge my clients for that. I also want them to be reassured and know that "yes, we can scale". The price of an EY/Joyent slice is the price you pay to be confident you can take the load.





# So what now?





We have a lot of optimization to do, we are working on moving more of our stuff to EC2 to avoid being charged for bandwidth twice (user => EY => S3), we are setting up a queuing system and we have a lot of nice features to deploy. But one thing for sure, I can focus on my code since I know EY has my back and I don't have to worry about our servers.





p.s: [GumGum](http://gumgum.com) is doing great, a lot of people are really interested in the product, new features are coming up (we already auto import content from some major Paparazzi agencies) and we are getting a [good press coverage](http://blog.gumgum.com/2008/02/gumgums-press.html)
