---
date: '2008-03-03 08:22:00'
layout: post
legacy_url: http://railsontherun.com/2008/03/03/how-to-use-github-and-submit-a-patch/
slug: how-to-use-github-and-submit-a-patch
source: railsontherun.com
status: publish
title: How to use github and submit a patch
wordpress_id: '668'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- contribute
- git
- github
- merb
---

If you don't know about [git](http://git.or.cz/) and [github](http://github.com) yet, it's time you clean up your RSS feeds and find some good source of information.





[Github](http://github.com) is used by the [Merb core team](http://merbivore.com) and I'll show you how to use github to fork Merb, make your modifications and "submit your patch".





This is the exact reason why github is simply awesome, it makes forking projects just super simple and submitting changes even easier.





First thing, you need to have a github account, if you don't have one yet, email me, I have a couple of invitations left, otherwise, just wait until github gets public.





Now, let's go to [Merb's repository](http://github.com/wycats/merb-core/tree/master) and fork Merb-core by clicking on the fork button.





![fork merb](http://img.skitch.com/20080303-q2aceaadcqk59kqck4b199bdmh.jpg)





Actually, for this example, I'll fork [merb-plugins](http://github.com/wycats/merb-plugins/tree/master) because I want to improve the ActiveRecord rake tasks.





Because I forked merb-plugins, I now have my own forked repo: !![my forked repo](http://img.skitch.com/20080303-g8jas46gq1enn9fhu1t7rsyunj.jpg)





I'll start by checking out/cloning my forked repo locally.






  
    
    <tt>
    </tt>


  
    
    git clone git<span class="iv">@github</span>.com<span class="sy">:mattetti</span>/merb-plugins.git






!![git clone](http://img.skitch.com/20080303-rf87t44c3c6m6hqy5b38ksycua.jpg)





Great I can now make my own changes.... but wait, what if the merb core team makes a change to the code? Well, I need to track their changes. Here is how:






  
    
    <tt>
    </tt>


  
    
    git remote add coreteam git<span class="sy">:/</span>/github.com/wycats/merb-plugins.git






FYI, it adds the following to edit .git/config:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt>


  
    
    <tt>
    </tt>  [remote <span class="s"><span class="dl">"</span><span class="k">coreteam</span><span class="dl">"</span></span>]<tt>
    </tt>  url = git<span class="sy">:/</span>/github.com/wycats/merb-plugins.git<tt>
    </tt>  fetch = +refs/heads/*<span class="sy">:refs</span>/remotes/coreteam/*<tt>
    </tt>






then 






  
    
    <tt>
    </tt>


  
    
    git fetch coreteam






and finally






  
    
    <tt>
    </tt>


  
    
    git checkout -b coreteam coreteam/master






You can now track the latest change and merge them with your branch. Note that you can also track other forks and merge some other changes. (just that feature is worth using git)





Alright, now you can do your stuff, and push your local change to your remote repo at github.





Once you are done, you can simply click on the "pull request button"





![pull request button](http://img.skitch.com/20080303-xxh5d3a2uu65ksf8u4i9tga59e.jpg)





fill up the form and select the recipient. (wycats in this example if you want him to merge your changes into the official version of Merb).





![pull request](http://img.skitch.com/20080303-dei7bxpt577gqe3d1x4ncpmq6w.jpg)





p.s: The github guys are working on a gem to make our loves easier, give [http://github.com/defunkt/github-gem/tree/master](http://github.com/defunkt/github-gem/tree/master) a try. I'll post about the gem when it will be a bit more stable.
