---
date: '2011-01-18 08:00:42'
layout: post
slug: causality-of-scalability
status: publish
title: Causality of scalability
wordpress_id: '852'
categories:
- blog-post
- Software Design
tags:
- architecture
- cloud
- design
- scalability
---

Part of my job at [Sony PlayStation](http://us.playstation.com/) is to architect scalable systems which can handle a horde of excited players eager to be the first to play the latest awesome game and who would play for 14-24 hours straight. In other words, I need to make sure a system can "scale". In my case, a scalable system is a system that can go from a few hundred concurrent users/players to hundreds of thousands of concurrent users/players and stay stable for months.

One can achieve scalability in many ways, and if you expect me to provide you with a magical formula you will be disappointed. I actually believe that you can scale almost anything if you have the adequate resources. So saying that X or Y doesn't scale is for me a sign that people are taking shortcuts in their explanations (X or Y are really hard to scale so they don't scale) or that they don't understand the causality of scaling. However what I am exploring in this post is the relationship between cause and effect when trying to make a system scalable. We will see that the scalability challenge is not new and not exclusive to the tech world. We will study the traditional approach to scaling and as well as the challenge of scaling in relation to the web and what to be aware of when planning to make a solution scalable.


## Scaling outside of the tech world


Trying to scale isn't new. It goes back to well before technology was invented. Scaling something up or increasing something in size or number is a goal businesses have aimed for ever since the oldest profession in the world was invented. ![](http://upload.wikimedia.org/wikipedia/commons/thumb/4/49/EN_BESKYTTERINDE_AF_INDUSTRIEN.gif/180px-EN_BESKYTTERINDE_AF_INDUSTRIEN.gif)A prostitute wanting to scale up her business was limited by her own time and body. She would reach a point where she couldn't take more clients. (Independent contractors surely know what I am talking about!) So a prostitute wanting to scale up would usually become a madam/Mama-san and scale the business by having girls work for her.

Another simple example would be a restaurant. A restaurant can handle up to a certain amount of covers/clients at once, after that, customers have to wait in line. The restaurant example is interesting because you can clearly see that opening a huge restaurant with a capacity of 1,000 covers might not be a good idea. First because the cost of running such a restaurant might be much more than the income generated. But also because even though the restaurant does 1,000 covers at peak time, it doesn't mean that the restaurant will stay that busy during the entire time it's open. So now you have to deal with waiter/waitresses, busboys and other staff who won't have anything to do. As you probably have understood already, scaling a restaurant means that the scaling has to be done in a cost effective manner.  And what's even more interesting is that what we could have thought was the bottleneck (the amount of concurrent covers) can be easily scaled up but it wouldn't provide real scalability. In fact this choice would cascade into other areas of management like staffing and the building size. Often, the scaling solution for restaurants is to open new locations which can result in keeping the lines shorter, targeting new markets and reducing risks since one failing branch won't dramatically affect the others.


