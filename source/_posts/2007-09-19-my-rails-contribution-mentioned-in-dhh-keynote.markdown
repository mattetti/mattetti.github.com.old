---
date: '2007-09-19 02:36:00'
layout: post
legacy_url: http://railsontherun.com/2007/09/19/my-rails-contribution-mentioned-in-dhh-keynote/
slug: my-rails-contribution-mentioned-in-dhh-keynote
source: railsontherun.com
status: publish
title: my Rails contribution mentioned in DHH keynote
wordpress_id: '95'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- dhh
- rails
- rake
- tasks
---

That's kind of cool, I was reading a [post](http://casperfabricius.com/blog/2007/09/18/railsconf2007-dhh/) about DHH Keynote at RailsConf Europe when I realized that DHH mentioned one of my contribution to Rails Edge.





"small, but to me significant improvement has also found its way to Rails 2.0: You can now create the needed databases with a rake command. By running this command, all referenced databases in your database.yml will be created"






  
    
    <tt>
    </tt>


  
    
    rake db<span class="sy">:create</span><span class="sy">:all</span>






I'm glad to see that my contribution is appreciated, as a reminder you also have the following options:





Only create your current environment database (can be useful to bootstrap your application):






  
    
    <tt>
    </tt>


  
    
    rake db<span class="sy">:create</span>






Another command I use often in development is:






  
    
    <tt>
    </tt>


  
    
    rake db<span class="sy">:reset</span>






It simply drops your current environment database, re create it and migrate it :)
(btw, your new database will be utf-8 by default)





Here is a list of the new rake tasks:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt>


  
    
    rake db<span class="sy">:create</span>                       <span class="c"># Create the local database defined in config/database.yml for the current RAILS_ENV</span><tt>
    </tt>rake db<span class="sy">:create</span><span class="sy">:all</span>                   <span class="c"># Create all the local databases defined in config/database.yml</span><tt>
    </tt>rake db<span class="sy">:drop</span>                         <span class="c"># Drops the database for the current environment</span><tt>
    </tt>rake db<span class="sy">:reset</span>                         <span class="c"># Drops, creates and then migrates the database for the current environment. Target specific version with VERSION=x</span>






Read DHH Keynote summary [there](http://casperfabricius.com/blog/2007/09/18/railsconf2007-dhh/)
