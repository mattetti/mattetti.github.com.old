---
date: '2007-07-21 03:30:00'
layout: post
legacy_url: http://railsontherun.com/2007/07/21/attachment_fu-flash-and-to_xml-tips/
slug: attachment_fu-flash-and-to_xml-tips
source: railsontherun.com
status: publish
title: attachment_fu, Flash and to_xml tips
wordpress_id: '65'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- api
- attachment
- attachment_fu
- edge
- flash
- to_xml
- upload
- xml
---

I recently had to deal with an interesting challenge. I had to write a simple interface between a rails app and a Flash application. Nothing hard and if you browse the archives, you'll find examples and tutorials on how to create a REST interface to communicate between Rails and Flash.





The thing was that this time I had to interface with a model using [attachment_fu](http://svn.techno-weenie.net/projects/plugins/attachment_fu/). I'm a great fan of a_fu and it's definitely the best way of dealing with uploads. 





My model looked more or less like that:




    
    <code>class Photo < ActiveRecord::Base
    
    belongs_to :user
    
      has_attachment(
        :content_type => :image,
        :storage => :file_system,
        :max_size => 10.megabytes,
        :resize_to => '640x480>',
        :thumbnails => { :thumb => '100x100>',
                                  :preview => '300x200>',
                         }
      )
      validates_as_attachment
      # read more about validates_existence_of http://blog.hasmanythrough.com/2007/7/14/validate-your-existence
      validates_existence_of :user
    end
    </code>





My show action in my photo controller could have looked a bit like that:




    
    <code>respond_to do |format|
      format.html # show.html.erb
      format.xml  { render :xml => @photo }
    end
    </code>





That's great, the problem is that we are displaying a lot of information that our Flash client doesn't need to see, actually we are exposing a lot of information nobody should ever see and we are not displaying what we should. On top of being a waste of bandwidth and giving too much work to the client, we are not actually providing the user with the details of the thumbnail.





The first thing to do would be not to display some of the object attributes, the to_xml method lets you do that. 





Note that in Edge, Rails will automatically try to convert your object using to_xml, you don't even need to mention it. However in our case, we want to use some _advanced_ features offered by to_xml, and here is how our code should look like:  




    
    <code>format.xml do
      render :xml => @photo.to_xml( :except => [ :id, :filename, :updated_at, :user_id, :height, :width], :skip_types => true )
    end
    </code>





What I just did is very simple, we rendered our object as an xml object but we didn't convert few attributes, :id, :filename, :updated_at, :user_id, :height, :width. By default Rails also adds the object type, we don't really need that right now, so let's skip them.  

(The reason why I don't want to convert the filename is that I want to provide our Flash client with the photo thumbnail instead of the original picture.)





As far as I know, to_xml doesn't let you create new attributes. (if I have some time, I'll submit a patch to get that added).





What we are trying to do is to display the avatar of a user. We found the photo record using @user.photo but that's the original photo and we want to provide Flash with the avatar info, not the original.





What we need to do is to simply add a new attribute called avatar:




    
    <code>format.xml do
      @photo[:avatar] = @photo.public_filename(:thumb)
      render :xml => @photo.to_xml( :except => [ :id, :filename, :updated_at, :user_id, :height, :width], :skip_types => true )
    end
    </code>





Simple enough, but it took me a little while to figure it out ;)





Voila, we now have a clean, trimmed and safe XML returned object that you can be consumed by our Flash client. Ohh, and we added a new attribute that the original object didn't have :)
