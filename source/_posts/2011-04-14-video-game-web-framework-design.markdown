---
date: '2011-04-14 10:28:39'
layout: post
slug: video-game-web-framework-design
status: publish
title: Video game web framework design
wordpress_id: '1010'
categories:
- blog-post
- Ruby
- Software Design
tags:
- design
- frameworks
- video games
---

In this post I will do my best to explain why and how I reinvented the wheel and wrote a custom web framework for some of Sony's [AAA console titles](http://www.gameproducer.net/2006/05/26/what-are-aaa-titles/). My goal is to reflect on my work by walking you through the design process and some of the implementation decisions. This is not about being right or being wrong, it's about designing a technical solution to solve concrete business challenges.


## Problem Domain


The video game industry is quite special, to say the least. It shares a lot of similarities with the movie industry. The big difference is that  the movie industry hasn't evolved as quickly as the video game  industry has. But the concept is the same, someone comes up with a great  idea, finds a team/studio to develop the game and finds a publisher. The  development length and budget depends on the type of game, but for a AAA  console game, it usually takes a least a [few million](http://www.joystiq.com/2010/03/09/god-of-war-3-has-44-million-dollar-budget/) and a minimum of a year of work once the project has received the green light. The creation of such a game involves various teams, designers, artists, animators, audio teams, developers, producers, QA, marketing, management/overhead etc.. Once the game gets released, players purchase the whole game for a one time fee and the studio moves  on to their next game. Of course things are not that simple, with the latest platforms, we now have the option to patch games, add [DLC](http://en.wikipedia.org/wiki/Downloadable_content) etc.. But historically, a console game is considered done when it ships, exactly like a movie, and very little work is scheduled post release.

