---
date: '2007-12-10 06:43:00'
layout: post
legacy_url: http://railsontherun.com/2007/12/10/google-chart-gem/
slug: google-chart-gem
source: railsontherun.com
status: publish
title: Google Chart Gem
wordpress_id: '324'
categories:
- Gems &amp; Libs
- googlecharts
- Ruby
- railsontherun.com
- blog-post
tags:
- api
- chart
- charts
- gchart
- gem
- google
- googlecharts
- sexy charts
---

## update Nov 11: the gem is finally available [there](http://rubyforge.org/projects/googlecharts/) or simply:




    
    
      sudo gem install googlecharts
    





Note that I'm working on merging this gem with another Google Charts gem. (see comments for more info about that)





![gchart](http://chart.apis.google.com/chart?chtt=Rails+on+the+run&cht=p3&chs=200x90&chd=s:Hellobla&chl=May|Jun|Jul|Aug|Sep|Oct&chco=0000ff)





I've been working on a Google Chart Gem that I have ready for a beta release but unfortunately, getting a new project setup on RubyForge takes forever. (apparently 72 hours)





It's mainly a wrapper for the great GChart API, but instead of using a helper to generate your graphs, you can simply do:




    
    
      Gchart.bar(:title => 'My Mojo', :data => [1,2,4,67,100,41,234], :max_value => 300, :bg => 'c3c3c3')
      
      Gchart.line(:title => 'My Mojo', 
                  :data => [[1,2,4,67,100,41,234],[41,63,96,17,100,14,423]],
                  :bg => '666666', 
                  :graph_bg => 'cccccc', 
                  :line_colors => 'ff0000,00ff00',
                  :legend => ['morning','evening'])
    




    
    
      Gchart.pie(:data => [20,10,15,5,50], :title => 'SDRuby fu', :size => '400x200', :labels => ['matt', 'rob', 'patrick', 'jordan', 'ryan'])
    






![img](http://chart.apis.google.com/chart?chs=400x200&chd=s:YMSG9&chtt=SDRuby+fu&chl=matt|rob|patrick|jordan|ryan&cht=p)





As far as I know this is most complete Ruby wrapper for Google Chart API, but feel free to look around.
