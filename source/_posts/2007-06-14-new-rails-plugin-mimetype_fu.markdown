---
date: '2007-06-14 04:45:00'
layout: post
legacy_url: http://railsontherun.com/2007/06/14/new-rails-plugin-mimetype_fu/
slug: new-rails-plugin-mimetype_fu
source: railsontherun.com
status: publish
title: 'new rails plugin: mimetype_fu'
wordpress_id: '35'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- attachment_fu
- content type
- flash
- mime
- mime type\'
- mimetype_fu
- plugins
- upload
---

[mimetype_fu/](http://code.google.com/p/mimetype-fu/) is a new plugin I just wrote. It's simple and it can be really useful if you need to get the mime type of a file already on your server.





During one of my project, I add to migrate old assets from a legacy system to a new Rails app. The new app uses [attachment fu](http://svn.techno-weenie.net/projects/plugins/attachment_fu/) and even though techno weenie did an amazing job, attachment_fu validation is based on the content type. A_fu gets the content type coming from the CGI query. 





Unit test has a helper faking this process but in real life, if you use a Flash uploader (Flash doesn't give you the proper mime type/content type) or if you want to migrate files, the attachment_fu validation won't work for you.





The solution is simple: [mimetype_fu/](http://code.google.com/p/mimetype-fu/)





[mimetype_fu/](http://code.google.com/p/mimetype-fu/) extends the File class and is really easy to use:




    
    <code>File.mime_type?(@file)
    </code>





Check it out [http://code.google.com/p/mimetype-fu/](http://code.google.com/p/mimetype-fu/)





Expect a post showing how a ninja would use the mimetype_fu / attachment_fu combo :)
