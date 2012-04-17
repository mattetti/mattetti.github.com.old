---
date: '2007-11-27 06:44:00'
layout: post
legacy_url: http://railsontherun.com/2007/11/27/lowpro-0-5-what-s-new/
slug: lowpro-0-5-what-s-new
source: railsontherun.com
status: publish
title: LowPro trunk, what's new?
wordpress_id: '266'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- javascript
- lowpro
- rails
- san diego
- ujs
---

If you've read my [post on Ajax pagination](http://railsontherun.com/2007/9/27/ajax-pagination-in-less-than-5-minutes) you know that I'm a big fan of [Dan Webb](http://www.danwebb.net)'s [LowPro](http://www.danwebb.net/lowpro) unobtrusive javascript library.





Doing Unobtrusive Javascript (UJS) is basically registering event handlers programmatically using CSS selectors to select the elements to register. In other words : keeping things separate and avoiding inline javascript.





If you've been using LowPro 0.4 and recently tried to upgrade to [Prototype 1.6](http://www.prototypejs.org/) you probably noticed that things don't work as they used to. 





The first thing you want to do, is to update to the [latest version of Lowpro](http://svn.danwebb.net/external/lowpro/trunk/dist/).





## So what's new in 0.5 trunk?





First off, you need to know that a lot of lowpro features were moved in Prototype 1.6 core :)







  * You now get a warning via firebug if you try to use Low Pro with a version of Prototype that its not designed to work with.


  * Alternative event system ripped out: uses core events


  * DOM method mixins ripped out: alternatives all in prototype


  * Event.onReady delegates to the new dom:loaded event.  However this doesn't fire immediately if the dom is already loaded like Event.onReady did. (might be patched to work as before)


  * DOMBuilder is staying but now is a thin shell around the new proto 1.6 Element stuff.


  * You can still return false from event handlers in addEvent and Behaviors to stop the event but now if you use Event.observe raw you don't get this.


  * Behavior.create now works like Class.create in 1.6 so behaviors can have full inheritance:






  
    
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


  
    
      <span class="co">Basic</span> = <span class="co">Behavior</span>.create({<tt>
    </tt>   onclick: function() {<tt>
    </tt>    alert(<span class="s"><span class="dl">'</span><span class="k">woo</span><span class="dl">'</span></span>);<tt>
    </tt>   }<tt>
    </tt>  });<tt>
    </tt><tt>
    </tt>  <span class="co">SuppedUp</span> = <span class="co">Behavior</span>.create({<tt>
    </tt>   onclick: function(<span class="gv">$super</span>) {<tt>
    </tt>    alert(<span class="s"><span class="dl">'</span><span class="k">wee</span><span class="dl">'</span></span>);<tt>
    </tt>    <span class="gv">$super</span>();<tt>
    </tt>   }<tt>
    </tt>  });






Works really nicely.







  * core behaviors : Remote and Observed are now moved into the lowpro core (you don't need to include the external files).



  * Event.addBehavior.reassignAfterAjax defaults to false. If you want re assign behaviors after an ajax call, you need to turn this option to true.



  * Event.addBehavior.reload(); added to reload/re assign behaviors. Very useful if you dynamically insert elements you want to observe!



  * [new website](http://lowprojs.com) has been set up and will contain documentation and tips - Full API docs coming soon.  There's also a dedicated [google group](http://groups.google.co.uk/group/low-pro).  






Here is a quick example with real life code. (which could be refactored, I know)






  
    
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
    </tt>16<tt>
    </tt>17<tt>
    </tt>18<tt>
    </tt>19<tt>
    </tt><strong>20</strong><tt>
    </tt>21<tt>
    </tt>22<tt>
    </tt>23<tt>
    </tt>24<tt>
    </tt><strong>25</strong><tt>
    </tt>26<tt>
    </tt>27<tt>
    </tt>28<tt>
    </tt>29<tt>
    </tt><strong>30</strong><tt>
    </tt>31<tt>
    </tt>32<tt>
    </tt>33<tt>
    </tt>34<tt>
    </tt><strong>35</strong><tt>
    </tt>36<tt>
    </tt>37<tt>
    </tt>38<tt>
    </tt>39<tt>
    </tt><strong>40</strong><tt>
    </tt>41<tt>
    </tt>42<tt>
    </tt>


  
    
    <span class="c">// Make sure the behaviors still work even after navigating to another page using the ajax navigation.</span><tt>
    </tt>Event.addBehavior.reassignAfterAjax = <span class="pc">true</span>;<tt>
    </tt><tt>
    </tt><span class="c">// Behaviors</span><tt>
    </tt>Event.addBehavior({<tt>
    </tt>  <span class="c">// Pagination links  </span><tt>
    </tt>  <span class="s"><span class="dl">'</span><span class="k">div.pagination a</span><span class="dl">'</span></span> : Remote.<span class="pt">Link</span>,<tt>
    </tt><tt>
    </tt>  <span class="c">// Reset the list when a user clicks on cancel.</span><tt>
    </tt>  <span class="s"><span class="dl">'</span><span class="k">a.cancel_button:click</span><span class="dl">'</span></span> : <span class="r">function</span>() {<tt>
    </tt>    $(<span class="s"><span class="dl">'</span><span class="k">list_of_things</span><span class="dl">'</span></span>).update(<span class="s"><span class="dl">"</span><span class="dl">"</span></span>);<tt>
    </tt>  },<tt>
    </tt><tt>
    </tt>  <span class="c">// carousel navigation prev</span><tt>
    </tt>  <span class="s"><span class="dl">'</span><span class="k">a#carousel_prev:click</span><span class="dl">'</span></span> : <span class="r">function</span>() {<tt>
    </tt>    moveCarousel(<span class="s"><span class="dl">'</span><span class="k">prev</span><span class="dl">'</span></span>); <span class="r">return</span> <span class="pc">false</span>;<tt>
    </tt>  },<tt>
    </tt><tt>
    </tt>  <span class="c">// carousel navigation next</span><tt>
    </tt>  <span class="s"><span class="dl">'</span><span class="k">a#carousel_next:click</span><span class="dl">'</span></span> : <span class="r">function</span>() {<tt>
    </tt>    moveCarousel(<span class="s"><span class="dl">'</span><span class="k">next</span><span class="dl">'</span></span>); <span class="r">return</span> <span class="pc">false</span>;<tt>
    </tt>  },<tt>
    </tt><tt>
    </tt>  <span class="s"><span class="dl">'</span><span class="k">div.panel_pic:click</span><span class="dl">'</span></span> : <span class="r">function</span>() {<tt>
    </tt>    removePanelPic(<span class="pc">this</span>);<tt>
    </tt>  },<tt>
    </tt><tt>
    </tt>  <span class="s"><span class="dl">'</span><span class="k">div.photo_from_row img:click</span><span class="dl">'</span></span> : <span class="r">function</span>() {<tt>
    </tt>    <span class="c">// Get the div holding the pic and use it as a target</span><tt>
    </tt>    <span class="r">var</span> target = <span class="pc">this</span>.up();<tt>
    </tt>    addPicToPanel(target);<tt>
    </tt>    new Effect.Highlight(target);<tt>
    </tt>  }<tt>
    </tt>});<tt>
    </tt><tt>
    </tt><span class="r">function</span> addPicToPanel(target){<tt>
    </tt>  new Insertion.Bottom(<span class="s"><span class="dl">'</span><span class="k">control_panel_photos</span><span class="dl">'</span></span>, <span class="s"><span class="dl">"</span><span class="k"><div id='edit_</span><span class="dl">"</span></span>+ target.id +<span class="s"><span class="dl">"</span><span class="k">' class='panel_pic'><img class='panel_pic' src='</span><span class="dl">"</span></span> +  target.immediateDescendants()[0].src + <span class="s"><span class="dl">"</span><span class="k">'/></div></span><span class="dl">"</span></span>);<tt>
    </tt>  <span class="c">// Reload the behaviors so the new inserted pic can be monitored </span><tt>
    </tt>  <span class="c">// and the 'div.panel_pic:click' behavior can be triggered</span><tt>
    </tt>  Event.addBehavior.<span class="fu">reload</span>();<tt>
    </tt>}<tt>
    </tt>  






LowPro is a great way of keeping your code really clean and your views very accessible.





If you are interested in knowing more about UJS, come to our [SDruby group meeting](http://sdruby.com/) on Dec 6 @ 7:30pm ([directions](http://tinyurl.com/2f486e)). And if you don't care about UJS, come later to hear about Facebook API. Don't forger to bring your questions for our first [Rails roundtable](http://groups.google.com/group/sdruby/browse_thread/thread/d488b70d67f84a5f#).
