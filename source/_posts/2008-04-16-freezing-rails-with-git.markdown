---
date: '2008-04-16 08:21:00'
layout: post
legacy_url: http://railsontherun.com/2008/04/16/freezing-rails-with-git/
slug: freezing-rails-with-git
source: railsontherun.com
status: publish
title: Freezing Rails with Git
wordpress_id: '959'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- freeze
- git
- rais
---

As you've [probably heard](http://blog.rubyonrails.com/2008/4/11/rails-premieres-on-github), Rails now moved to [its own GitHub repo](http://github.com/rails/rails).





If, like me you were a heavy [piston](http://piston.rubyforge.org/) user, you are wondering how you will be able to do the same thing if you switch to git. 





First off, you need to know that Piston will soon support git. As a matter a fact it already does. At least you can download a beta version from [François's blog](http://blog.teksol.info/tags/piston).





You can also go with [giston/braids](http://evil.che.lu/projects/braid) which was meant to make the svn/switch easy on you. I heard rumors that [evilchelu](http://evil.che.lu) might not keep on developing this project. You might want to check with him.





Personally I didn't really like using any of these solutions. Rails also came with its' own approach. ([rake rails:freeze:edge](http://github.com/rails/rails/commit/4b17082107aced980fc4b511028ee763247bc5ab))





When I recently worked on [Merb's freezer](http://railsontherun.com/2008/4/15/merb-tip-how-to-freeze-a-project), I discovered the power of [git submodules](http://www.kernel.org/pub/software/scm/git/docs/git-submodule.html).





Submodules allow you to import "modules" from other git repos inside your own repo. Basically they do what piston does for SVN, apart that submodules are built-in git. Of course it has an expected limitation, you can only add git submodules. 





The good news is that Rails moved to git and now you can "freeze" Rails as a submodule and update really easily!





First thing first, you need to move your project to git. If you are not confident it's a good move yet, you can use "git-svn". However, I would personally recommend you don't. I did that for few months and when I finally moved to git only, it was a pain to restructure the entire path of the app.





Anyways, let's say you created a new [github](http://github.com) project and if still wish to use git svn do:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt>


  
    
    <span class="er">$</span> git-svn import svn<span class="sy">:/</span>/path-to-your-svn-repo project-name<tt>
    </tt><span class="er">$</span> cd project-name<tt>
    </tt><span class="er">$</span> git remote add origin git<span class="iv">@github</span>.com<span class="sy">:mattetti</span>/project-name.git<tt>
    </tt><span class="er">$</span> git push origin master<tt>
    </tt>






Your project is now under git, but if you pistonized Rails, you can't update it anymore :(





Do not fear my dear friend and do as follows:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>


  
    
    <span class="er">$</span> rm -rf vendor/rails<tt>
    </tt><span class="er">$</span> git commit<tt>
    </tt><span class="er">$</span> git submodule add git<span class="sy">:/</span>/github.com/rails/rails.git vendor/rails<tt>
    </tt><span class="er">$</span> git submodule init<tt>
    </tt><span class="er">$</span> git commit<tt>
    </tt><span class="er">$</span> 






That's it you are done :)





Next time you want to update just do:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>


  
    
    <span class="er">$</span> cd vendor/rails<tt>
    </tt><span class="er">$</span> git remote update<tt>
    </tt><span class="er">$</span> git merge origin/master






or you can also do






  
    
    1<tt>
    </tt>2<tt>
    </tt>


  
    
    <span class="er">$</span> cd vendor/rails<tt>
    </tt><span class="er">$</span> git pull






(yes, each plugin acts a a normal git repo)





A quick note for gitHub users. If you browse your repo you won't see the vendor/rails folder and might freak out. Don't! Git is smart and wants to stay slim, instead of copying the files over, it just creates a reference to the original repo. If you try to pull your project in another folder you will see that the Rails folder gets created as expected.





Personally, when plugins are not available in a git repo I usually do a simple svn export to my project vendor's folder. If I need to modify one of these plugins, I just import it to [github](http://github.com) and work on it from there.





You might still want to stick to Piston or Braids and that's fine, but now you won't have an excuse not to switch to Git :)





UPDATE: I just found out that Graeme wrote a [nice detailed post](http://woss.name/2008/04/09/using-git-submodules-to-track-vendorrails/) about tracking plugins using git, [check it out](http://woss.name/2008/04/09/using-git-submodules-to-track-vendorrails/)