Concretely such an approach exposes a few challenges when trying to implement online features for a [AAA console title](http://www.gameproducer.net/2006/05/26/what-are-aaa-titles/):



	
  * Communication with the game client network team

	
  * Scalability, performance

	
  * Insane deadlines, unstable design (constant change of requirements)

	
  * Can't afford to keep on working on the system once released (time delimited projects)





## Communication


As in most situations, communication is one of the biggest challenges. Communication is even harder in the video game industry since you have so many teams and experts involved. Each team speaks its own jargon, has its own expertise and its own deadlines. But all focus on the same goal: releasing the best game ever. The team I'm part of has implementing online features as its goal. That's the way we bring business value to our titles. Concretely, that means that we provide the game client developers with a C++ SDK which connects to custom web APIs written in Ruby. The API implementations rely on various data stores ([MySQL](http://www.mysql.com/), [Redis](http://redis.io/), [Memcached](http://memcached.org/), memory) to store and retrieve all sorts of game data.

Nobody but our team should care about the implementation details, after all, the whole point of providing an API is to provide a simple interface so others can do their part of the job in the easiest way possible. This is exactly where communication becomes a problem. The design of these APIs should be the result of the work of two teams with two different domains of expertise and different concerns. One team focuses on client performance, memory optimization and making the online resources available to the game engine without affecting the game play. The other, focuses on server performance, latency, scalability, data storage and system contention under load. Both groups have to come together to find a compromise making each other's job doable. Unfortunately, things are not that simple and game designers (who are usually not technical people) have a hard time _not_ changing their designs and requirements every other week (usually for good reasons) making API design challenging and creating tension between the teams.

From this perspective, the API is the most important deliverable for our team and it should communicate the design goal while being very explicit about how it works, why it works the way it does, and how to implement it client side. This is a very good place where we can improve communication by making sure that we focus on making clear, well designed, well documented, flexible APIs.




## Scalability, performance


On the server side, the APIs need to perform and scale to handles tends of thousands of concurrent requests. Web developers often rely on aggressive [HTTP caching](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html) but in our case, the web client (our SDK) has a limited amount of memory available and 90% of the requests are user specific (can't use full page HTTP cache) and a lot of these are POST/DELETE requests (can't be cached). That means that, to scale, we have to focus on what most developers don't often have to worry too much about: all the small details which, put together with a high load, end up drastically affecting your performance.

While Ruby is a great language, a lot of the libraries and frameworks are not optimized for performance, at least not the type of performance needed for our use case. However, the good news is that this is easily fixable and many alternatives exist (lots of async, non-blocking drivers for i.e). When obsessed with performance, you quickly learn to properly load test, profile, and monitor your code to find the bottlenecks and the places where you should focus your attention. The big, unique challenge though, is that a console game will more than likely see its peak traffic in the first few weeks, not really giving the chance to the online team to iteratively handle the prod issues. The only solution is to do everything possible before going live to ensure that the system will perform as expected. Of course if we were to write the same services in a more performant language, we would need to spend less time optimizing. But we are gaining so much flexibility by using a higher level programming language that, in my mind, the trade off is totally worth it (plus you still need to spend a lot of time optimizing your code path, even if your code is written in a very fast language).




## Deadlines, requirement changes


That's just part of the way the industry works. Unless you work for [Blizzard](http://blizzard.com) and you can afford to spend a crazy amount of time and money on the development of a title; you will have to deal with sliding deadlines, requirement changes, scope changes etc... The only way I know how to protect myself from such things is to plan for the worst. Being a non-idealistic (read pessimistic) person helps a lot. When you design your software, make sure your design is sound but flexible enough to handle any major change that you know could happen at any time. Pick your battles and make sure your assumptions are properly thought through, communicated and documented so others understand and accept them. In a nutshell, this is a problem we can't avoid, so you need to embrace it.




## Limited reusability


This topic has a lot to do with the previous paragraph. Because scopes can change often and because the deadlines are often crazy, a lot of the time, engineers don't take the time to think about reusability. They slap some code together, pray to the [lords of Kobol](http://en.wikipedia.org/wiki/Lords_of_Kobol) and hope that they won't have to look at their code ever again (I'm guilty of having done that too). The result is a lot of throw away code. This is actually quite frequent and normal in our industry. But it doesn't mean that it the right thing to do! The assumption/myth is that each game is different and therefore two games can't be using the same tech solution. My take on that is that it's partly true. But some components are the same for 80% of the games I work on. So why not design them well and reuse the common parts? (A lot of games share the same engines, such as [Unreal](http://www.unrealengine.com/) for example, and there is no reason why we can't build a core online engine extended for each title)




## My approach


When I joined Sony, I had limited experience with the console video game industry and my experience was not even related to online gaming. So even though I had (strong) opinions (and was often quite (perhaps even too) vocal about them), I did my best to improve existing components and work with the existing system. During that time, the team shipped 4 AAA titles on the existing system. As we were going through the game cycles, I did my best to understand the problem domain, the reasons behind some of the design decisions and finally I looked at what could be done differently to improve our business value. After releasing a title with some serious technical difficulties, I spent some time analyzing and listing the problems we had and their root causes. I asked our senior director for a mission statement and we got the team together to define the desiderata/objectives of our base technology. Here is what we came up with:



	
  1. Stability

	
  2. Performance / Scalability

	
  3. Encapsulation / Modularity

	
  4. Documentation

	
  5. Conventions

	
  6. Reusability / Maintainability


These objectives are meant to help us objectively evaluate two options. The legacy solution was based on Rails, or more accurately: Rails was used in the legacy solution. Rails had been hacked in so many different ways that it was really hard to update anything without breaking random parts of the framework. The way to do basic things kept being changed, there was no consistent design, no entry points, no conventions and each new game would duplicate the source code of the previously released game and make the game specific changes. Patches were hard to back port and older titles were often not patched up. The performance was atrocious under load, mainly due to hacked-up Rails not performing well. (Rails was allocating so many objects per request that the GC was taking a huge amount of the request cycles, the default XML builder also created a ton load of objects etc...) This was your typical [broken windows scenario](http://en.wikipedia.org/wiki/Broken_windows_theory). Engineers were getting frustrated, motivation was fainting, bugs were piling up and nobody felt ownership over the tech.

Now, to be fair, it is important to explain that the legacy system was hacked up together due to lack of time, lack of resources and a lot of pressure to release something ASAP. So, while the end result sounds bad, the context is very important to note. This is quite common in software engineering and when you get there, the goal is not to point fingers but to identify the good and the bad parts of the original solution. You then use this info to decide what to do: fix the existing system or rewrite, porting the good parts.

Our report also came up with a plan. A plan to redesign our technology stack to match the desiderata previously mentioned. To put it simply, the plan was to write a new custom web framework focusing on stability, performance, modularity and documentation. Now, there are frameworks out there which already do that or value these principles. But none of them focus on web APIs and none of them are specific to game development. Finally, the other issue was that we had invested a lot of time on game specific code and we couldn't throw away all that work, so the new framework had to support a good chunk of legacy code but had to make it run much faster.


## Design choices


**Low conversion cost**

Using [node.js](http://nodejs.org/) & [coffee script](http://jashkenas.github.com/coffee-script/)/[Scala](http://www.scala-lang.org/)/[whatever](http://weblocks.viridian-project.de/) [new](http://code.google.com/p/v8cgi/) [fancy tech](https://github.com/tenderlove/phuby) was not really an option. We have a bunch of games out there which are running on the old system and some of these games will have a sequel or a game close enough that we could reuse part of the work. We don't want to have to rewrite the existing code. I therefore made sure that we could reuse 90% of the business logic by adding an abstraction layer doing the heavy lifting at boot time and therefore not affecting the runtime performance. Simple conversion scripts were also written to import the core of the existing code over.

_Lessons learned:_ It is very tempting to just redo everything and start from scratch. However, the business logic implementation wasn't the main cause of our problems. Even though I wish we could have redesigned that piece of the puzzle, it didn't make sense from a business perspective. A lot of thought had to be put into how to obtain the expected performance level while keeping the optional model/controller/view combos. By having full control of the "web engine", we managed to isolate things properly without breaking the old paradigms. We also got rid of a lot of assumptions allowing us to design new titles a bit differently while being backward compatible and have our code run dramatically faster.

**Web API centric**

This is probably the most important design element. If I had to summarize what our system does in just a few words, I would say: a game web API. Of course, it's much more than that. We have admin interfaces, producer dashboards, community websites, lobbies, p2p, BI reports, async processing jobs etc... But at the end of the day, the only one piece you can't remove is the game web API. So I really wanted the design to focus on that aspect. When a developer starts implementing a new online game feature, I want him/her to think about the API. But I also want this API to be extremely well documented so the developer working client-side understands the purpose of the API, how to use it, and what the expected response is right away. I also wanted to be able to automatically test our APIs at a very basic level so we could validate that there are discrepancies between what the client expects and what the server provides. To do that, I created a standalone API DSL with everything needed to describe your API but without any implementation details whatsoever. The API DSL lets the developer define a route (url), the HTTP verb expected, if the request should be authenticated or not, SSL or not, the param rules, default values and finally a response description (which was quite a controversial choice). All of these settings can be documented by the developer. This standalone DSL can then be consumed by different tools. For instance we have a tool extracting all the info into nicely formatted HTML doc for the game client developers. This tool doesn't need to load the framework to just render the documentation. We also use this description at boot time to compile the validation rules and routes, allowing for a much faster request dispatch. And we also use these API description to generate some low level data for the client. Finally, we used the service description DSL to help create mocked service responses allowing the client team to test service designs without having to wait for the implementation streamlining the process.

_Lessons learned:_ We had a lot of internal discussions about the need to define the response within the service description. Some argued that it's a duplication since we already had a view and we could parse that to get most of what we needed (which is what the old system was doing). We ended up going with the response description DSL for a few critical reasons: testing and implementation simplicity. _Testing:_ we need to have an API expectation reference and to keep this reference sane so we can see if something is changed. If we were to magically parse the response, we couldn't test the view part of the code against a frame of reference. _Implementation simplicity_: magically parsing a view template is more tricky that it sounds, you would need to render the template with the right data to make it work properly. Furthermore, you can't document a response easily in the view, and if you do, you arguably break the separation of concern between the description and the implementation. Finally, generated documentation isn't enough and that's why we decided to write English documentation, some being close to the code and some being just good old documentation explaining things outside of the code context.

**Modularity**

In order to make our code reusable we had to isolate each component and limit the dependencies. We wrote a very simple extension layer allowing each extension to registers itself once detected. The extension interface exposes the path of the extension, its type, models, services, controllers, migrations, seed data, dependencies etc.. Each extension is contained in a folder. (The extension location doesn't matter much but as part of the framework boot sequence, we check a few default places.) The second step of the process is to check a manifest/config file that is specific to each title. The manifest file lists the extensions that should be activated for the title. The framework then activates the marked extensions and has access to libs, models, views, migrations, seed data and of course to load services (DSL mentioned earlier) etc...

Even though we designed the core extensions the best we could, there are cases where some titles will need to extend these extensions. To do that, we added a bunch of hooks that could be implemented on the title side if needed (Ruby makes that super easy and clean to do!). A good example of that is the login sequence or the player data.

_Lessons learned:_ The challenge with modularity is to keep things simple and highly performing yet flexible. A key element to manage that is to stay as consistent as possible. Don't implement hooks three different ways, try to keep method signatures consistent, keep it simple and organized.




## Conclusion


It's a bit early to say if this rewrite is a success or not and there are still lots of optimizations and technology improvements we are looking forward to doing. Only time will give us enough retrospect to evaluate our work. But because we defined the business value (mission statement) and the technical objectives, it is safe to say that the new framework meets the expectations quite well. On an early benchmark we noted a 10X speed improvement and that's before drilling into the performance optimizations such as making all the calls non-blocking, using better connection pools, cache write through layer... However, there is still one thing that we will have to monitor: how much business value will this framework generate. And I guess that's where we failed to define an agreed upon evaluation grid. I presume that if our developers spend more time designing and implementing APIs and less time debugging that could be considered business value. If we spend less time maintaining or fighting with the game engine, that would also be a win. Finally, if the player experience is improved we will be able to definitely say that we made the right choice.

To conclude, I'd like to highlight my main short coming: I failed to define metrics that would help us evaluate the real business value added to our products. What I consider a technical success might not be a business success. How do you, in your own domain, find ways to define clear and objective metrics?
