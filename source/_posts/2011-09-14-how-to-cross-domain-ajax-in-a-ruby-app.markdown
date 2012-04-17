---
date: '2011-09-14 16:11:41'
layout: post
slug: how-to-cross-domain-ajax-in-a-ruby-app
status: publish
title: How to - cross domain ajax to a Ruby app
wordpress_id: '1133'
categories:
- blog-post
- Misc
- Ruby
tags:
- html5
---

In some cases, you might have a bunch of apps running on different domains/subdomains and/or ports and you would like to make ajax requests between these services. The problem is that browsers wouldn't let you make such requests because of the Same Origin Policy which only allowed them to make request to resources within the same domain.

However, most browsers (IE 8+, Firefox 3.5+, Safari 4+, Chrome) implement a simple way to allow cross domain requests as defined in this [w3C document](http://www.w3.org/TR/cors/).

Of course, if your users have an old version of their browser, you  might have to look into jsonp or something else such as cheating by using iframes & setting document.domain. Let's pretend for a minute that 100% of your users are on Chrome. The only thing you need to do is set a response header listing the accepted domains or "*" for all. A simple Rack middleware to do that would look like that.



    
    class XOriginEnabler
      ORIGIN_HEADER = "Access-Control-Allow-Origin"
    
      def initialize(app, accepted_domain="*")
        @app = app
        @accepted_domain = accepted_domain
      end
    
      def call(env)
        status, header, body = @app.call(env)
        header[ORIGIN_HEADER] = @accepted_domain
        [status, header, body]
      end
    end


And to use the middleware you would need to set it for use:

    
    use XOriginEnabler


To enable all requests from whatever origin, or pass the white listed domain(s) as shown below.

    
    use XOriginEnabler, "demo.mysite.com demo.mysite.fr demo.techcrunch.com"


For a full featured middleware, see [this project](https://github.com/cyu/rack-cors).
