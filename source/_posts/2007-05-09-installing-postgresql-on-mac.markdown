---
date: '2007-05-09 15:51:00'
layout: post
legacy_url: http://railsontherun.com/2007/05/09/installing-postgresql-on-mac/
slug: installing-postgresql-on-mac
source: railsontherun.com
status: publish
title: Installing PostgreSQL on Mac
wordpress_id: '15'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- database
- install
- mac
- macports
- postgresql
- setup
---

I'm working on a Rails project and I need to make sure that our code is compatible with PostgreSQL. I never installed/used before and since I'm lazy and rely on other people knowledge, I decided to install Postgresql using [MacPort](http://blog.duncandavidson.com/2006/04/sandboxing_rail.html).





It was actually simpler than I expected. I simply followed [this post](http://blog.evanweaver.com/articles/2006/06/26/building-ruby-rails-lighttpd-mysql-and-postgres-on-os-x-tiger) and almost everything went ok.




    
    <code>sudo port install postgresql81 +server
    </code>





will install postgresql 




    
    <code>sudo gem install postgres -- --with-pgsql-include-dir=/opt/local/include/postgresql81 --with-pgsql-lib-dir=/opt/local/lib/postgresql81
    </code>





Will do a gem install





Start your server automatically by doing:




    
    <code>sudo launchctl load -w /Library/LaunchDaemons/org.macports.postgresql81-server.plist
    sudo launchctl start org.macports.postgresql81-server
    </code>





Create a folder for your dbs
    mkdir /opt/local/var/db/pgsql/data





Add pgsql to your path (I use textmate)




    
    <code>mate ~/.profile
    </code>





My path looks like that:




    
    <code>export PATH=/opt/local/bin:/opt/local/sbin:/opt/local/apache2/bin:/opt/local/lib/postgresql81/bin/:$PATH
    
    export PGDATA="/opt/local/var/db/pgsql/data"
    </code>





restart your shell (or open a new tab) and type
    initdb -D /opt/local/var/db/pgsql/data





Success. You can now start the database server using:




    
    <code>postmaster -D /opt/local/var/db/pgsql/data
    </code>





or
    pg_ctl -D /opt/local/var/db/pgsql/data -l logfile start 





Now, create your db: $ createdb test -E utf8    or drop your newly created db: $ dropdb test





That's it, it was easy.





[more info from apple](http://developer.apple.com/internet/opensource/postgres.html)
