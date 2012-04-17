---
date: '2009-03-02 11:20:34'
layout: post
legacy_url: http://merbist.com/2009/03/02/merb-11-roadmap/
slug: merb-11-roadmap
source: merbist.com
status: publish
title: Merb 1.1 roadmap
wordpress_id: '448'
categories:
- merb
- News
- merbist.com
- blog-post
tags:
- merb
- release
---

Yesterday, Carl Lerche, Yehuda Katz and myself had a meeting to discuss Merb 1.1's roadmap.

Key items on the agenda were:



	
  * **Ruby 1.9**

	
  * **Mountable apps**

	
  * **migration path to Rails3**


After spending some time arguing back and forth, we decided that few things had to happen before we could migrate the current slices to pure mountable apps. Freezing the releases while waiting to get that done doesn't seem like a good idea.


### Therefore, here is the plan for Merb 1.1:





	
  * **Ruby 1.9 full compatibility** (with the very appreciated help from [Maiha](http://twitter.com/maiha) and [Genki](http://blog.s21g.com/takiuchi) (preview of their work [there](http://merbi.st))). Because Merb depends on different gems, we also need to work with 3rd party developers to make sure Merb's dependencies are Ruby 1.9 compatible



	
  * **Merb helpers** (fixes, enhancement and missing helpers)



	
  * **Make Merb controllers, rack endpoints**. This is a fully transparent change for the framework users. By making this switch, we offer more flexibility to the router (you can mount a sinatra app for instance) and we adopt the same approach as Rails 2.3 making the transition to 3 much easier and facilitating the implementation of mountable apps. Again, this is an internal change and you won't have to change anything in your application.



	
  * **Router optimization**, Carl has been working on few tricks/optimizations for the router that will be available in 1.1



	
  * **Namespacing**. If we want to make every single application, a potential mountable app, we need to namespace our applications. This is something we already do with slices, but currently generated applications are not namespaced. We are planning on doing that for 1.1 (backward compatible) to make mountable apps easier.



	
  * **ActiveORM**. ActiveORM is an ORM abstraction layer developed by Lori Holden (AT&T interactive) which helps with helpers and other parts of your code accessing your ORM directly. For instance, the errors_for method need to be implemented differently depending on the underlying ORM. ActiveORM offers mapping for the 3 major Ruby ORMs: ActiveRecord, DataMapper and Sequel but let you hook to it if you want to extend ActiveORM to support your own ORM.


There is plenty to do but we decided to still try to have an expected release date: around the end of March. As always in the OSS world, this is something we hope for, not a promise ;)


### What about Merb 1.2?


1.2 will focus on mountable apps and we hope to get started on a separate branch before we release 1.1. However, mountable apps are hard to spec and we need a better feedback from the community. Tell us what you like with slices and what you don't like. Let us know how you would like the new mountable apps to work. Be as precise as possible. (you can leave a comment here or on the mailing list)
