---
date: '2008-04-10 08:10:00'
layout: post
legacy_url: http://railsontherun.com/2008/04/10/rails-or-merb-what-s-best-for-you/
slug: rails-or-merb-what-s-best-for-you
source: railsontherun.com
status: publish
title: Rails or Merb, what's best for you?
wordpress_id: '872'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- benchmark
- merb
- rails
---

![](http://brainspl.at/louiecon.gif)





If you follow my blog, you already know what [Merb](http://merbivore.com) is.





I love Rails and I truly believe it has changed web development. At least it has changed the way I do web development. 





But Merb looks slick, apparently is way faster than Rails, and has less "fluff" and less magic. 





Now that we are getting really close to a Merb 1.0 (scheduled for Rails Conf '08) it's time to evaluate if Merb is the good choice for some of my clients' projects.





However, according to Merb's author, Ezra, at MountainWest RubyConf 2008, [Rails will get you there faster](http://mwrc2008.confreaks.com/02zygmuntowicz.html). In a client's case, they don't need to build a huge app but need a lot of speed and the ability to easily handle a heavy load right away without using caching. Also most of the traffic will go through an API so we won't have to manage too many views.





## Let's see how fast Merb really is.





To test Merb's speed, I built the very same prototype using Merb 0.9.2 and Rails Edge (pre 2.1). Both apps use ActiveRecord and are connected to a UTF8 MySQL database, both apps have exactly the same views. (Note that Merb would run way faster using DataMapper, but I don't feel that DM 0.9x is production ready yet, also, using a rack handler would certainly be way faster but my goal was really to compare ActionPack vs Merb.)





Both apps use the same ActiveRecord class, their controllers are a bit different but basically do the same thing.





Here is what was tested:







  * The Merb/Rails app should receive a GET request with a JSON object in the query. 



  * The Merb/Rails app should route the request to a controller and pass the JSON object to an AR class.



  * The AR class should parse the JSON object (which contains an array of objects), extract each object, and try to find them in the database using one of the attributes. If the object isn't found, it should be created, otherwise it should return the AR object. The amount of hits should be incremented by 1 and the object should be saved back to the database.



  * A simple HTML view should be rendered






## Quick Merb benchmark





![merb](http://merbivore.com/img/header_logo.png)





I setup Merb to run locally on my MacBook 2.16Ghz Core Duo 2, 2Gb Ram. To test the raw performance, Merb is started in production mode.





I then used httperf to make 10000 connections to the server at a rate of 500 (--rate=500 --send-buffer=4096 --recv-buffer=16384 --num-conns=10000 --num-calls=1)





Here are the results:




    
    <code>Maximum connect burst length: 29
    Total: connections 4377 requests 4221 replies 2932 test-duration 41.629 s
    Connection rate: 105.1 conn/s (9.5 ms/conn, <=1022 concurrent connections)
    Connection time [ms]: min 41.0 avg 1920.4 max 35390.8 median 898.5 stddev 4887.3
    Connection time [ms]: connect 2118.1
    Connection length [replies/conn]: 1.000
    
    Request rate: 101.4 req/s (9.9 ms/req)
    Request size [B]: 321.0
    
    *Reply rate [replies/s]: min 0.0 avg 73.3 max 143.0 stddev 65.8 (8 samples)*
    Reply time [ms]: response 809.0 transfer 18.1
    Reply size [B]: header 121.0 content 557.0 footer 0.0 (total 678.0)
    *Reply status: 1xx=0 2xx=2932 3xx=0 4xx=0 5xx=0*
    
    CPU time [s]: user 0.35 system 36.54 (user 0.8% system 87.8% total 88.6%)
    Net I/O: 78.4 KB/s (0.6*10^6 bps)
    
    Errors: total 7068 client-timo 0 socket-timo 0 connrefused 0 connreset 1445
    Errors: fd-unavail 5623 addrunavail 0 ftab-full 0 other 0
    </code>





What we care about is the reply rate/s. We have an average of _73.3 requests per second_ with a standard deviation of 65.8 using 8 samples.





We also make sure that all the replies were successful. (status == 2xx)





I also checked the database, made sure my AR object was created and that the hits were increased. AR object hits: 2932, which matches the amount of replies reported by httperf.





We don't care so much about the rest of the httperf. Let's move on to the Rails benchmark.





* * *





## Quick Rails benchmark





![rails](http://www.rubyonrails.org/images/rails.png)





Rails is set the same way, running locally in production mode, same httperf settings.





Here are the results:




    
    <code>Maximum connect burst length: 44
    
    Total: connections 2923 requests 2825 replies 1672 test-duration 37.418 s
    
    Connection rate: 78.1 conn/s (12.8 ms/conn, <=1022 concurrent connections)
    Connection time [ms]: min 382.7 avg 5635.4 max 36384.5 median 1887.5 stddev 10103.1
    Connection time [ms]: connect 3631.2
    Connection length [replies/conn]: 1.000
    
    Request rate: 75.5 req/s (13.2 ms/req)
    Request size [B]: 319.0
    
    *Reply rate [replies/s]: min 0.0 avg 43.4 max 75.2 stddev 30.8 (7 samples)*
    Reply time [ms]: response 1568.1 transfer 36.7
    Reply size [B]: header 471.0 content 581.0 footer 0.0 (total 1052.0)
    *Reply status: 1xx=0 2xx=1672 3xx=0 4xx=0 5xx=0*
    
    CPU time [s]: user 0.25 system 31.31 (user 0.7% system 83.7% total 84.4%)
    Net I/O: 69.4 KB/s (0.6*10^6 bps)
    
    DB hits: 1672
    </code>





First thing, the database object was created properly and the hits incremented to 1672 which matches the amount of replies reported by httperf.





Then, we notice that on this test, we only got 7 samples, that's more than enough though. The standard deviation is 30.8 which is better than Merb's 65.8. That means that in our benchmarks, the reply speed difference in Merb's requests was bigger than Rails'. Not a big deal, this is not a scientific test but it's good to acknowledge it.





What we really care about is the average reply rate: 43.4





Let's also note that all the replies had a 2xx status, so everything went well.





## Results





Based on this really basic benchmark, my Merb app had an average reply rate of _73.3 requests per second_ against Rails' _43.4 requests per second_.





That means that in this very specific case, 





## _Merb is 69% faster than Rails_!  Sexy! 





In other words, my Merb prototype could handle 69% more requests than the Rails prototype in the same amount of time.





I heard people reporting than Merb was 3 to 5 times faster than Rails. Honestly, it really depends on what you do. By using ActiveRecord on both prototypes, I limited the speed difference since AR is not multithread and therefore Merb can't run as fast as it would using Sequel or DataMapper. By actually hitting the database on every single request, I also made sure to really compare ActionPack vs Merb.





## Conclusion





The conclusion is simple, I recommended that my client go with Merb. Merb 1.0 is almost ready, the public API has been frozen. My client needs speed and simplicity. Using Merb I get exactly what I need and nothing more. Actually, we'll probably increase the performance by writing a rack handler and bypassing the entire framework for API calls (that should be wicked fast!).  Also, as soon as DataMapper becomes production ready, we'll switch to DM and should get way better performance!





Am I suggesting to give up Rails and switch to Merb?
Absolutely not!  First off, Merb is a "lower level" framework. It requires a deeper understanding of Web Development in general and being more than just 'acquainted' with the Ruby language. So, unless you are an advanced developer or have time to learn, I would suggest to keep on using Rails (start using Merb on personal projects, it's a perfect way of learning).
If you have a lot of views and/or use loads of AJAX, RJS, built-in helpers, you probably want to stick to Rails and start looking at how you can do all of that from scratch. By default Rails uses nasty helpers that create inline javascript, and is something you really want to avoid. RJS is fun, but it goes against Merb's philosophy, so you need to make sure you can live without it (note that you can reproduce the same behavior in Merb rendering JS, it just requries more work). If you rely a lot of Rails plugins, you might want to delay your switch, Merb is pretty new and doesn't have a mass-load of plugins yet.





Finally, Merb doesn't have a lot of documentation and changed a lot when 0.9 got released. To understand how Merb works, you will need to go through the source code, specs, Google, and ask on the Merb IRC channel.





It turns out that in our case we have experienced developers, a great need for speed, not too many views and are following Merb's development really closely . I honestly think it's the best choice for my client and I'm excited they accepted to use Merb.





Merb is addressing different issues than Rails and doing it well. I think there is a bright future for Merb. And don't even think that Rails is going away, that won't happen anytime soon! 





Recently, [Sony Playstation](http://www.us.playstation.com/Corporate/About) even posted a [job post](https://www2.recruitingcenter.net/Clients/playstation/PublicJobs/controller.cfm?jbaction=JobProfile&Job_Id=11424&esid=az) looking for a Rails/Merb developer. This is very promising for the Merb community!
