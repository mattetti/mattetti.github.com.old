---
date: '2007-07-15 05:37:00'
layout: post
legacy_url: http://railsontherun.com/2007/07/15/irb-tricks/
slug: irb-tricks
source: railsontherun.com
status: publish
title: irb tricks
wordpress_id: '54'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- irb console tips tricks history wirble newbies
---

I recently had a discussion with someone(not really familiar with Ruby and its frameworks)  and this person didn’t realize the power of IRB and the Rails console.





![irb](http://myskitch.com/matt_a/irb-20070714-223658.jpg)





Not, this irb, the Ruby IRB. According to [wikipedia](http://en.wikipedia.org/wiki/Interactive_Ruby_Shell) IRB is a shell for programming in the object-oriented scripting language Ruby. IRB is run from the command line and allows the programmer to experiment with code in real time. It allows you to enter Ruby commands at the prompt and have the interpreter respond immediately. It features command history, line editing capabilities, and job control, and is able to communicate directly as a shell script over the Internet and interact with a live server. It was developed by Keiju Ishitsuka.





IRB is awesome and the Rails developers knew it and extended it by creating the Rails console. (come on, you know: $ ruby script/console )





The only issue I have with the Rails console is that it doesn’t have a history. Not really a big deal but don’t worry the console can be extended.





[IRB tips and tricks](http://wiki.rubygarden.org/Ruby/page/show/Irb/TipsAndTricks)





You can also decide to install [wirble](http://pablotron.org/software/wirble/) a gem compiling most of the tricks from rubygarden.org





Simply do:




    
    <code>sudo gem install wirble
    </code>





then edit your ~/.irbrc file and add the following:




    
    <code># load libraries
    require 'rubygems'
    require 'wirble'
    Wirble.init
    </code>





or if you want the fancy colors (I personally don’t like using them), your ~/.irbrc file should look like:




    
    <code># load libraries
    require 'rubygems'
    require 'wirble'
    
    # start wirble (with color)
    Wirble.init
    Wirble.colorize
    </code>





Start your console and you can now use the history by pressing up.





[read more about wirble](http://pablotron.org/software/wirble/README)





# Tricks I use on a regular basis:





Start a console in a different environment:




    
    <code>$ ruby script/console production
    </code>





Reload the console after you modified a class




    
    <code>>> reload!
    </code>





Rollback changes on exit:




    
    <code>$ script/console --sandbox
    </code>





or




    
    <code>$ script/console -s
    </code>





and if feel like a ninja, try the combo which will load the production environment in sanbbox : 




    
    <code>$ script/console production -s
    </code>





![ninja combo](http://myskitch.com/matt_a/console-ninja-combo-20070714-231517.jpg)





Most Rails developers are really familiar with everything I explained in this post but I realized that too many new comers ignore the great tools provided to them for free. Hopefully this post will help few newbies ;)
