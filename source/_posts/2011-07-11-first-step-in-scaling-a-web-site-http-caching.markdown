---
date: '2011-07-11 10:21:02'
layout: post
slug: first-step-in-scaling-a-web-site-http-caching
status: publish
title: 'First step in scaling a web site: HTTP caching'
wordpress_id: '1094'
categories:
- blog-post
- JavaScript
- Misc
- Software Design
tags:
- caching
- http cache
- javascript
- scalability
- scaling
- varnish
---

Today my friend [Patrick Crowley](http://twitter.com/mokolabs) and I were talking about scaling his website: [http://cinematreasures.org](http://cinematreasures.org/) since an article covering his work will soon be published in a very popular newspaper. Patrick's site is hosted on [Heroku](http://www.heroku.com/) which comes by default with [Varnish caching](https://www.varnish-cache.org/) enabled.

The challenge is that a lot of people using the Rails framework are used to doing page caching instead of relying on HTTP caching, even though this feature was added a long time ago. The major problem with page caching is that it doesn't scale that well as soon as you run more than one server. Indeed you would need to store the page content to a shared drive between your servers or use memcached and do some work to avoid hitting your app every single time. On the other hand, HTTP caching is extremely easy to handle at the application level and it will dramatically reduce the amount of requests hitting your app. Let me explain a little more about HTTP caching.

Ryan Tomako wrote an [excellent post](http://tomayko.com/writings/things-caches-do) about the details of caching, I strongly recommend you [read it](http://tomayko.com/writings/things-caches-do). In a nutshell, the HTTP caching layer (usually) seats before your application layer and allows you, the developer to store some responses that can be send back to the users based on optional conditions. That might still seem vague, let's take a concrete example. If you look at [http://cinematreasures.org](http://cinematreasures.org)'s home page you can see that it's an agglomerate of various information:


[![CinemaTreasures homepage](https://img.skitch.com/20110709-dnxjhikxr14tdr7e35n97madhn.jpg)](http://cinematreasures.org)


And the bottom of the page contains even more dynamic data such as the popular movie theater photos, latest movie theater videos and latest tweets. One might look at that and say that this page can't really be cached and that the caching should be done at the model layer (i.e. cache the data coming from the database). I would certainly agree that caching the data layer is probably a good idea, but you shouldn't start by that. In fact without caching, this page renders fast enough. The problem is when someone like [Roger Ebert](http://rogerebert.suntimes.com/) tweets about [CinemaTreasures](http://twitter.com/#!/ebertchicago/status/85912164648497152) the load on the app peaks significantly. At the point, the amount of concurrent connections your app can handle gets put to the challenge. Even though your page load is "fast enough", requests will queue up and some will eventually time out. That's actually a perfect case of HTTP caching.

What we want to do in that case is to cache a version of the home page in Varnish for 60 seconds. During that time, all requests coming to the site, will be served by Varnish and will all get the same cached content. That allows our servers to handle the non cached requests and therefore increase our throughput. What's even better, is that if a user refreshes the home page in his/her browser during the first 60 seconds the requests won't even make it all the way to our servers. All of that thanks to conditions set on the response. The first user hitting the HTTP cache layer (Varnish in this case) won't find a fresh cached response, so varnish will forward the request to our application layer which will send back the homepage to varnish and tell Varnish that this content is good for a full minute so please don't ask for it again until a minute from now. Varnish serves this response to the users' browser and let the browser know that the server said that the response was good enough for a minute so don't bother asking for it again. But now, if during these 60 seconds another user comes in, he will hit Varnish and Varnish will have the cached response from the first user and because the cache is still fresh (it's not been 60 seconds since the first request) and the cache is public, then the same response will be sent to the second user.

As you can see, the real strength of HTTP caching is the fact that it's a conditional caching. It's based on the request's URL and some "flags" set in the request/response headers.

Setting these conditions in your app is actually very simple since you just need to set the response's headers. If you are using a Ruby framework you will more than likely have access to the request object via the "request" method and you can set the headers directly like that: "response.headers['Cache-Control'] = 'public, max-age=60'".
In Rails, you can actually use a helper method instead: expires_in 1.minute, :public => true.

You might have a case where you HAVE TO serve fresh content if available and can't serve stale cached content even for a few seconds. In this case, you can rely on the Etag header value. The Etag is meant to validate the freshness of a cached response. Think of it as a signature (unique ID) that is set on the response and used by the client (or cache layer) to see if the server response has changed or not. The way it works is that the client keeps track of the Etag received for each request (attached to the cached response) and then sends it with the next requests. The HTTP layer or application sees the Etag in the request and can check if it is still valid and the content didn't change. If that's the case, an empty response can be sent with a special HTTP status code (304) to let know the client that the old cached value is still good to be used.  Rails has a helper called "stale?" that helps you do the Etag/last modified check and allows you to not fetch all the objects from the database by doing a cheap check on an attribute (For instance you can check the updated_at value and use that as a condition to pull an object and its relationships).

So I explain HTTP caching, I often hear people telling me: "that's great Matt, but you know what, that won't work for us because we have custom content that we display specifically to our users". So in that case, you can always set the Cache-Control header to private which will only cache the response in the client's browser and not the cache layer. That's good to some extent, but it can definitely be improved by rethinking a bit your view layer. In most web apps, the page content is rendered by server side code (Rails, Django, node.js, PHP..) and sent to the user all prepared for him. There are a few challenges with this approach, the biggest one is that the server has to wait until everything is ready (all data fetched, view rendered etc...) before sending back a response and before the client's browser can start rendering (there are ways to chunk the response but that's besides the scope of this post). The other is that the same expensive content has to be calculated/rendered for two different users because you might be inserting the username of the current user at the top of the page for instance. A classic way to deal with that is often to use fragment caching, where the expensive rendering is cached and reused by different requests. That's good but if the only reason to do that is because we are displaying some user specific data, there is a simpler way: async page rendering. The concept is extremely simple: remove all user specific content from the rendered page and then inject the user content in a second step once the page is displayed. The advantage is that now the full page can be cached in Varnish (or Squid or whatever you use for HTTP caching). To inject the user content, the easiest way is to use JavaScript.

Let's stay on CinemaTreasures, when you're logged in, the username is shown on the top of each page:

[caption id="" align="aligncenter" width="574" caption="Once logged in, the username is displayed on all pages"]![](https://img.skitch.com/20110710-mh5tqxuw1txf9kppn1smkkarrs.jpg)[/caption]

The only things that differs from the page rendered when the user is not logged in and when he is, are these 2 links and an avatar. So let's write some code to inject that after rendering the page.

In Rails, in the sessions controller or whatever code logs you in, you need to create a new cookie containing the username:

    
    cookies[:username] = {
             :value => session[:username],
             :expires => 2.days.from_now,
             :domain => ".cinematreasures.org"
           }


As you can see, we don't store the data in the session cookie and the data won't be encrypted. You need to be careful that someone changing his cookie value can't access data he/should shouldn't. But that's a different discussion. Now that the cookie is set, we can read it from JavaScript when the page is loaded.

    
    document.observe("dom:loaded", function() {
      displayLoggedinUserLinks();
    });
    
    function readCookie(name) {
         var nameEQ = name + "=";
         var ca = document.cookie.split(';');
         for(var i=0;i < ca.length;i++) {
              var c = ca[i];
              while (c.charAt(0)==' ') c = c.substring(1,c.length);
              if (c.indexOf(nameEQ) == 0) return c.substring(nameEQ.length,c.length);
         }
         return null;
    }
    
    function displayLoggedinUserLinks() {
      var username            = readCookie('username');
      var loginLink           = $('login');
      var logout              = $('logout');
      if (username == null){
        loginLink.show();
        logout.hide();
      }else{
        // user is logged in and we have his/her username
        loginLink.hide();
        if(userGreetings){ userGreetings.update("<span id="username">username</span>"); }
        logout.show();
        showAvatar(username);
      };
      return true;
    }


The code above doesn't do much, once the DOM is loaded, the displayLoggedinUserLinks() function gets trigger. This function reads the cookie via the readCookie() function and if a username is found, the login link is hidden, the user name is displayed, as well as the logout link and the avatar. (You can also use a jQuery cookie plugin to handle the cookie, but this is an old example using Prototype, replace the code accordingly)
When the user logs out, we just need to delete the username cookie and the cached page will be rendered properly. In Rails, you would do delete the cookie like that: cookies.delete('username').
Quite often you might even want to make an Ajax call to get some information such as the number of user messages or notifications. Using jQuery or whatever JS framework you fancy you can do that once the page is rendered. Here is an example, on this page, you can see the learderboards for MLB The Show. The leaderboards don't change that often, especially the overall leaderboards so they can be cached for a little while, however the player's presence can change anytime. The smart way to deal with that, would be to cache the  leaderboards for a few seconds/minutes and make an ajax call to a presence service passing it a list of user ids collected from the DOM. The service called via Ajax could also be cached  depending on the requirements.

Now there is one more problem that people using might encouter: flash notices. For those of you not familiar with Rails, flash notices are messages set in the controller and passed to the view via the session (at least last time I checked). The problem happens if I'm the home page isn't cached anymore and I logged in which redirects me to the home page with a flash message like so:

![](https://img.skitch.com/20110710-1u6dn8rrc6r62rsg6niphhd2pi.jpg)

The problem is that the message is part of the rendered page and now for 60 seconds, all people hitting the home page will get the same message. This is why you would want to write a helper that would put this message in a custom cookie that you'd pull JS and then delete once displayed. You could use a helper like that to set the cookie:

    
    def flash_notice_cookie(msg, expiration=nil)
      cookies[:flash_notice] = {
        :value => msg,
        :expires => expiration || 1.minutes.from_now,
        :domain => ".cinematreasures.com"
       }
    end


And then add a function called when the DOM is ready which loads the message and injects it in the DOM. Once the cookie read, delete it so the message isn't displayed again.



So there you have it, if you follow these few steps, you should be able to handle easily 10x more traffic without increasing hardware or making any type of crazy code change. Before you start looking into memcached, redis, cdns or whatever, consider HTTP caching and async DOM manipulation. Finally, note that if you can't use Varnish or Squid, you can very easily setup [Rack-Cache](http://rtomayko.github.com/rack-cache/) locally and share the cache via memcached. It's also a great way to test locally.



* * *


**Update:** CinemaTreasures was updated to use HTTP caching as described above. The hosting cost is now half of what it used to be and the throughput is actually higher which offers a better protection against peak traffic.


* * *





External resources:



	
  * [http://tomayko.com/writings/things-caches-do](http://tomayko.com/writings/things-caches-do)

	
  * [HTTP Caching at Heroku](http://devcenter.heroku.com/articles/http-caching)

	
  * [W3 caching protocol ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html)

	
  * [Rack-Cache middleware](http://rtomayko.github.com/rack-cache/)

        
  * [Blog post covering HTTP Caching/Varnish/Rails](http://www.nolanevans.com/2011/03/optimizing-your-rails-site-with-http.html)

	
  * [jQuery cookie plugin](http://plugins.jquery.com/project/Cookie)