![posted by Matt Aimonetti](http://upload.wikimedia.org/wikipedia/en/thumb/4/4a/Nighthawks.jpg/400px-Nighthawks.jpg)





## Scaling in the traditional tech world


If you've ever done console development or worked on embedded devices, you know that they are restricted by some key elements. ![posted by Matt Aimonetti](http://upload.wikimedia.org/wikipedia/commons/thumb/d/d3/PS3Versions.png/220px-PS3Versions.png)It can be memory, CPU, hard drive space etc... You have to "cram" as many features as you can into the device, working around the fixed limitations of the hardware. In the console industry, what's interesting to note is that the hardware doesn't change often but people expect than a new game on the same platform will do things better than the previous game, even though the limitations are exactly the same. This is quite a challenging problem because you have to fight against the hardware limitations by optimizing your code to be super efficient. That's exactly the reason why console video game developers manage memory manually instead of relying on a garbage collector. This way they can squeeze every resource they can from the console.

The great advantage of this type of development is that you can reproduce and accurately anticipate issues. The bottlenecks/limitations are well known and immutable! If you find a way around in your lab, you know that the solution will work for everyone. Console video game developers (and to some extent, iOS developers) don't have to wonder how their game will behave if the player has an old graphic card or not enough RAM.

But ever since we started distributing the processing power, scaling technology has become more challenging.


## Scaling on the web


Scaling a web based solution might actually seem quite like scaling a restaurant, except that you can't easily open multiple locations since the concept of proximity in web browsing isn't really as concrete as in real life. So the solution can't be directly transposed. Most people will only have to scale up by optimizing their code running on one server, or maybe two. That's because their service/app is not, and won't be, generating high traffic. Scaling such systems is common and one can rely on work done in the past decades for good examples of solutions.

However some web apps/games are or will become high traffic. But because every single entrepreneur I've met believes that their solution will be high traffic, they think they need to be able to scale and therefore should be engineered like that from the beginning. (This is, by the way, the reason scalability is a buzzword and you can sell almost anything technical saying that it scales.) The problem with this approach is that people want scalability but don't understand its causality. In other words, they don't understand the relationship between cause and effect related to making a solution scalable.

Basically we can reduce the concept of causality of scalability to something like this: you change a piece of the architecture to handle more traffic, but this part has an effect on other parts that also need to change and the pursuit of scalability almost never ends (just ask Google). **Making a system scalable needs to have well a defined cause and expected effect, otherwise it's a waste**. In other words, the effect of scaling engenders the need for solutions which themselves have complex effects on a lot of aspects of a system. Let's make it clearer by looking at a simple example:

[![Simple Architecture by Matt Aimonetti](http://merbist.com/wp-content/uploads/2011/01/simplestarchitecture-300x269.png)](http://merbist.com/2011/01/18/causality-of-scalability/simplestarchitecture/)We have an e-commerce website and this website uses a web application with a database to store products and transactions. Your system is made of 1 webserver handling the requests and one database storing the data. Everything goes well until Black Friday, Christmas or Mother's Day arrives and now some customers are complaining that they can't access your website or that it's too slow. This is also sometimes referred to as the digg/slashdot/reddit effect. All of a sudden you have a peak of traffic and your website can't handle it. This is actually a very simple use case, but that's also the only use case most people on the web need to worry about.

The causality of wanting this solution to scale is simple, you want to scale so you can sell more and have happy customers. The effect is that the system needs to become more complex.

To scale such a system, you need to find the root cause of the problem. You might have a few issues, but start by focusing on the main one. In this case, it's more than likely that your webserver (frontend) cannot handle more than x requests/second. Interestingly enough, the amount of reqs/s might not match the result of your load tests. That's probably because you didn't expect the usage pattern that you are seeing, but that's a whole different topic. At this point you need to understand why you can't go above the x reqs/s limit you're hitting. Where is the bottleneck? Is it that your application code is too slow? Is it the database has been brought to its knees? Or maybe the webserver serves as many requests as technically possible but it's still not enough based on the traffic you are getting.

If we stop right here, we can see that the reasons why the solution doesn't scale can be multiple. But what's even more interesting is that the root cause this time depends on the usage pattern and that it is really hard to anticipate all patterns. If we wanted to make this system scale we could do it different ways.

To give you some canned answers, if the bottleneck is that your code is too slow, you should check if the code is slow because of the DB queries made (too many, slow queries etc..). Is it slow because you are doing something complex that can't be easily improved or is it because you are relying on solutions that are known to not support concurrent traffic easily? More than likely, you will end up going for the easy caching approach. By caching some data (full responses, chunk of data, partial responses etc..) you avoid hitting your application layer and therefore can handle more traffic.

[caption id="attachment_879" align="aligncenter" width="300" caption="Caching avoids data processing & DB access "][![More complex Architecture by Matt Aimonetti](http://merbist.com/wp-content/uploads/2011/01/simplestarchitecture3-300x201.png)](http://merbist.com/2011/01/18/causality-of-scalability/simplestarchitecture3/)[/caption]

If your code is as fast as it can be, then a solution is to add more application servers or to async some processes. But now that means that you need to change the topology of your system, the way you deploy code and the way you route traffic. You will also increase the load on the database by opening more connections and maybe the database will now becoming the new bottleneck. You might also start seeing race conditions and you are certainly increasing the maintenance and complexity aka cost of your system (caching might end up having the same effect depending on the caching solution chosen).

[caption id="attachment_876" align="aligncenter" width="297" caption="One way of scaling it to load balance the traffic"][![load balanced approach by Matt Aimonetti](http://merbist.com/wp-content/uploads/2011/01/simplestarchitecture2-297x300.png)](http://merbist.com/2011/01/18/causality-of-scalability/simplestarchitecture2/)[/caption]

Just looking at these possible causes and the various solutions (we didn't even mention DB replication, sharding, NoSQL etc..), we can clearly see that making a system scalable has some concrete effects on system complexity/maintenance which directly translate in cost increase.

If you are an engineer, you obviously want your system to be super scalable and handle millions of requests per second. But if you are a business person, you want to be realistic and evaluate the causality of not scaling after a certain point and convert that as loss. Then you weigh the cost of not scaling with the cost of "maybe" scaling and you make a decision.

The problem here though is that scaling is a bit like another buzzword: SEO (Search Engine Optimization). A lot of people/solutions will promise scaling capabilities without really understanding the big picture. Simple systems can easily scale up using simple solutions but only up to a certain level. After that, what you need to do to scale becomes so complex than anyone promising you the moon probably doesn't know what they are talking about. If there was a one-size-fits-all, easy solution for scaling, we would all be using it, from your brother for his blog, to Google without forgetting Amazon.

![AWS logo posted by Matt Aimonetti](http://awsmedia.s3.amazonaws.com/logo_aws.gif)Speaking of Amazon, I hear a lot of people saying that Amazon AWS services is "THE WAY" (i.e: the only way) to scale your applications. I agree that it's a compelling solution for a lot of cases but it's far from being a silver bullet. Remember that the cause and effect of why you need to scale are probably different than anyone else.


## Amazon Web Services


Let me give you a very concrete example of where [AWS services](http://aws.amazon.com/) might_ not_ be a good idea: high traffic sites with lots of database writes and low latency.

[Zynga](http://zynga.com), the famous social game company behind Farmville, Mafia Wars etc., is using AWS and it seems that they might have found themselves in the same scenario as above![](http://www.zynga.com/img/logo.png). And that would be almost correct. Zynga games have huge traffic and they do a ton of DB writes. However I don't think they need low latency since their game clients are browsers and Flash clients and that their games are mainly async so they just need to be able to handle unstable latency. We'll see in a second how they manage to perform on the AWS cloud.

The major problem with AWS when you have a high traffic site is IO: IO reliability, IO latency, IO availability. By IO, I'm referring to network connection (internal/external) and disk access. Put differently, when you design your system and you know you are going to run on AWS, you need to take into consideration that your solution should survive with zero or limited IO because you will more than likely be IO bound. This means that your traditional design won't work because your database hard drive won't be available for 30s or will be totally saturated. You also need to have a super redundant system because you are going to randomly lose machines. Point number one, moving your existing application from a dedicated hosting solution to AWS might not help you scale if you didn't architect to be resilient to bad IO. Simply put, and to only pick one example: if you were expecting your database to be able to always properly write to disk you will have problems.

[caption id="" align="alignleft" width="184" caption="Octocat, the GitHub mascot"]![](http://tctechcrunch.files.wordpress.com/2010/07/github-logo.png)[/caption]

The solution depends on how you want to look at it and where you are at in your project. You can go the [Zynga route and design/redesign your entire architecture to be highly redundant, not rely on disk access](http://highscalability.com/blog/2010/2/8/how-farmville-scales-to-harvest-75-million-players-a-month.html) (everything is kept in memory and flushed to disk when available) and tolerate a certain % of data loss. Or you can go with the[ GitHub approach](https://github.com/blog/493-github-is-moving-to-rackspace)[ and mix dedicated hardware for IO and "cloud" front end servers all on the same network](https://github.com/blog/493-github-is-moving-to-rackspace). One solution isn't better than the other, they are just different and depend on your needs. [GitHub](http://github.com) and [Zynga](http://www.zynga.com/) both need to scale but they have different requirements.

When it comes to scaling, things are not black or white. To stay on the AWS topic, let's take another example: Amazon Relational Database Service (RDS). Earlier today, I was complaining on Twitter that RDS doesn't and probably won't let you use the [MySQL HandlerSocket plugin](http://yoshinorimatsunobu.blogspot.com/2010/10/using-mysql-as-nosql-story-for.html) any time soon, even though it's been released for almost 6 months and used in prod by many. Then someone asked me if using this plugin would offset the scalability cost-saving. The quick and wrong answer  is yes. By using the plugin, you can potentially get rid of your Memcached servers, probably your Redis/MongoDB/CouchDB servers or whatever NoSQL solution you write and just keep the database servers you currently have. You might have to beef up your DB servers a bit but it would certainly be a huge cost reduction and your system would be simpler, easier to maintain and the data would be more consistent. Sounds good right? After all the biggest online social game company designed it and uses it.

The only problem is that RDS is an AWS service and like every AWS service, it suffers from poor IO. So, if you were deciding to not use RDS and run your own MySQL servers with the HandlerSocket plugin, it wouldn't bring you much improvement _(1)_. Actually, if you are already IO bound, it would make things worse, because you are centralizing your system around the most unreliable part of your architecture. Based on that premise, RDS won't support HandlerSocket because RDS runs on the same AWS architecture and has to deal with the same IO constraints. What's the solution, you might ask? Amazon already went through these scaling problems and they offer a custom, non-relational, data storage solution working around their own issues called SimpleDB. But why would they improve RDS and fix a really hard problem when they already offer an alternative solution? Easy. SimpleDB forces you to redesign your architecture to work with their custom solution and, guess what? You are now locked-in to that vendor!

So the answer is yes, you can offset scalability costs if you don't use AWS or any other providers with bad IO. Now you should look at the cost of moving away from AWS and see if it's worth it. How much of your code and of your system is vendor specific? Is that something you can easily change? The [fog library](https://github.com/geemus/fog), for instance, supports multiple cloud providers. Are you using something similar? Can you transition to that?  Can you easily deploy to another hosting company? ([Opscode chef](http://opscode.com/chef) makes that task much easier) But if, for one reason or another, you have to stick with AWS/<other cloud provider>, make sure that the business people in charge understand the consequences and the cost related to that choice.


## Conclusion


My point is not to tell you to not design a scalable solution, or not to use AWS, or that RDS sucks. My point is to show that making a system scale is hard and has some drastic effects that are not always obvious. There aren't any silver bullet solutions and you need to be really careful about the consequences (and costs) involved with trying to scale. Make sure it's worth it and you have a plan. Define measurable goals for your scalability even though it's really hard, don't try to scale to infinity and beyond, that won't work. Having to redesign later on to handle even more traffic, is a good problem to have, don't over engineer.

Finally, be careful to understand the consequences of your decisions. What seems to be an almost trivial scaling move such as moving your app from dedicated hosting to a specific cloud provider might end up getting you in a vendor lock in situation!



* * *

_1: I assume that you are IO bound. If you are not and your DB data fits in memory/cache, then HS on AWS is fine but if that's the case what's your bottleneck? ;)_
