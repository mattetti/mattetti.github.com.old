---
date: '2008-10-13 03:18:46'
layout: post
legacy_url: http://merbist.com/2008/10/13/merb-10-rc1-delayed-by-few-hours-hopefully/
slug: merb-10-rc1-delayed-by-few-hours-hopefully
source: merbist.com
status: publish
title: Merb 1.0 RC1 delayed (by few hours hopefully)
wordpress_id: '162'
categories:
- News
- merbist.com
- blog-post
tags:
- merb
- release
---

[![](http://farm2.static.flickr.com/1235/1410398234_c341320704.jpg?v=0)](http://flickr.com/photos/aihibed/1410398234/) Tonight, the core team members present during MerbCamp got together at my place to prepare and release 1.0 RC1.

The problem is that we spent more time joking, laughing and arguing about the new git system that we didn't feel right about pushing a release at 4am :(

The git -core and -more repos got merged into a centralized [repo](http://github.com/wycats/merb). You will notice an active branch and that's where most of the hacking is going on.

Each core team member has a fork of the project and we all create a new branch everytime we work on a new feature or bug fix. Branch comments get squashed into 1 comment, merged back into the local rebased active branch. The commit get then pulled by Yehuda or pushed to his active branch. Yehuda then make sure all the commits are clean and merge them back in master (edge).

Releases will have their own branches and tags.

People wanting to hack on a specific feature should really fork a core member branch. Usually a core member involved with what you want to work on. Of course that's optional but whatever you do, if you want to hack on Merb, please use the active branch and not master.

It's 4:15am, and we are still cleaning up the last few quirks before the release, I know we planned on a releasing today but I guess it's better to wait few more hours and to get a solid release.

Sorry about that :(
