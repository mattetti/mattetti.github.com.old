---
date: '2010-01-18 16:13:37'
layout: post
slug: how-to-detect-cylons-with-macruby
status: publish
title: How to detect Cylons with MacRuby
wordpress_id: '696'
categories:
- blog-post
- macruby
tags:
- cylon technology
- debugger
- macruby
---

Over the weekend, MacRuby's trunk became version 0.6 and the bug fixing is currently done in both the 0.5 branch and trunk. Based on MacRuby's usual release cycle I would expect a 0.5 beta3 or 0.5 final to be released soon so most of the work can be focused on trunk.

I'll let you check on the [TODO list](http://svn.macosforge.org/repository/ruby/MacRuby/trunk/TODO) to see what was done in 0.5 and what is in the plan for 0.6.

However, there is one feature in 0.6 that I know lots of you will just love! The good news is that Laurent already committed a very early version of his work so I figured, I should share the good news with you:


### Introducing MacRuby's debugger!



If you were expecting to read: "a Cylon detector!", keep on reading.

Again, this feature is in really early development since it's scheduled for 0.6 and 0.5 final is not out yet. But if you install MacRuby using the [nightly builds](http://macruby.icoretech.org/) or build from trunk, you can already play with the debugger.

Let me give you a really quick tour of the debugger.  


![BSG75Logo posted by Matt Aimonetti](http://img.skitch.com/20100118-qn7yuq9h2ce61yxt6kwag9pnbt.jpg)

Let's imagine that we were given the task to debug the cylon detector written by Gaius Baltar which looks like that:

    
    characters = %w{Adama Apollo Baltar Roslin StarBuck Six}
    
    def cylon?(character)
      false
    end
    
    characters.each do |character|
      if cylon?(character)
        puts "#{character} is a Cylon!"
      else
        puts "#{character} is not a cylon."
      end
    end
    



Here is what happens when I execute the script:


    
    
    $ macruby cylon_detector.rb 
    Adama is not a cylon.
    Apollo is not a cylon.
    Baltar is not a cylon.
    Roslin is not a cylon.
    StarBuck is not a cylon.
    Six is not a cylon.
    



The only problem is that we all know that Six is a Cylon, the detector isn't working right so let's debug it:


    
    
    $ macrubyd cylon_detector.rb
    Starting program.
    cylon_detector.rb:1> b cylon_detector.rb:8 if character == 'Six'
    Added breakpoint 1.
    cylon_detector.rb:1> c
    Adama is not a cylon.
    Apollo is not a cylon.
    Baltar is not a cylon.
    Roslin is not a cylon.
    StarBuck is not a cylon.
    cylon_detector.rb:8> p cylon?(character)
    => false
    cylon_detector.rb:8> p "This detector is broken!"
    => "This detector is broken!"
    cylon_detector.rb:8> p def cylon?(character); character == 'Six'; end
    => nil
    cylon_detector.rb:8> p cylon?(character)
    => true
    cylon_detector.rb:8> p cylon?('Matt')
    => false
    cylon_detector.rb:8> c
    Six is a Cylon!
    Program exited.
    



The first thing we do is to add a conditional breakpoint: 

    
    
    b cylon_detector.rb:8 if character == 'Six'
    



Basically, the debugger will add a breakpoint at line 8 which will only be active when the value of 'character' is equal to 'Six'. 
Now that the breakpoint added, we can continue the program execution and just wait until we reach the defined condition.


    
    
    cylon_detector.rb:1> c
    



Once we reach the breakpoint, we evaluate the result of "cylon?(character)" by using the p command. We see that the result is "false" when we know for sure that it should be true since the value of the character variable is 'Six' and she is a cylon. At this point, you might have guessed that somewhat acted as a cylon agent and I pretended to fix the problem by overwriting the "cylon?" method:


    
    
    cylon_detector.rb:8> p def cylon?(character); character == 'Six'; end
    



Now that the method is overwritten, I can check that Six is recognized as being a cylon: 

    
    
    cylon_detector.rb:8> p cylon?(character)
    => true
    



and also check that I am not detected a cylon:

    
    
    cylon_detector.rb:8> p cylon?('Matt')
    => false
    



I can now continue the execution of the program and see that Six is detected as a Cylon!



Of course this is just a very early version of the debugger and we will see lots of improvement in the next few weeks. Who knows someone might even create a GUI for the debugger and/or a Xcode integration. 

Anyway, the point being that MacRuby developers should expect a lot of awesome stuff coming up their way soon. (also be careful about the skin jobs around you, cylon detectors can't be trusted!)
