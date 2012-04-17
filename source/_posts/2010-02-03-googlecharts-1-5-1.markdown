---
date: '2010-02-03 21:18:23'
layout: post
legacy_url: http://railsontherun.com/2010/02/03/googlecharts-1-5-1/
slug: googlecharts-1-5-1
source: railsontherun.com
status: publish
title: Googlecharts 1.5.1
wordpress_id: '1722'
categories:
- Gems &amp; Libs
- googlecharts
- Ruby
- railsontherun.com
- blog-post
tags:
- charts
- gem
- google charts
---

To celebrate the relaunch of this site and since we are waiting for Rails 3.0 beta to be released, I figured I should share with you what I worked on the other night.

I merged patches, refactored and released a new version of googlecharts, my Gem to create graphs using Google Chart API.

`sudo gem install googlecharts`

Here is a quick example of how the API works when dealing with a complex graph:

    
    require 'gchart' # or require 'googlecharts' if you prefer to use the Googlecharts constant.
    title = "Player Count"
    size = "575x300"
    data = [85,107,123,131,155,172,173,189,203,222,217,233,250,239,256,267,247,261,275,295,288,305,322,307,325,347,331,346,363,382,343,359,383,352,374,393,358,379,396,416,377,398,419,380,409,426,453,432,452,465,436,460,480,440,457,474,501,457,489,507,347,373,413,402,424,448,475,488,513,475,507,530,440,476,500,518,481,512,531,367,396,423,387,415,446,478,442,469,492,463,489,508,463,491,518,549,503,526,547,493,530,549,493,520,541,564,510,535,564,492,512,537,502,530,548,491,514,538,568,524,548,568,512,533,552,577,520,545,570,516,536,555,514,536,566,521,553,579,604,541,569,595,551,581,602,549,576,606,631,589,615,650,597,624,646,672,605,626,654,584,608,631,574,597,622,559,591,614,644,580,603,629,584,615,631,558,591,618,641,314,356,395,397,429,450,421,454,477,507,458,490,560,593]
    Gchart.line(:title => title, :size => size, :data => data, :axis_with_labels => 'x,y', :line_color => '1e60cc', :axis_labels => [(1.upto(24).to_a << 1)], :max_value => 700, :custom => 'chg=10,15,1,0')
    


Which provides you with the url or image tag (or downloaded file) that produces the following graph:

![Google Chart](http://chart.apis.google.com/chart?chxl=0:|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19|20|21|22|23|24|1&chxt=x,y&chco=1e60cc&chg=10,15,1,0&chd=s:HJKLNPPQRTTUWVWXVXYaZbcbcedeghefhfhifhjkhjlhklomopmoqmopsorsehkjlnqrtqsumqstqtvgjliknqnprprsprtwsuwruwruvxtvxrtvsuwrtvyuwytvwzuwytvxtvyuwz1vy0wz1wz130250357135z13y03x025z13z23x024bfijlnloqsorx0&chtt=Player+Count&cht=lc&chs=575x300&chxr=1,85,700)

This release works great with Ruby 1.9 and [MacRuby](http://macruby.org), lots of bugs got fixed and some new features were added. Something a lot of people complained was that the gem was called googlecharts but that the main class was called Gchart. The problem was that I wrote my gem and called it Gchart and when I went to register the rubyforge project page, the name was already taken. In this release, I fixed this problem by allowing users to require and use the constant name they want, Gchart or Googlecharts. I also spent quite a lot of time cleaning up the old code which I was a bit ashamed of. Class variables are now removed and overall, the code should be a bit more sane.

The source code can be found in my [GitHub accout](http://github.com/mattetti/googlecharts/) and the documentation [there](http://mattetti.github.com/googlecharts/).
