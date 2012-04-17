---
date: '2008-02-29 22:40:00'
layout: post
legacy_url: http://railsontherun.com/2008/02/29/resolving-git-svn-conflicts/
slug: resolving-git-svn-conflicts
source: railsontherun.com
status: publish
title: resolving git-svn conflicts
wordpress_id: '648'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- git
- git-svn
- repository
- scm
- subversion
- svn
---

I've been using [git](http://git.or.cz/) and [git-svn](http://www.kernel.org/pub/software/scm/git/docs/git-svn.html) for a little while and never had a problem... until today.





On one of my [project](http://gumgum.com), we have a SVN repo but since I prefer using Git, I'm using git-svn.





Git-svn has been great, it let me create my own local branches for each new set of features (that's when I don't forget to create a branch) and to commit all the changes back to svn.





The problem today happened after I did a simple git-svn rebase. I had some sort of error and my local repo looked like it got reverted to the head of the svn repo.... 




    
    <code>error: patch failed: trunk/app/models/view.rb:1
    error: trunk/app/models/view.rb: patch does not apply
    [blah blah]
    sing index info to reconstruct a base tree...
    Falling back to patching base and 3-way merge...
    </code>





I hadn't committed to SVN for 24 hours and had a lot of work that was just checked in locally... You can imagine the panic.  [Rob](http://notch8.com) started digging in the .git repo to finally find the hash representing the latest delta before the rebase. With the help of the #caboose guys, I did a simple




    
    <code>git reset --hard hash-name
    </code>





Which restore my repo to the pre SVN commit state. Awesome... however I still had issues to commit my stuff. After a little while I as able to commit again, worked a bit more and tried to commit again.... same error :(





But this time I noticed I could simply do




    
    <code>git rebase --abort
    </code>





to restore the original branch.an 





But I still couldn't commit properly... until I discovered that I just needed to fix the conflicts manually using




    
    <code>git-mergetool
    </code>





git-mergetool uses whichever merge tool available: kdiff3 tkdiff xxdiff meld gvimdiff opendiff emerge vimdiff filemerge





I fixed my conflicts in no time, then did a 




    
    <code>git rebase --continue
    </code>





and finally




    
    <code>git-svn dcommit
    </code>





Looking back, I wish I knew how to properly deal with conflicts when using git-svn, I wasted a bit of my precious time ;)  hopefully this post will help you.





p.s:  [here](http://brian.maybeyoureinsane.net/blog/2008/01/31/git-sake-tasks/) is an interesting use of Sake to handle git-svn
