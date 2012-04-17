---
date: '2007-05-11 18:11:00'
layout: post
legacy_url: http://railsontherun.com/2007/05/11/note-to-myself/
slug: note-to-myself
source: railsontherun.com
status: publish
title: note to myself
wordpress_id: '18'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- activeRecord
- distinct
- mapping
- rails
- tip
- unique
---

This morning I couldn't remember how to get an array with only the unique properties of a Model attribute. For instance, I have a User model, and I want to retrieve the list of all my users' cities. I also don't want to retrieve all the attributes and only the unique rows.





in my model:




    
    <code>def self.cities
        User.find(:all, :select => "DISTINCT city").map(&:city)
    end
    </code>





Clean and simple, but for some reason I know I'll forget and loose 5 minutes trying different things. 
