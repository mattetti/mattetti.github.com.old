---
date: '2011-08-30 17:20:34'
layout: post
legacy_url: http://merbist.com/2011/08/30/deploying-a-rails-3-1-app-gotchas/
slug: deploying-a-rails-3-1-app-gotchas
source: merbist.com
status: publish
title: Deploying a Rails 3.1 app - gotchas
wordpress_id: '1115'
categories:
- rails
- merbist.com
- blog-post
tags:
- rails
---

Recently I had to build a new app as part of my research & development job at [LivingSocial](http://livingsocial.com). My goal was to get the app up and running in just a few weeks, solid application architecture and graphic design included.
When you need to build an app quickly and you want it to have some solid foundations, Rails is quite useful.
I used Rails 3.1RCx so if we would to keep my app and push it to production, the engineering team wouldn't have to update it and the transition should be seamless. I also quite like [CoffeeScript](http://jashkenas.github.com/coffee-script/) and the app being quite heavy on JavaScript, the choice was easy. Furthermore, my coworker [Bruce Williams](http://codefluency.com/) is a fan of [SCSS](http://sass-lang.com/) and he's writing a [PragProg](http://pragprog.com/) book called "The Rails View" with other LivingSocialist: [John Athayde](http://www.boboroshi.com/). So you got the point, I'm using Rails3.1, but this post is about the challenges I faced when it was time to deploy and the solutions I found.

I'll skip the intro to Rails 3.1 and how to use the new asset pipeline, refer to the [Rails guide](http://guides.rubyonrails.org/asset_pipeline.html) or one of the mainly posts referenced in t[his post](http://jasonrudolph.com/blog/2011/06/06/helpful-resources-for-upgrading-to-rails-3-1/) (if I had properly read the [guide](http://guides.rubyonrails.org/asset_pipeline.html), it would have saved me some valuable time, trust me, read it carefuly).

At that point last night, I had my app working great locally, Bruce created some awesome scss code using mixins and nested rules, the HTML was clean and working great, my [CoffeeScript](http://jashkenas.github.com/coffee-script/) was brewing nicely, all was great until I tried to deploy to our QA environment.


### JavaScript runtime dependency


The first thing you will notice is that you need the proper JavaScript runtime so the asset pipeline works properly. Not a big deal, you'll find a lot of documentation about that. The problem is that you need to update your production environment or use depend on gem that will compile the required runtime (sounds dirty to me). So if you are deploying to many machines and you are using an image solution (EC2 AMI or other), you will need to update your image or spin new instances via updated chef/puppet recipes. In this case, the awesome team at LivingSocial had an image ready for me, so that wasn't a big deal, but still, you need to take that in consideration as you are planning to update.

So the asset pipeline optimizes your asset management by processing/compiling asset files for you and optimizing their delivery. Instead of serving static files directly via public/images or public/javascripts you know serve them via the asset pipeline which will take care of compiling your CoffeeScript, grouping and minifying your JS and preprocessing all sorts of format. It also optimizes the caching process by giving a unique filename to each file based on the file metadata and gziping files. This is great, but you really, really, really don't want to have your apps take care of that in production. Why wasting precious resources to serve assets when they can be prepared ahead of time. (by making Rails serve static assets, you are seriously reducing the throughput of  your app, please think of the children (or the dolphins/trees if you don't like children))


### Capistrano


Rails obviously has a preprocessor available as a rake task and you should update your deployment recipe to use that new feature. Here is my Capistrano code:

    
    after 'deploy:update_code', 'deploy:compile_assets'
    namespace :deploy do
      task :compile_assets do
        run "cd #{release_path}; RAILS_ENV=production rake assets:precompile"
      end
    end


Well, my real code doesn't hardcode the RAILS_ENV constant value, it's in fact set in each env file, but I simplified it since most people only use 1 env outside of dev & test.

What that will do is compile all the files and dump them in public/assets/. But the file I had called bubble.png now becomes bubble-27543c671a3ab45141ee0d3216085009.png which means that my app is totally broken because images use in Bruce SCSS don't load, my js files don't load and the app is totally broken. Now this is least fun part, that I wish I had known before. This is where you go back and change your code so it uses magic to get the right file names.


### Images


Fixing images was actually quite simple, in all my views, I just had to make sure I was using the image_tag helper everywhere.


### CSS


SCSS files were a bit more tricky, I had to use the new scss preprocessor helpers you will find in the [Rails guide](http://guides.rubyonrails.org/asset_pipeline.html) (image_path and image_url). I first looked into using erb, but turned out it wasn't needed and the end result is much cleaner.


### Javascript/CoffeeScript


For the CoffeeScript files, I was referring to image assets in the code and of course all the links were broken. So I had to use ERB in my coffee which looked funky but it worked.

But to get that to work, you need to rename your coffee script and append erb at the end. For instance my feature.js.coffee script had to be renamed feature.js.coffee.erb. That made me cry a little inside, but oh well, at least its not a XML config file. Maybe soon we will start seeing code in filenames or filenames called my_feature.js.compressed.minified.coffee.erb.from_rails.mattetti.org
Also, be careful about the order of the file extensions, otherwise it won't work. I thought I was done, ready to deploy my apps and this time the assets will show up properly. Turns out I was wrong :(


### Rails asset precompilation env specific configuration.


My css looked good, the precompiling task had run fine but I was missing some js files. I scratched my head as I could only see some of my js files. I then realized that all my JS files were there but some of my CoffeeScript files were missing. The answer was given to me by Bruce who asked me if I had updated my "config.assets.precompile" setting. Sometimes I feel that Rails is trying to compete with Struts and here I was really surprised that by default Rails, in production mode only precompiles all static JS and application.js files, but none of the other dynamic js files. Now it does precompile all the scss files, but for a reason I just don't understand, it's not the case for the JS files. So, you have to go edit production.rb in the config/environments folder and add the other js files you would like Rails to precompile for you.

After making all these changes, I was able to redeploy my app and everything was working again. (you might want to tweak your apache/nginx config as explained in the [Rails guide](http://guides.rubyonrails.org/asset_pipeline.html))

 


### Conclusion


Don't be fooled like me and expect that because you have an app running locally, deployment will work right away. Make sure to read about the new features and what's needed. Overall, I think that the asset pipeline is a nice addon to Rails and if you don't feel like using it, just can put/leave all your files in the public folder and everything will work just like before. I do have to say that I was surprised to see that even in a brand new Rails 3.1 project, Rails isn't running in threaded mode by default. But that's a different (old) story and I guess people still get more excited about asset management than framework raw performance ;)

 

 

 
