---
date: '2008-04-15 07:42:00'
layout: post
legacy_url: http://railsontherun.com/2008/04/15/merb-tip-how-to-freeze-a-project/
slug: merb-tip-how-to-freeze-a-project
source: railsontherun.com
status: publish
title: Merb tip - how to freeze a project?
wordpress_id: '944'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- freezer
- merb
- merb-freezer
---

**THIS CONTENT is OUTDATED and can't be used with Merb 1.0 or more recent**





[Merb](merbivore) doesn't really have a core team per say. It's actually managed the same way [Rubinius](http://rubini.us) is managed meaning that few people such as [Ezra](http://github.com/ezmobius), [Wycats](http://github.com/wycats) and [Ivey](http://github.com/ivey) lead the development while many other contributors have commit rights to the different repos.





Patches are handled via [GitHub pull Request](http://github.com) and [LightHouse tickets](http://merb.lighthouseapp.com/). Read the following [contribution documentation](http://merbivore.com/contribute.html) for more info.





![vanilla ice](http://www.music-lyrics-chord.com/cover/Vanilla_Ice_Cool_as_Ice.jpg)





Anyway, I've been working on a [merb-more](http://github.com/wycats/merb-more) gem called [merb-freezer](http://github.com/wycats/merb-more/tree/master/merb-freezer).





We don't have a logo for the plugin yet so I picked a "cool" star from the late 80's to represent.





_Vanilla Ice!_









Right, so you might wonder what's the connection between a [kitsch](http://en.wikipedia.org/wiki/Kitsch) white rapper and a new [Merb](merbivore) Gem? Not much, apart that they are both cool (or kinda cool).





Let's forget about "ice ice Baby" and focus on [merb-freezer](http://github.com/wycats/merb-more/tree/master/merb-freezer).





[merb-freezer](http://github.com/wycats/merb-more/tree/master/merb-freezer) has a simple goal: let you "freeze" your application and run it without dependencies. 





## Why would you want to freeze your app?





_(This is only valid for Merb 0.9.3 and 0.9.2 edge as of April 14)_







  * You might have multiple applications on the same server/slice/cluster. Different applications might require different versions of Merb or some other Merb gems.



  * You might work with a team of developers and want everyone to be using the same version of the gems.



  * You are using Merb Edge and want to make sure that your coworkers are developing/testing against the same revision.






## How to freeze your app?





First thing, in your init.rb file you need to require merb-freezer






  
    
    <tt>
    </tt>


  
    
    require <span class="s"><span class="dl">'</span><span class="k">merb-freezer</span><span class="dl">'</span></span>






Now that you required the plugin when you get new rake tasks:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt>


  
    
    <tt>
    </tt>  rake freeze<span class="sy">:core</span>            <span class="c"># Freeze core from git://github.com/wycats/merb...</span><tt>
    </tt>  rake freeze<span class="sy">:more</span>            <span class="c"># Freeze more from git://github.com/wycats/merb...</span><tt>
    </tt>  rake freeze<span class="sy">:plugins</span>         <span class="c"># Freeze plugins from git://github.com/wycats/m...</span><tt>
    </tt>






The rake freeze tasks use by default git submodules to freeze the various components. That means that you need to have your project under git to use that feature. However, if you didn't switch to git yet, do no worry, we have a plan B.





When you run the freeze tasks a framework directory is created at the root of the folder and the gems are checked out there.





Not a git user? We thought of you and added an option for you to use rubygems. 






  
    
    1<tt>
    </tt>2<tt>
    </tt>


  
    
    <tt>
    </tt>  rake freeze<span class="sy">:core</span> <span class="co">MODE</span>=rubygems<tt>
    </tt>






When doing that, you are freezing the latest version available on the rubygems server or installed locally.
Note that when using the default mode, you are pulling the latest version of Merb from the official git repo. If you want to do that using rubygems you will need to checkout the git repos locally and install the gems yourself before freezing them.





Also it's worth noting that you can also do all of that manually, as long as you follow the same conventions you should be fine.





## How to use the freezing gems?





If you are a Rails user, you might expect that Merb uses the frozen gems by default, at least that what I expected. Turns out, it's not the case since Merb avoids too much magic and unless you ask for it, Merb will use the available system gems.





So how to start a frozen merb app? Easy enough:






  
    
    <tt>
    </tt>


  
    
    frozen-merb






That's it, you can uninstall merb-core from your system and as long as you froze merb-core, you can start your app.





## How to update a frozen app?





simply re-freeze but with the UPDATE=true param






  
    
    1<tt>
    </tt>2<tt>
    </tt>


  
    
    <tt>
    </tt>  rake freeze<span class="sy">:core</span> <span class="co">UPDATE</span>=<span class="pc">true</span><tt>
    </tt>






This option works for in both modes (rubygems and git submodules).





## What's next?





I'd like to add a locking mechanism that would allow you to force your app to only run on specific versions of few gems. The main advantage of this approach is that you wouldn't need to freeze files in your repo as long as you have the required versions on your machine. 





I would also like to extend the freezer to let you use the submodules to freeze other gems.





Other suggestions? Found a bug? Want to submit a patch?  Leave me a comment or use [the Merb LightHouse ticketing system](http://merb.lighthouseapp.com/)
