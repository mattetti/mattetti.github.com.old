---
date: '2007-03-30 22:40:00'
layout: post
legacy_url: http://railsontherun.com/2007/03/30/migrating-legacy-app-part-1/
slug: migrating-legacy-app-part-1
source: railsontherun.com
status: publish
title: 'Migrating a legacy app to Rails PART I:'
wordpress_id: '159'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- activeRecord
- legacy
- migrating
---

I recently started working on migrating an old PHP based application to a new _sexy_ Rails app. The old model was kind of messy, the usual case of bloated, [feature creep](http://en.wikipedia.org/wiki/Feature_creep) app written by many people without any standard conventions. 





Anyway, I have to migrate users and content from many sites using the legacy app.





# Data Structure





Instead of having 1 code base and 1 database per site, we now only have 1 code base and 1 database for all sites on a server. 





The legacy application had a minimum of 3 databases:
    1. installer database (keeps track of site version, manage upgrades, list sites on a server)
    2. central server database (keeps all the sites details such as billing address, location, contacts, licenses… )
    3. site database (contains users, content, settings)





Our new application, on the other hand, is as mentioned earlier, very _sexy_ and has a nice Model. It is also developed following the [Behaviour-Driven Development](http://behaviour-driven.org/) principles using [RSpec](http://rspec.rubyforge.org/). The best way for me to migrate the legacy data is probably to develop a Rails Plugin with a set of [Rake Tasks](http://www.martinfowler.com/articles/rake.html#RakeTasks) that I could use to migrate my sites automatically.





I therefore decided to start working on my migration plugin… using BDD, with RSpec edge….





The first think I started thinking was: what should I test/spec? how am I access the pile of not organized content from the legacy sites? How am I connect nicely to each of the legacy sites?





# Model Connection





First question by order of priority: how could I leverage the legacy Model by using ActiveRecord? 





I could create a Legacy Model for each table and connect each of them directly to the legacy database. Sounds ok but that means a lot of connections, and every time I would need to access another site, I would have to reconnect all my legacy classes… I was sure there was a better solution… and I was right. After googling for a solution, I noticed that [Dave Thomas](http://pragdave.pragprog.com) had already found a [solution](http://pragdave.pragprog.com/pragdave/2006/01/sharing_externa.html) few months earlier: 




    
    <code>class LegacyBase < ActiveRecord::Base
        establish_connection ...
      end
    
      class LegacyUser < LegacyBase
         ...
      end
    
      class LegacyContent< LegacyBase
        ...
      end
    </code>





And as Dave explains in his post: 





> 
    
> 
> “It turns out that Rails does just about everything lazily. That includes connecting to databases and reflecting on tables to extract the schema (needed to build the internals of the models). This improves performance, but it also makes this hack possible. In general, youâ€™d expect the LegacyBase class to map to a database table called legacy_base. It would, if we ever tried to use it to access data. But because we donâ€™t, and because Rails only reflects on the table the first time a data access occurs, we can safely create an ActiveRecord class with no underlying database table. This scheme lets me specify the legacy connection once, and share that connection between all my legacy models. Itâ€™s tidy, expressive, and saves resources.”    
> 
> 






Great, I had a technical solution to nicely connect to a legacy database.





[Go to Part 2](http://www.railsontherun.com/2007/4/2/migrating-legacy-app-part-2)
