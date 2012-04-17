---
date: '2009-07-13 13:00:13'
layout: post
slug: ruby-authentication-of-couchdb-requests
status: publish
title: Ruby authentication of CouchDB requests
wordpress_id: '526'
categories:
- Misc
tags:
- couchdb
- merb
- nginx
- ruby
---

[CouchDB](http://couchdb.apache.org/) is an awesome technology. I'm lucky enough to work on quite a big project where we decided to switch from MySQL to Couch for various reasons.

One of the many things I like with Couch is that it handles attachments and can replicate them as well as serve them for you using the [Erlang](http://en.wikipedia.org/wiki/Erlang%20%28programming%20language%29) based builtin webserver. (you can load balance your dbs and do some other really cool stuff)

Let's take a use case. Let's imagine that you have a web app with logged in users. Every user can have their own avatar.

No big deal, you get the user to upload his/her avatar to your app and add it to the user document in the database. To serve it from the database, you just need to create a proxy in nginx/apache and redirect the virtual avatar url to the protected DB making sure the request is a GET request.

Add to that a caching solution like [varnish](http://varnish.projects.linpro.no/) or [memcached module for nginx](http://www.igvita.com/2008/02/11/nginx-and-memcached-a-400-boost/) and all your db goodies get cached and served by the cache (server/client) until they get modified.

Now, the problem is when you want to serve authorized attachments. Let's imagine that we want to let our users upload private files, files that should be accessible only by the owner or users designated by the owner.

In this case, a simple [nginx](http://en.wikipedia.org/wiki/Nginx) rewrite wouldn't work. We need to authorize attachment requests. Here is a cool way of doing that using nginx and merb's router. (Expect Rails3 router to do the same).

Let's start by setting up nginx and create a proxy for couchdb:

Now that this is done, we are going to use Merb's awesome router to handle the incoming requests. The cool part of this is that we won't be dispatching requests so, going through the router is almost free. (check on the Merb router benchmarks for more info).  Let's edit our router and set a special route for our assets.  

We are using a deferred route which gets executed instead of dispatching the request.

If the attachment route is being matched then we are checking what environment we are currently running in. If we are in production or staging environment then we are sending back a rack response to the webserver. The response is just a forward to the proper couchdb document behind the proxy. Of course, before allowing that to happen, we could authenticate the logged in user, log the request and do a couple of other things. You have full access to your models from the router, so authenticating a session isn't a big deal. You could even create temporarily urls like AWS s3 does.

If we are not in production or staging mode, then just redirect the request to couch since we assume you have access to the local db. This way, your asset urls will be working in production and dev. In real life, you'll want to apply the authorization before choosing how to deliver the document/attachment tho as you want it work the same way in development and production.
