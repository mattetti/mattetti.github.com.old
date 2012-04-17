---
date: '2007-09-27 03:40:00'
layout: post
legacy_url: http://railsontherun.com/2007/09/27/ajax-pagination-in-less-than-5-minutes/
slug: ajax-pagination-in-less-than-5-minutes
source: railsontherun.com
status: publish
title: Ajax Pagination in less than 5 minutes
wordpress_id: '107'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- behaviors
- graceful degradation
- javascript
- lowpro
- pagination
- progressive enhancement
- rails
- ujs
- unobtrusive
- will_paginate
---

## updated Nov 26 to reflect the recent low pro changes. (please use low pro 0.5 and Prototype 1.6)





Recently one of my client asked me to add 'ajax' pagination to his application. His site already had a very nice pagination using the excellent [will_paginate](http://plugins.require.errtheblog.com/browser/will_paginate) from [Mislav](http://www.workingwithrails.com/person/2764-mislav-marohni) and the guys(PJ & Chris) from [err the blog](http://errtheblog.com/) but since my client had a special need where he had to have Ajax.





It took me **virtually no time to convert the standard pagination into an Ajax navigation** while still degrading gracefully.(it works even without Javascript)





I really enjoy using will_paginate, it's very well written and the authors keep up with the [bugs](http://err.lighthouseapp.com/projects/466-plugins/tickets?q=tagged%3Awill_paginate) and [new features](http://err.lighthouseapp.com/projects/466-plugins/tickets?q=tagged%3Awill_paginate).





## Start by installing will_paginate:




    
    <code>ruby script/plugin install svn://errtheblog.com/svn/plugins/will_paginate
    </code>





## Then go watch the [Railcast screencast](http://railscasts.com/episodes/51) about will paginate.





Once you have your pagination working, we will do some '[progressive enhancement](http://en.wikipedia.org/wiki/Progressive_enhancement)'.





What we want is to **add a behavior to the pagination link**. The behavior would make the same call than the normal link but via an ajax(Javascript) call. 





## Add lowpro





To do that, you simply need to add the excellent ['lowpro'](http://www.danwebb.net/lowpro) Prototype extension from [Dan Webb](http://www.danwebb.net)





You can get the files directly from the [lowpro's repository](http://svn.danwebb.net/external/lowpro/trunk/dist/lowpro.js).





Add [lowpro.js](http://svn.danwebb.net/external/lowpro/trunk/dist/lowpro.js) to your public/javascript folder.





Don't forget to include the javascript in your page. (






  
    
    1<tt>
    </tt>2<tt>
    </tt>


  
    
    <tt>
    </tt>  <%= javascript_include_tag <span class="s"><span class="dl">'</span><span class="k">lowpro</span><span class="dl">'</span></span> <span class="s"><span class="dl">%></span><span class="k"><tt>
    </tt></span></span>






## Create a behavior





Now open your application.js file (or whichever Javascript file you're using) and add the following:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>


  
    
    <tt>
    </tt>  <span class="co">Event</span>.addBehavior.reassignAfterAjax = <span class="pc">true</span>;<tt>
    </tt>  <span class="co">Event</span>.addBehavior({<tt>
    </tt>    <span class="s"><span class="dl">'</span><span class="k">div.pagination a</span><span class="dl">'</span></span> : <span class="co">Remote</span>.<span class="co">Link</span><tt>
    </tt>  })<tt>
    </tt>






Refresh your cache, reload your page, and test the link. It will probably look like it doesn't do anything but if you are using [firebug](http://www.getfirebug.com/) or if you are checking your logs, you'll notice something happened. The problem is that we didn't tell our action to send an Ajax response so we get the html full page all over again.





## Setup a response for javascript requests





Got to your action handling the pagination and let's setup a response for Javascript:






  
    
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
    </tt>


  
    
    <tt>
    </tt>  <span class="r">def</span> <span class="fu">index</span><tt>
    </tt>  <span class="iv">@photos</span> = <span class="co">Photo</span>.paginate(<span class="sy">:all</span>, <span class="sy">:conditions</span> => [<span class="s"><span class="dl">"</span><span class="k">photos.user_id = ?</span><span class="dl">"</span></span>, current_user.id], <span class="sy">:page</span> => params[<span class="sy">:page</span>])<tt>
    </tt>    respond_to <span class="r">do</span> |format|<tt>
    </tt>      format.html <span class="c"># index.html.erb</span><tt>
    </tt>      format.js <span class="r">do</span><tt>
    </tt>        render <span class="sy">:update</span> <span class="r">do</span> |page|<tt>
    </tt>          page.replace_html <span class="s"><span class="dl">'</span><span class="k">photos</span><span class="dl">'</span></span>, <span class="sy">:partial</span> => <span class="s"><span class="dl">"</span><span class="k">photos</span><span class="dl">"</span></span><tt>
    </tt>        <span class="r">end</span><tt>
    </tt>      <span class="r">end</span><tt>
    </tt>    <span class="r">end</span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>






Perfect! Now when a visitor clicks on my pagination links, only the photos are paginated, the rest of the page stays the same. Note that my navigation bar is inside the partial so it gets 'updated' after a visitor clicks on any pagination link.





## Read more and convert to UJS (unobtrusive javascript)





Read more about [UJS](http://jlaine.net/2007/8/3/from-rails-ajax-helpers-to-low-pro-part-i) and think about replacing all your nasty inline javascript snippets by pretty behaviors :) (Think about stopping using the obtrusive rails helpers)
