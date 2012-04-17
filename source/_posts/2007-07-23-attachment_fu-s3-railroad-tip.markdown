---
date: '2007-07-23 03:47:00'
layout: post
legacy_url: http://railsontherun.com/2007/07/23/attachment_fu-s3-railroad-tip/
slug: attachment_fu-s3-railroad-tip
source: railsontherun.com
status: publish
title: attachment_fu + s3 + railroad tip
wordpress_id: '66'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- attachment_fu
- aws
- diagram
- graphviz
- railsroad
- s3
- tip
---

Here is another type for attachment_fu users.





This evening I wanted to try [Railroad](http://railroad.rubyforge.org/) to generate one of my app diagram. I alreay wrote my own script generating a .dot file that I usually import in [omnigraffle](http://www.omnigroup.com/applications/omnigraffle/) before exporting a pretty version for my clients. The thing is, my script is quite simple and only covers models while [railroad](http://railroad.rubyforge.org/) also deals with controllers.





Wanting to check on railroad, after installing the gem I typed 




    
    <code> railroad -o models.dot -M
    </code>





and here is the _'pretty'_ error message I got.





![railroad error msg](http://farm2.static.flickr.com/1209/874252844_25e869205e_o.jpg)





No worries, Technoweenie didn't mess up, it's just that I use s3 for storage and his great plugin checks on the environment to load the proper credentials. Since railroad doesn't care about the environment, RAILS_ENV isn't set and my model diagram isn't generated.





To fix this issue, simply type:




    
    <code>$ railroad -o models.dot -M RAILS_ENV='development'
    </code>
