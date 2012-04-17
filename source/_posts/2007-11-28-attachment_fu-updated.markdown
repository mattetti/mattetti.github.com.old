---
date: '2007-11-28 08:26:00'
layout: post
legacy_url: http://railsontherun.com/2007/11/28/attachment_fu-updated/
slug: attachment_fu-updated
source: railsontherun.com
status: publish
title: Attachment_fu updated!
wordpress_id: '270'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- attachment_fu
- imagescience
- minimagick
- plugin
- rails
- rmagick
- update
---

I recently bugged [Rick Olson](http://techno-weenie.net/) so much about [attachment_fu](http://svn.techno-weenie.net/projects/plugins/attachment_fu/) that he gave me SVN access to fix few bugs.





Rick being really busy with [ActiveReload](http://activereload.net/) he didn't spend too much time maintaining [attachment_fu](http://svn.techno-weenie.net/projects/plugins/attachment_fu/).





On my side of things, I've been using [attachment_fu](http://svn.techno-weenie.net/projects/plugins/attachment_fu/) on a lot of projects and I've been fixing bugs and adding new features.





My first contribution to [attachment_fu](http://svn.techno-weenie.net/projects/plugins/attachment_fu/) is a fix for the [ImageScience](http://seattlerb.rubyforge.org/ImageScience.html) processor.





Attachment_fu is very flexible and let you use your favorite image processor:







  * [RMagick](http://rmagick.rubyforge.org/) based on [ImageMagick](http://www.imagemagick.org/script/mogrify.php) and [GraphicsMagick](http://www.graphicsmagick.org/)(known to leak memory and being a pain to setup)


  * [minimagick](http://rubyforge.org/projects/mini-magick/) based on [ImageMagick](http://www.imagemagick.org/script/mogrify.php)


  * [ImageScience](http://seattlerb.rubyforge.org/ImageScience.html) based on [FreeImage](http://sf.net/projects/freeimage). 





Like many rubyists, I like [ImageScience](http://seattlerb.rubyforge.org/ImageScience.html) for its simplicity and efficiency. However, [attachment_fu](http://svn.techno-weenie.net/projects/plugins/attachment_fu/) had few problems when being used with [ImageScience](http://seattlerb.rubyforge.org/ImageScience.html).







  * File sizes for thumbnails were not saved correctly in the database. Fixed


  * Thumbnails based on a gif files were not processed properly. So, this was the big problem. [FreeImage](http://sf.net/projects/freeimage) has issues dealing with resizing gif files because of the gif palette limitation (256 colors). to avoid this problem thumbnails of gif files are converted to png. However the thumbnail content type info in the database was not saved properly. That's now fixed.


  * Because of the gif bug reported above, any thumbnail link was broken since it was trying to link to the thumbnail version with a gif extension instead of a png one. Fixed





I also fixed a small bug related to [S3 storage](http://www.amazon.com/gp/browse.html?node=16427261) and the fact that a_fu had issues loading the config file. (Fixed)





I'll also try be able to add some of the S3 features I've been working on. As well as maybe enhancing the validation process. 





In the mean time, you might want to read [this blog post](http://the.railsi.st/2007/11/27/roll-your-own-attachment_fu-validations) about better validation with attachment_fu.





If you are heavily using attachment_fu or starting using it and think that a google group would be great idea, please let me know in the comment and I'll try to convince Technoweenie that we need to set that up :)





Ooohh I almost forgot, if you fixed some bugs you found while using a_fu, please contact me so we can get a_fu bug free.
