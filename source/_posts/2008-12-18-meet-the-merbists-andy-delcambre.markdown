---
date: '2008-12-18 04:22:09'
layout: post
legacy_url: http://merbist.com/2008/12/18/meet-the-merbists-andy-delcambre/
slug: meet-the-merbists-andy-delcambre
source: merbist.com
status: publish
title: 'meet the merbists: Andy Delcambre'
wordpress_id: '378'
categories:
- merb
- merbist.com
- blog-post
tags:
- Andy Delcambre
- Meet the merbists
- merb
---



[![Andy Delcambre](http://merbist.com/wp-content/uploads/2008/12/andy_cafe-224x300.jpg)](http://andy.delcambre.com)

Today, [Andy Delcambre](http://andy.delcambre.com) is our featured merbist.




**Matt Aimonetti:** Could you please introduce yourself and tell us what you do for a living.




**Andy Delcambre:** My name is Andy Delcambre and I work for Engine Yard
(http://engineyard.com) as a Software Engineer.  I work primarily on
internal and customer facing projects.  These projects are almost
exclusively written in Merb.  At Engine Yard we take advantage of many
of the modular aspects of Merb.  For example, we use Salesforce for
customer tracking and have a Merb slice which includes the DataMapper
adapters for Salesforce.  This gives us the ability to interact with
salesforce from any application for free.


  



**Matt Aimonetti:** How did you get started with Ruby, and what's your general programming background?




**Andy Delcambre:** I majored in Computer Science at college and had been hearing about
Ruby and Ruby on Rails for a while before I finally dove in.  I worked
for the College of Engineering as a Linux Administrator and took the
opportunity to build my first rails application: a tool for managing
our automated deployments.  This was a huge learning experience, I had
done web development in raw php before, but this was my first time
using a framework.  It was both a bit overwhelming and a breath of
fresh air.  When I graduated from University, I decided to pursue Ruby
and Rails as a career path and have yet to look back.


  



**Matt Aimonetti:** You chose to contribute to Merb;  how and why did that happened?




**Andy Delcambre:** I was working at Planet Argon (http://planetargon.com), a small Ruby
on Rails development company and was looking to do some open source
contributions.  I had been hearing quite a lot about Merb in the rails
community, especially that it was smaller, lighter and faster.  I
decided to look in the bug tracker and see if there was something
small I could tackle and found a bug with the way nil and false
default arguments were handled in merb_action_args.  When my patch was
accepted I was fairly hooked.  I have been submitting small patches
ever since.  I also led the inline documentation team at the Merb
sprint in San Diego during the run up to 1.0


  



**Matt Aimonetti:** Do you have any Merb projects available online we can look at? What's your experience been so far?




**Andy Delcambre:** All of my merb projects are projects at Engine Yard right now.  I
wrote a simple Merb app to manage the content on the Engine Yard
homepage and the blog is based on the Feather blogging engine, written
in Merb.  My current projects are not yet publicly available but
should be very cool when they come out.


  



**Matt Aimonetti:** What is your favorite aspect of the Merb framework?




**Andy Delcambre:** I have two answers, first as a someone who works on merb, then as
someone who works with merb.

Ever since that first patch to action_args, I have loved the ease with
which I can jump in and dig under the hood.  I have dug down into the
router, the dispatcher, the form helpers, and the mailer, all without
much problem understanding the code.

Second, as a developer of merb applications I think the direction merb
is going with the heavy emphasis on modularity is fantastic.  In the
app I am currently working on, we are using four different slices
right now.  One of which is an entirely different merb app modified
slightly and mounted within the application.  One is a custom
merb-auth strategy that we use internally that makes it much much
easier to maintain consistency for our internal applications.  These
would be much much harder to integrate for most other frameworks.  It
is so useful to be able to just plug in these small, specific pieces
of functionality.


  



**Matt Aimonetti:** What parts of Merb you hope to see improved in the near future?




**Andy Delcambre:** I am looking forward to the future improvements to the modularity of
merb.  I am really ready for the day that you can mount entire merb
apps inside one another.  Then, once everything uses merb-auth, I can
imagine mounting a blog and a cms inside the same "app" and have the
users shared across, automatically.  How awesome would that be.


  



**Matt Aimonetti:** Anything else you would like to add?




**Andy Delcambre:** I am looking forward to being part of the merb community as this
project just keeps getting better and kicking more butt.  Thanks a lot
for including me in this series.
