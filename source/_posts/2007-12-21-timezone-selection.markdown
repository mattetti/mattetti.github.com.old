---
date: '2007-12-21 23:47:00'
layout: post
legacy_url: http://railsontherun.com/2007/12/21/timezone-selection/
slug: timezone-selection
source: railsontherun.com
status: publish
title: TimeZone selection
wordpress_id: '409'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- timezone
- tzinfo. time_zone_select
---

Supporting TimeZones is a serious pain in a developer's neck. Thankfully, Rails comes with a bunch of tools making our life easier. One of my Rails patch [just got merged](http://dev.rubyonrails.org/changeset/8473) in Rails Edge (Future Rails 2.1)  and I believe it will make my life just a tiny be more enjoyable.





Let's look at a simple scenario, your application has many users and each user has the option of setting his own TimeZone. Most of your users are in the US and actually on the West Coast. You probably don't want to list all the time zones on earth, and you don't really want to include a blank option. What we want is to have a list of top priority time zones (US zones) and to pre select the most comment zone. (L.A. / PST)





I won't go into TZ support in the back-end, check [this post](http://weblog.jamisbuck.org/2007/2/2/introducing-tztime), [this other one](http://www.caboo.se/articles/2007/2/23/adding-timezone-to-your-rails-app) or yet [that one](http://www.marklunds.com/articles/one/311) for more information.





Let's look at our views, option one, you want to use the Rails built-in TimeZones:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>7<tt>
    </tt>8<tt>
    </tt>9<tt>
    </tt><strong>10</strong><tt>
    </tt>11<tt>
    </tt>12<tt>
    </tt>13<tt>
    </tt>14<tt>
    </tt>


  
    
    <% form_for <span class="sy">:user</span> <span class="r">do</span> |f| -<span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt>  <p</span><span class="dl">></span></span><tt>
    </tt>    <label <span class="r">for</span>=<span class="s"><span class="dl">"</span><span class="k">name</span><span class="dl">"</span></span>><span class="co">Name</span>: <em>(full name)<<span class="rx"><span class="dl">/</span><span class="k">em><</span><span class="dl">/</span></span>label><br/><tt>
    </tt>    <%= f.text_field <span class="sy">:name</span> <span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt>  </p</span><span class="dl">></span></span><tt>
    </tt>  <p><tt>
    </tt>    <label><span class="co">Default</span> timezone<span class="sy">:<</span>/label><br/><tt>
    </tt>    <%= f.time_zone_select (<span class="s"><span class="dl">'</span><span class="k">time_zone</span><span class="dl">'</span></span>, <span class="co">TimeZone</span>.us_zones, <tt>
    </tt>                                          <span class="sy">:default</span> => <span class="s"><span class="dl">"</span><span class="k">Pacific Time (US & Canada)</span><span class="dl">"</span></span>) <span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt>  </p</span><span class="dl">></span></span><tt>
    </tt><tt>
    </tt>  <div <span class="r">class</span>=<span class="s"><span class="dl">"</span><span class="k">actions</span><span class="dl">"</span></span>><%= submit_tag <span class="s"><span class="dl">'</span><span class="k">register</span><span class="dl">'</span></span> <span class="s"><span class="dl">%></span><span class="k"></div</span><span class="dl">></span></span><tt>
    </tt><tt>
    </tt><% <span class="r">end</span> -<span class="s"><span class="dl">%></span></span>






![builtin timezones](http://myskitch.com/matt_a/time_zone_select-20071221-182928.jpg)





Option 2, we are using [TZinfo](http://rubyforge.org/projects/tzinfo/):






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>7<tt>
    </tt>8<tt>
    </tt>9<tt>
    </tt><strong>10</strong><tt>
    </tt>11<tt>
    </tt>12<tt>
    </tt>13<tt>
    </tt>14<tt>
    </tt><strong>15</strong><tt>
    </tt>


  
    
    <% form_for <span class="sy">:user</span> <span class="r">do</span> |f| -<span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt>  <p</span><span class="dl">></span></span><tt>
    </tt>    <label <span class="r">for</span>=<span class="s"><span class="dl">"</span><span class="k">name</span><span class="dl">"</span></span>><span class="co">Name</span>: <em>(full name)<<span class="rx"><span class="dl">/</span><span class="k">em><</span><span class="dl">/</span></span>label><br/><tt>
    </tt>    <%= f.text_field <span class="sy">:name</span> <span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt>  </p</span><span class="dl">></span></span><tt>
    </tt>  <p><tt>
    </tt>    <label><span class="co">Default</span> timezone<span class="sy">:<</span>/label><br/><tt>
    </tt>    <%= f.time_zone_select(<span class="s"><span class="dl">'</span><span class="k">time_zone</span><span class="dl">'</span></span>, <span class="co">TZInfo</span>::<span class="co">Timezone</span>.us_zones, <tt>
    </tt>                                         <span class="sy">:default</span> => <span class="s"><span class="dl">"</span><span class="k">America/Los_Angeles</span><span class="dl">"</span></span>, <tt>
    </tt>                                         <span class="sy">:model</span> => <span class="co">TZInfo</span>::<span class="co">Timezone</span> <span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt>  </p</span><span class="dl">></span></span><tt>
    </tt><tt>
    </tt>  <div <span class="r">class</span>=<span class="s"><span class="dl">"</span><span class="k">actions</span><span class="dl">"</span></span>><%= submit_tag <span class="s"><span class="dl">'</span><span class="k">register</span><span class="dl">'</span></span> <span class="s"><span class="dl">%></span><span class="k"></div</span><span class="dl">></span></span><tt>
    </tt><tt>
    </tt><% <span class="r">end</span> -<span class="s"><span class="dl">%></span></span>






![TZinfo](http://myskitch.com/matt_a/timezone_select_tzinfo-20071221-183443.jpg)





Obviously, the default timezone is used only if the user didn't set one already. 





Pretty simple feature but hopefully quite helpful!
