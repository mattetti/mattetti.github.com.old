---
date: '2008-10-05 21:48:24'
layout: post
slug: few-changes-in-merb-edge
status: publish
title: Few changes in Merb Edge
wordpress_id: '124'
categories:
- News
tags:
- config
- edge
- merb
---

I thought I should share few changes that my affect your apps if you want to upgrade to Edge.



	
  * merb_helpers which was previously in [merb-plugins](http://github.com/wycats/merb-plugins/tree/master) now moved to [merb-more](http://github.com/wycats/merb-more/) and got renamed [merb-helpers](http://github.com/wycats/merb-more/tree/master/merb-helpers).Â  What that means for you is that you probably want to install merb-helpers and change the reference in your init.rb from dependencies "merb_helpers" to dependencies "merb-helpers"

	
  * People started reporting problems with templates not being reloaded in dev mode etc.. The reason is that we made some changes to the config/environments/development.rb file few weeks ago and people did not notice. [Here is](http://github.com/wycats/merb-more/tree/master/merb-gen/lib/generators/templates/application/merb/config/environments/development.rb) the new generated [development.rb](http://github.com/wycats/merb-more/tree/master/merb-gen/lib/generators/templates/application/merb/config/environments/development.rb) file. Note the following interesting change:

    
    c[:reload_templates] = true


You will need to update this setting if you want Merb to auto reload your templates.


Also, if you didn't read it yet, go check on Yehuda's explanation of [Merb new master process](http://yehudakatz.com/2008/10/03/merb-master-process/).

Finally, I have a bit of a bad news. We were hoping to release Merb 1.0 final during [MerbCamp](http://merbcamp.com) next week end. Unfortunately it looks like we will only release 1.0RC.

The reason behind this choice is simple, we have been adding a lot of features, fixed a lot of bugs and stabilized the API. However we need more feedback from users to be confident enough to release a final 1.0 release.
