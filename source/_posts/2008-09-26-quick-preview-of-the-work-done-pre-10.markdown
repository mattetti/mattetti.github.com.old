---
date: '2008-09-26 10:59:00'
layout: post
legacy_url: http://merbist.com/2008/09/26/quick-preview-of-the-work-done-pre-10/
slug: quick-preview-of-the-work-done-pre-10
source: merbist.com
status: publish
title: quick preview of the work done pre 1.0
wordpress_id: '82'
categories:
- News
- merbist.com
- blog-post
tags:
- backtraces
- merb
---

Yesterday I spyed on [wycats aka Yehuda Katz](http://yehudakatz.com/) while he's working on cleaning things up for 1.0 which will be released during [MerbCamp](http://merbcamp.com) Oct 11-12.

Here is the screenshot of how you would see ugly backtraces originating from Merb's core:

[caption id="attachment_83" align="aligncenter" width="150" caption="old backtraces"][![old backtraces](http://merbist.com/wp-content/uploads/2008/09/backtraces_before-150x150.jpg)](http://merbist.com/wp-content/uploads/2008/09/backtraces_before.jpg)[/caption]

Note: Did you notice that wycats is using [Ruby Enterprise Edition](http://www.rubyenterpriseedition.com/)? Merb 0.9.8 has been modified to really take advantage of REE!

That's a lot of noise just to tell you that you used an invalid argument

Well, part of the prep work for Merb 1.0 is to clean up this things to make development work easier and nicer. Here is how the same error will look like in 1.0

[caption id="attachment_85" align="aligncenter" width="150" caption="new backtraces"][![new backtraces](http://merbist.com/wp-content/uploads/2008/09/backtraces-after-150x150.jpg)](http://merbist.com/wp-content/uploads/2008/09/backtraces-after.jpg)[/caption]

As you can see, it's way more elegant and less confusing. You can still see the full trace by using the --verbose argument.
