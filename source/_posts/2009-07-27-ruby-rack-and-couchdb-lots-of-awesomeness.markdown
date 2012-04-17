---
date: '2009-07-27 13:49:20'
layout: post
slug: ruby-rack-and-couchdb-lots-of-awesomeness
status: publish
title: Ruby, Rack and CouchDB = lots of awesomeness
wordpress_id: '536'
categories:
- merb
- Misc
- rails
- ruby
- Tutorial
tags:
- couchdb
- logging
- rack
- rails3
- railssummit
- ruby
---

Over the weekend, I spent some time working on a Ruby + Rack +CouchDB project. Three technologies that I know quite well but that I never put to work together at the same time, at least not directly.  Let's call this Part I.

Before we get started, let me introduce each component:



	
  * [Ruby](http://en.wikipedia.org/wiki/Ruby%20%28programming%20language%29) : if you are reading this blog, you more than likely know at least a little bit about, what I consider, one of the most enjoyable programming language out there. It's also a very flexible language that lets us do some interesting things. I could have chosen Python to do the same project but that's a whole different topic. For this project we will do something Ruby excels at: reopening existing classes and injecting more code.

	
  * [Rack](http://rack.rubyforge.org/): a webserver interface written in Ruby and inspired by [Python's WSGI](http://www.wsgi.org/wsgi/). Basically, it's a defined API to interact between webservers and web frameworks. It's used by most common Ruby web frameworks, from Sinatra to Rails (btw, Rails3 is going to be even more Rack-focused than it already is). So, very simply put, the webserver receives a request, passes it to Rack, that converts it, passes it to your web framework and the web framework sends a response in the expected format (more on Rack later).

	
  * [CouchDB](http://couchdb.apache.org/): Apache's document-oriented database. RESTful API, schema-less, written in Erlang with built-in support for map/reduce. For this project, I'm using [CouchRest](http://github.com/mattetti/couchrest), a Ruby wrapper for Couch.




## Goal: Log Couch requests and analyze data


Let's say we have a Rails, Sinatra or Merb application and we are using CouchRest (maybe we are using CouchRest and ActiveRecord, but let's ignore that for now).

Everything works fine but we would like to profile our app a little and maybe optimize the DB usage. The default framework loggers don't support Couch. The easy way would be to tail the Couch logs or look at the logs in [CouchDBX](http://janl.github.com/couchdbx/). Now, while that works, we can't really see what DB calls are made per action, so it makes any optimization work a bit tedious. (Note that Rails3 will have some better conventions for logging, making things even easier)

So, let's see how to fix that. Let's start by looking at Rack.


## Rack Middleware


Instead of hacking a web framework specific solution, let's use Rack. Rack is dead simple, you just need to write a class that has a _call_ method.
In our case, we don't care about modifying the response, we just want to instrument our app. We just want our middleware to be transparent and let our webserver deal with it normally.    

Here we go ... that wasn't hard, was it? We keep the application reference in the @app variable when a new instance of the middleware is created. Then when the middleware is called, we just call the rest of the chain and pretend nothing happened.

As you can see, we just added some logging info around the request. Let's do one better and save the logs in CouchDB:  

Again, nothing complicated. In our rackup file we defined which Couch database to use and we passed it to our middleware (we change our initialize method signature to take the DB).
Finally, instead of printing out the logs, we are saving them to the database.

W00t! At this point all our requests have been saved in the DB with all the data there, ready to be manipulated by some map/reduce views we will write. For the record, you might want to use the bulk_save approach in CouchDB which will wait for X amount of records to save them in the DB all at once. Couch also let's you send new documents, but only save it to the DB every X documents or X seconds.

![](http://img.skitch.com/20090726-ebmpgjtrc6x8239ia69kmri1rt.jpg)

As you can see, our document contains the timestamps and the full environment as a hash.

All of that is nice, but even though we get a lot of information, we could not actually see any of the DB calls made in each request. Let's fix that and inject our logger in CouchRest (you could apply the same approach to any adapter).

Let's reopen the HTTP Abstraction layer class used by CouchRest and inject some instrumentation:



Again, nothing fancy, we are just opening the module, reopening the methods and wrapping our code around the _super_ call (for those who don't know, _super_ calls the original method).

This is all for Part I. In Part II, we'll see how to process the logs and make all that data useful.

By the way, if you make it to [RailsSummit](http://www.railssummit.com.br/), I will be giving a talk on Rails3 and the new exciting stuff you will be able to do including Rack based stuff, CouchDB, MongoDB, new DataMapper etc..

[![](http://railssummit.com.br/images/banners/en_souPalestrante_210x60.jpg)](http://railssummit.com.br/)
