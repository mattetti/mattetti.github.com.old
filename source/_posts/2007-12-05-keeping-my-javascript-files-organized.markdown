---
date: '2007-12-05 04:01:00'
layout: post
legacy_url: http://railsontherun.com/2007/12/05/keeping-my-javascript-files-organized/
slug: keeping-my-javascript-files-organized
source: railsontherun.com
status: publish
title: keeping my javascript files organized
wordpress_id: '301'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- caching
- content_for
- javascript
- low pro
- lowpro
- rails
- structure
---

I was recently asked by Rubyist friend ([Josh Knowles](http://joshknowles.com/)) how I was organizing my Javascript files when using [LowPro](http://lowpro.stikipad.com/home/show/HomePage).





[LowPro](http://lowpro.stikipad.com/home/show/HomePage) is the best solution for doing [Unobtrusive Javascript](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript) using [Prototype](http://www.prototypejs.org/).





With the help of [LowPro](http://lowpro.stikipad.com/home/show/HomePage), you define behaviors that get triggered by the user. This is great, however, you'll notice that some behaviors are used all over the place (a date chooser for instance) and some complicated behaviors only get used on very specific pages.





First things first, let's look at the header in my application.html.erb file (located in app/views/layouts).
This is the default layout used by all my views, I rarely use more than 5 layouts per app and always use a default layout.





Please note that I'm using Rails 2.0 so some features you'll see in my file won't work in Rails 1.2.x. (if you want to know about all the new Rails sexiness, check on [this awesome Peepcode PDF](http://peepcode.com/products/rails2-pdf).





Here we go:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>


  
    
      <%= javascript_include_tag <span class="s"><span class="dl">'</span><span class="k">prototype</span><span class="dl">'</span></span>, <span class="s"><span class="dl">'</span><span class="k">effects</span><span class="dl">'</span></span>, <span class="sy">:cache</span> => <span class="pc">true</span> <span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt>  <%= javascript_include_tag 'lowpro', :cache =</span><span class="dl">></span></span> <span class="pc">true</span> <span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt>  <%= javascript_include_tag  'application', :cache =</span><span class="dl">></span></span> <span class="pc">true</span> <span class="s"><span class="dl">%></span></span>






The first thing you might notice is that I don't use






  
    
    <tt>
    </tt>


  
    
      <%= javascript_include_tag <span class="sy">:defaults</span> <span class="s"><span class="dl">%></span></span>






The reason? I don't want to load prototype.js, effects.js, controls.js, dragdrop.js, and application.js all at once. I almost never use drag'n'drop and seldom use Autocompleter and InPlaceEditor so why loading them in each and every single page of my apps? I'm not saying they are bad libraries, I'm just saying that in more than 80% of my page, I don't use them, so they should not be in my default page load.





The second thing you might notice, I use :cache => true. Asset caching is a new feature in Rails 2.0 which combines related assets into a single file (works with css and js and only in production mode)
 Note that the above code is untested, but everything should be loaded properly, otherwise, make sure proto gets loaded first, then lowpro, then application. (and you can probably create a one-liner)





All the default behaviors are defined in the application.js file, so they get loaded on all page. However to handle action specific behaviors, I use another Rails trick right in the header:






  
    
    <tt>
    </tt>


  
    
     <%= <span class="r">yield</span> <span class="sy">:javascript</span> <span class="s"><span class="dl">%></span></span>






Why? Very simple, I want to load some custom JS in the header depending on the action that is used. For instance, when a visitor goes to my fancy ajax photo editor, I want to load the content editor javascript right in the header where it belongs.





For that, I simply need to add the following to my view:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>


  
    
      <% content_for <span class="sy">:javascript</span> <span class="r">do</span> <span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt>    <%= javascript_include_tag "photos_show" %</span><span class="dl">></span></span><tt>
    </tt>  <% <span class="r">end</span> <span class="s"><span class="dl">%></span></span>






If content_for :javascript isn't define, noting is yield in my header and therefore nothing is included but whenever I need, I can access my header directly from the view and insert javascript code in a very clean way.





photos_show.js is the javascript defining all the behaviors related to the photos controller and the show action. I usually only have few actions with a lot of custom behaviors so, these structure works well for me.






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>


  
    
      javascripts<tt>
    </tt>    <span class="er">\</span>controller_action.js<tt>
    </tt>    <span class="er">\</span>controller_another_action.js<tt>
    </tt>    <span class="er">\</span>another_controller_action.js<tt>
    </tt>    ...






However in the case of an app with a lot of custom behaviors, I recommend using the following structure:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>


  
    
      javascripts<tt>
    </tt>    <span class="er">\</span> controller_name_<tt>
    </tt>            <span class="er">\</span>action_name_.js<tt>
    </tt>      <span class="er">\</span>another_controller_name<tt>
    </tt>              <span class="er">\</span>action_name_.js<tt>
    </tt>  ...






That's it folks, I'm not a javascript expert, and if you know better, don't hesitate to leave me a comment. What I know for sure, is that since I started using LowPro and behavior driven with Prototype, I have much more fun. Adding a bit of structure is a simple way for me to keep my code clear and help other people who have to work with me. (more on that later)
