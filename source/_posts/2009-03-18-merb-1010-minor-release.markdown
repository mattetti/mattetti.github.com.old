---
date: '2009-03-18 17:11:35'
layout: post
legacy_url: http://merbist.com/2009/03/18/merb-1010-minor-release/
slug: merb-1010-minor-release
source: merbist.com
status: publish
title: Merb 1.0.10 (minor release)
wordpress_id: '456'
categories:
- merb
- News
- merbist.com
- blog-post
tags:
- merb
- release
---

We just pushed a really tiny update because of a bug in 1.0.9 affecting people using: Merb::Config[:max_memory]

Merb::Config[:max_memory] has been fixed and now polls for memory usage every 30s instead of 0.25s. (memory is set in KB)


This new version also uses DataMapper.repository instead of Kernel#repository (DM and Vlad related bug fix)




We are still on schedule for Merb 1.1 which is planned for early April. (If you install Merb from our edge server, the latest version should already be Ruby 1.9 compatible)

