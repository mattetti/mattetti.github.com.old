---
date: '2007-05-22 05:02:00'
layout: post
legacy_url: http://railsontherun.com/2007/05/22/dbtasks-intro/
slug: dbtasks-intro
source: railsontherun.com
status: publish
title: db_tasks or how to easily manage your db
wordpress_id: '24'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- database
- db_tasks
- rake
- rake task
- utf8
---

[Josh knowles](http://joshknowles.com) and Matt Heidemann from [Integrum](http://integrumtech.com) and I released few weeks a go a cool plugin called [rake_tasks](http://svn.integrumtech.com/public/plugins/rake_tasks/)





The thing is that the plugin was getting too big when I started hacking the MySQL adapter and adding utf-8 support we figured out it was smarter to split the plugin and create [db_tasks](http://svn.aimonetti.net/public/plugins/db_tasks/) and svn_tasks





I temporarily hosted db_tasks there:





http://svn.aimonetti.net/public/plugins/db_tasks/




    
    <code>ruby script/plugin install http://svn.aimonetti.net/public/plugins/db_tasks/
    </code>





check your Rake tasks:







  * _rake db:create_      Creates the databases defined in your config/database.yml (unless they already exist) You can also specify the charset and collation you want your dbs to use: rake db:create CHARSET='latin1' COLLATION='latin1_bin')


  * _rake db:drop_       Drops the database for your currenet RAILS_ENV as defined in config/database.yml


  * _rake db:reset_       Drops, creates and then migrates the database for your current RAILS_ENV


  * _rake db:shell_       Launches the database shell using the values defined in config/database.yml


  * _rake db:charset_    Returns the charset of your current RAILS_ENV database


  * _rake db:collation_   Returns the collation of your current RAILS_ENV database


  * _rake db:update_connection_settings_  add the encoding format to each environment in the database.yml file





By default, your new databases will be utf8, you can automatically create your databases by typing:





rake db:create





or reset your databases (drop, create, migrate)





rake db:reset





## Another simple plugin for lazy developers.
