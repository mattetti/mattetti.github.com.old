---
date: '2009-03-31 22:10:57'
layout: post
legacy_url: http://merbist.com/2009/03/31/merb-1011-minor-release/
slug: merb-1011-minor-release
source: merbist.com
status: publish
title: Merb 1.0.11 (minor release)
wordpress_id: '462'
categories:
- merb
- News
- merbist.com
- blog-post
---

Following the DataMapper 0.9.11 release, we just pushed a new minor Merb release.

This release is mainly targeting new developers and Windows users wanting to install the full Merb stack. Others can simply update their dependencies if they use the dependencies.rb file or install the new gems if nothing is bundled and no hard dependencies are set.

Merb is a metagem which installs a bunch of other gems (merb-core, DataMapper and a lot of small gems). The problem was that Merb was trying to install DM and dm-types, unfortunately, dm-types had a dependency on a gem which couldn't be installed on Windows. All of that is now fixed and Windows users can install Merb 1.0.11 without having to manually pick the gems they need.

This release also includes a fix for people using CouchRest, a CouchDB Document Mapping DSL.

Merb 1.1 is still planned to be released in April. A majority of the work has been done, but since Yehuda and myself are going to be traveling, the release will be slightly delayed.

The great news regarding Merb 1.1 is that, on top of being fully Ruby 1.9 compatible, and using action-orm, and being closer to Rack, Yehuda and Carl have been working on the router to make it awesomer and ready for mountable apps :)

Stay tuned for more news.

update: People using CouchRest or another CouchDB ORM/DSL make sure you define your resources route with an identifier:

resources :articles,        :identify => :id

Otherwise, resource(@article) won't work. (This is usually done by the merb orm plugin and I might add it to CouchRest in the future)
