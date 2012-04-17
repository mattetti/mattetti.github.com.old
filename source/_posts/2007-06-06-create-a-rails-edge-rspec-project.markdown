---
date: '2007-06-06 19:34:00'
layout: post
legacy_url: http://railsontherun.com/2007/06/06/create-a-rails-edge-rspec-project/
slug: create-a-rails-edge-rspec-project
source: railsontherun.com
status: publish
title: Create a Rails Edge / RSpec project
wordpress_id: '31'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- autotest
- BDD
- edge
- growl
- hackfest
- rails
- RSpec
- svn
- TDD
- texmate
- zentest
---

Recently I helped few people moving to [Rails Edge](http://dev.rubyonrails.org/) and start using [RSpec](http://rspec.rubyforge.org). I realized that I learned few tricks and even if for me everything seemed quite simple, things are not that simple when you recently started with Rails.





This would work on Mac and Linux, sorry Windows users, you'll have to slightly change the code below.





# Live on the Edge





This is actually a bit tricky but it was very well covered by [John Nunemaker](http://addictedtonew.com/about/) in [this post](http://railstips.org/2007/5/31/even-edgier-than-edge-rails)





To sum John's post:





Create a normal rails project:




    
    <code>$ rails my_project
    $ cd my_project
    
    $ rake rails:freeze:edge
    </code>





(or use checkout edge in the vendor folder)




    
    <code>$ cd ..
    $ ruby my_project/vendor/rails/railties/bin/rails my_edgie_project
    </code>





You now have have a real Edge project called my_edgie_project. You can delete the my_project folder since we only used it to create our real edge project.





Now, we are not really done since we need to add the Edge files into our vendor folder so we don't use our local rails gem.





I would refer to another post from John, that you can find [there](http://railstips.org/2007/3/5/my-local-rails-setup)




    
    <code>$ mkdir ~/rails
    $ cd ~/rails
    $ svn co http://dev.rubyonrails.com/svn/rails/trunk .
    </code>





We just created a rails folder called rails in our home folder and we checked out edge/trunk in it.
Now let's go in our Rails app and setup a symlink to the trunk folder we just created.




    
    <code>$ cd ~/rails_projects/my_edgie_project
    $ ln -s ~/rails/trunk vendor/rails
    </code>





If you are using subversion, you can ignore the symlink so it doesnâ€™t try to version it:




    
    <code>$ svn propset svn:ignore "rails" vendor/
    $ svn commit -m "using new sweet rails setup as recommended by John Nunemaker"
    $ svn up
    </code>





[Read more](http://railstips.org/2007/3/5/my-local-rails-setup) for advanced settings etc...





# Install RSpec





Very straight forward, you just need to follow [the documentation](http://rspec.rubyforge.org/documentation/rails/install.html)




    
    <code>$ cd ~/rails_projects/my_edgie_project
    $ ruby script/plugin install svn://rubyforge.org/var/svn/rspec/tags/CURRENT/rspec
    $ ruby script/plugin install svn://rubyforge.org/var/svn/rspec/tags/CURRENT/rspec_on_rails
    </code>





If you have TextMate, you might want to download the [latest RSpec-X.Y.Z.tmbundle.tgz Bundle](http://rubyforge.org/frs/?group_id=797)





Next thing you want to do is to install [ZenTest](http://www.zenspider.com/ZSS/Products/ZenTest/)




    
    <code>$ sudo gem install ZenTest
    </code>





Make sure you install all the required packages.





If you are using [Growl](http://growl.info/) create a new file called .autotest in your home directory:




    
    <code>$mate ~./autotest
    </code>





and add the following 2 lines to be warned when your specs/examples fail/pass




    
    <code>require 'autotest/redgreen'
    require 'autotest/growl'
    </code>





Now, lets go back to our project and create a model using the rspec scaffold (it uses scaffold_resource generator and create all the specs for you)




    
    <code>$ cd ~/rails_projects/my_edgie_project
    $ ruby script/generate rspec_scaffold User first_name:string last_name:string :age:integer
    </code>





Now, let's start autotest (from zentest) so out code is tested in the background




    
    <code>$ autotest
    </code>





There you go, really to modify your RSpec examples, make them fail, fix your code, examples should pass, refactor your code and start again :)
