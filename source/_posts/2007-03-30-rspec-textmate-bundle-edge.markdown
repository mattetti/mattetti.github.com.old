---
date: '2007-03-30 04:21:00'
layout: post
legacy_url: http://railsontherun.com/2007/03/30/rspec-textmate-bundle-edge/
slug: rspec-textmate-bundle-edge
source: railsontherun.com
status: publish
title: RSpec Textmate bundle and RSpec edge
wordpress_id: '1'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- BDD
- bundle
- failure
- RSpec
- rspec textmate
- spec
- textmate
---

## UPDATE:


[ Aslak Hellesoy](http://blog.aslakhellesoy.com/) just mentioned in a comment that the trunk version of RSpec bundle for TextMate ( svn://rubyforge.org/var/svn/rspec/trunk ) doesn't require the RSpec Gem installed anymore.
  
  


To install the bundle:

    
    <code>
    cd ~/Library/Application\ Support/TextMate/Bundles/
     svn co svn://rubyforge.org/var/svn/rspec/branches/0.9-dev/RSpec.tmbundle
    </code>



  



* * *


  
  
  


I recently discovered RSpec, well, not really, but I finally got to start using
RSpec.  

For those who don't know
[RSpec](http://rspec.rubyforge.org/) ,
RSpec is a BDD testing framework, or in better words: RSpec is a Behaviour
Definition Framework well suited for practicing Behaviour Driven Development
(BDD) in Ruby.  

  

If you don't know what is BDD, then just check out
[http://behaviour-driven.org/](http://behaviour-driven.org/), in few words, it's TDD (Test Driven Development in better)  

  

Anyway, I'm working on a Rails project where we use the trunk/edge version RSpec
plugin. I'm using my favorite editor:
[TextMate](http://macromates.com/)
and I noticed that the RSpec team created a
[cool
bundle for textmate](http://rspec.rubyforge.org/tools/extensions/editors/textmate.html).  

  

I was quite excited until I tried running a spec and realized that the task
failed giving some errors about some missing methods... Well, the thing is we
replaced "context ... do" by "describe ... do" and "specify ... do" by "it ...
do" see
[David's
post](http://blog.davidchelimsky.net/articles/2007/03/11/describe-it-with-rspec) about this specific change in trunk:  

  

The Textmate bundle uses the ruby gem instead of the plugin and since I'm trying
to use methods only defined in trunk the bundle simply dies on me every time I
try to run my specs.  

  

The only solution for me was to run a trunk version of the RSpec gem, here is
what to do:  

  

Check out RSpec trunk from:
svn://rubyforge.org/var/svn/rspec/trunk  

Check out RSpec trunk into its own project, or
if you're interested in  

using/learning RSpec for a particular Rails
project, consider using  

svn:externals to check out RSpec trunk into your
[RailsRoot]/vendor  

directory:  

  

svn propset svn:externals "rspec
svn://rubyforge.org/var/svn/rspec/trunk"  

vendor  

  

then update to grab the latest code from RSpec
trunk:  

  

svn update
vendor  

  

Next, build the gem.  You have to be
standing in vendor/rspec if you're  

using svn:externals (as described above) or the
root of RSpec if you checked  

it out as its own
project.  

  

rake gem  

  

then install
it:  

  

gem install pkg/rspec-X.X.X.gem (where X.X.X is
the version number reported  

in the output from "rake gem")  

  

---- from
[http://rubyforge.org/pipermail/rspec-users/2006-November/000135.html](http://rubyforge.org/pipermail/rspec-users/2006-November/000135.html)  

  

That's it, now I can run my specs directly from Textmate the same way I was
doing with Unit Test.  

  

( on a different post I'll explain why I couldn't simply run rake:spec )  

  

  

  

  

  

