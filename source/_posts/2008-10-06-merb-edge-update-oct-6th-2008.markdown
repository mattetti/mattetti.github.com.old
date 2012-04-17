---
date: '2008-10-06 20:50:15'
layout: post
slug: merb-edge-update-oct-6th-2008
status: publish
title: Merb Edge update (Oct 6th 2008)
wordpress_id: '134'
categories:
- Misc
- News
tags:
- edge
- full stack
- head
- merb
- rspec
- testing
---


	
  * Mode changes to init.rb, user updating to a newer version or Merb will need to add

    
    c[:log_file] = Merb.root / "log" / "development.log"


to their init.rb file or config/environments/development.rb for instance. (Newly generated apps are already setup properly)

	
  * We made some changes to the way Rake files work. Merb-core doesn't require the rspec tasks anymore so Test::Unit see annoying rspec tasks. Once again people upgrading to the latest version of Merb need to make a small change and add:

    
    require 'spec/rake/spectask'


to their rake file. Newly generated applications using RSpec already have that line setup.

	
  * In the last few days, [Yehuda](http://yehudakatz.com/) merged in his branch with the new "request-testing feature". This is a new way of testing your apps. It makes testing a real request going through a controller and being rendered in a view, something quite easy, Merb interegration tests here we go!. [Check here](http://gist.github.com/14910) to see a example of what you can now do. Rails + Rspec users might be surprised by this choice, and I've scheduled to interview Yehuda so he can explain why and when you want to use this way of testing your app. (Don't miss his talk at [MerbCamp](http://merbcamp.com) next week)

	
  * Talking about full stack testing, Merb is almost entirely full stack tested. What does that mean? Take a look at [merb-helpers](http://github.com/wycats/merb-more/tree/master/merb-helpers) specs in [merb-more](http://github.com/wycats/merb-more). Form builders are tested through a real app available from spec/fixture/app, the views are rendered in the specs and the results are check to make sure they will work for you in your real application. Avoiding using too many mocks and stubs helped us really test things in the contet of a real app and avoid a great amount of ghost bugs. Specs might run a bit slower but we believe Merb now has better testing suite than before. More coming up about this topic.

	
  * Ohh and we released Merb 0.9.8 "Time Machine", last release before 1.0RC1 ;)


