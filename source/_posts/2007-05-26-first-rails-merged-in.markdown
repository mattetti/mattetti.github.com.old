---
date: '2007-05-26 01:03:00'
layout: post
legacy_url: http://railsontherun.com/2007/05/26/first-rails-merged-in/
slug: first-rails-merged-in
source: railsontherun.com
status: publish
title: First Rails patch merged in!
wordpress_id: '26'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- core
- database
- patch
- rails
- unicode
- utf8
---

## I'm quite glad to let you know that my first patch made it to Rails Edge.





UPDATE: Ryan Daigle covered the new added features in [his blog](http://ryandaigle.com/articles/2007/5/29/what-s-new-in-edge-rails-new-database-rake-tasks)
How cool is that?





That's a great achievement that I owe to [josh Knowles](http://joshknowles.com) and [Josh Susser](http://hasmanythrough.com)





[Josh Knowles](http://joshknowles) and Matt Heidemann from [Integrum](http://integrumtech.com) started working on [rake_tasks](http://svn.integrumtech.com/public/plugins/rake_tasks/) a little while ago. I helped them hacking this cool plugin. After a while, we realized that the plugin was getting too big and we decided to split it in two:







  * db_tasks


  * svn_tasks





Without telling the other guys (sorry!), I started [discussing](http://groups.google.com/group/rubyonrails-core/browse_thread/thread/194c845ae5678266) merging some of the rake_tasks plugin in Rails core. 





I then submitted my patch following [Josh Susser](http://hasmanythrough.com/layingtracks) recommendations.





Today, Josh congratulated me on the merge. I'm quite glad/proud that our work made it to Edge.





## What's that patch anyway?





Here are the 2 features my patch added to the future Rails 2.0:







  * MySQL: create_database takes :charset and :collation options. Charset defaults to utf8. 


  * Add db:create, drop, reset, charset, and collation tasks





That means that you cna now do that:




    
    <code>$rails my_app
    $rake db:create
    $ruby script/server
    </code>





That's it, you have a working Rails app!! Well, almost, there's nothing it in yet but at least your databases are created. Ohh, and your database is unicode, that also means that all your new tables will be utf8.





Enjoy :)
