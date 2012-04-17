---
date: '2007-12-17 09:52:00'
layout: post
legacy_url: http://railsontherun.com/2007/12/17/rails-2-0-2-with-few-changes/
slug: rails-2-0-2-with-few-changes
source: railsontherun.com
status: publish
title: Rails 2.0.2 with few changes
wordpress_id: '376'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- contribution
- database
- rails
- release
- tasks
- update
---

## What's new?







  * Rails default database is now sqlite3. If you are running Leopard, everything is already setup for you. As DHH mentioned, just "rails -d mysql your_app_name" if you want to generate a new app preconfigured for MySQL.
Sqlite3 is great and if the only reason why you didn't give it a try is because you have to use CocoaMySQL, then check out [sqlitebrowser](http://sqlitebrowser.sourceforge.net/)



  * Other new feature: rake secret  to generate a key used to encrypt your session. Really useful task when you're migrating an app to 2.x



  * To improve performance, some changes were made to the template caching and you have to restart your production server after a template modification.



  * Validates acceptance of still works for non-existent tables . (sorry [Josh](http://blog.hasmanythrough.com/), I won't post after 2am, especially when I forget your life changing bug fix ;) )



  * Added option to pass proc to ActionController::Base.asset_host for maximum configurability. Example:







  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>7<tt>
    </tt>


  
    
        <span class="co">ActionController</span>::<span class="co">Base</span>.asset_host = <span class="co">Proc</span>.new { |source|<tt>
    </tt>      <span class="r">if</span> source.starts_with?(<span class="s"><span class="dl">'</span><span class="k">/images</span><span class="dl">'</span></span>)<tt>
    </tt>        <span class="s"><span class="dl">"</span><span class="k">http://images.example.com</span><span class="dl">"</span></span><tt>
    </tt>      <span class="r">else</span><tt>
    </tt>        <span class="s"><span class="dl">"</span><span class="k">http://assets.example.com</span><span class="dl">"</span></span><tt>
    </tt>      <span class="r">end</span><tt>
    </tt>    }








  * Finally, I added 2 new rake tasks: db:migrate:redo and db:migrate:reset  

db:migrate:redo rolls back your database and then migrates it up. You can define the STEP constant to specify the amount of steps you want to rollback. This task is very useful in development mode when making a modification to the latest migration(s).
db:migrate:reset will drop your database, re create it and migrate it up. Only use this task if you really have to and only in development/test environment.  Use db:reset in production mode since it uses the schema.rb file and won't go through the hundreds of migrations you might have.





## Changeset





_2.0.2_ (December 16th, 2007)







  * Changed the default database from mysql to sqlite3, so now running "rails myapp" will have a config/database.yml that's setup for SQLite3 (which in OS X Leopard is installed by default, so is the gem, so everything Just Works with no database configuration at all). To get a Rails application preconfigured for MySQL, just run "rails -d mysql myapp" [DHH]



  * Turned on ActionView::Base.cache_template_loading by default in config/environments/production.rb to prevent file system stat calls for every template loading to see if it changed (this means that you have to restart the application to see template changes in production mode) [DHH]



  * Introduce `rake secret` to output a crytographically secure secret key for use with cookie sessions #10363 [revans]



  * Fixed that local database creation should consider 127.0.0.1 local #9026 [parcelbrat]



  * Fixed that functional tests generated for scaffolds should use fixture calls instead of hard-coded IDs #10435 [boone]



  * Added db:migrate:redo and db:migrate:reset for rerunning existing migrations #10431, #10432  [matt]



  * RAILS_GEM_VERSION may be double-quoted also.  #10443 [James Cox]



  * Update rails:freeze:gems to work with RubyGems 0.9.5.  [Jeremy Kemper]



  * Added delete_via_redirect and put_via_redirect to integration testing #10497 [philodespotos]



  * Allow headers['Accept'] to be set by hand when calling xml_http_request #10461 [BMorearty]



  * Added OPTIONS to list of default accepted HTTP methods #10449 [holoway]



  * Added option to pass proc to ActionController::Base.asset_host for maximum configurability #10521 [chuyeow]. Example:





ActionController::Base.asset_host = Proc.new { |source|
  if source.starts_with?('/images')
    "http://images.example.com"
  else
    "http://assets.example.com"
  end
}



  * Fixed that ActionView#file_exists? would be incorrect if @first_render is set #10569 [dbussink]



  * Added that Array#to_param calls to_param on all it's elements #10473 [brandon]



  * Ensure asset cache directories are automatically created.  #10337 [Josh Peek, Cheah Chu Yeow]



  * render :xml and :json preserve custom content types.  #10388 [jmettraux, Cheah Chu Yeow]



  * Refactor Action View template handlers.  #10437, #10455 [Josh Peek]



  * Fix DoubleRenderError message and leave out mention of returning false from filters.  Closes #10380 [Frederick Cheung]



  * Clean up some cruft around ActionController::Base#head.  Closes #10417 [ssoroka]



  * Ensure optimistic locking handles nil #lock_version values properly.  Closes #10510 [rick]



  * Make the Fixtures Test::Unit enhancements more supporting for double-loaded test cases.  Closes #10379 [brynary]



  * Fix that validates_acceptance_of still works for non-existent tables (useful for bootstrapping new databases).  Closes #10474 [hasmanyjosh]



  * Ensure that the :uniq option for has_many :through associations retains the order.  #10463 [remvee]



  * Base.exists? doesn't rescue exceptions to avoid hiding SQL errors.  #10458 [Michael Klishin]



  * Documentation: Active Record exceptions, destroy_all and delete_all.  #10444, #10447 [Michael Klishin]



