---
date: '2009-10-07 20:45:02'
layout: post
legacy_url: http://merbist.com/2009/10/07/macruby-0-5-beta-1-and-textorize/
slug: macruby-0-5-beta-1-and-textorize
source: merbist.com
status: publish
title: MacRuby 0.5 beta 1 and Textorize
wordpress_id: '592'
categories:
- macruby
- merbist.com
- blog-post
tags:
- macgem
- macruby
- release
- rubygems
- textorize
---

Good news everyone!



MacRuby beta 1 has been released! [Official announcement here](http://www.macruby.org/blog/2009/10/07/macruby05b1.html).

[caption id="" align="aligncenter" width="144" caption="Download MacRuby 0.5 beta1"][![Download MacRuby 0.5 beta1](http://www.macruby.org/images/zip.png)](http://www.macruby.org/files/MacRuby%200.5%20beta%201.zip)[/caption]

Note that the download is only for SnowLeopard, intel machines.

Lots of great stuff in this new release, the first one based on LLVM. Check the [Laurent's post](http://www.macruby.org/blog/2009/10/07/macruby05b1.html) to learn more about the work done on compilation, optimization, concurrency, compatibility and Cocoa interaction. And a big thank you to [Laurent Sansonetti](http://www.youtube.com/watch?v=BVgM7qeAlko) who is putting so much effort in this project!

However, don't forget it's still a beta release and you might encounter bugs. Feel free to report them in the bug tracker or ask on the mailing list.

On a different topic, the other day, John Gruber from [Daring Fireball](http://daringfireball.net/) [wrote a quick note](http://daringfireball.net/linked/2009/09/30/textorize) about [Thomas Fuchs' textorize script](http://mir.aculo.us/2009/09/29/textorize-pristine-font-rendering-for-the-web/) which since got its own place on the internet [http://textorize.org/](http://textorize.org/).

Textorize is a Ruby-based font rasterizer command line utility for Mac OS X. It generates PNG files from an input string and options, using Mac OS X's pristine typography facilities. As John said, it's a case where a few lines of Ruby code beat Photoshop.

Thomas version is based on RubyCocoa which is great... but not MacRuby.

To celebrate[ MacRuby 0.5 beta1](http://www.macruby.org/blog/2009/10/07/macruby05b1.html), I ported the gem over and pushed it to the excellent [gemcutter.org facility](http://gemcutter.org/).

After installing MacRuby beta, follow these directives:

    
    $ macgem sources -a http://gemcutter.org
    $ sudo macgem install textorize-mr
    $ textorize -f"Didot" -s200 "MacRuby 0.5b1"
    $ open output.png


And you will get a [subpixel antialiased](http://en.wikipedia.org/wiki/Subpixel_antialiasing) fancy bitmap like that:

![macruby05b1](http://merbist.com/wp-content/uploads/2009/10/macruby05b1.png)

Check [http://textorize.org/](http://textorize.org/) for more examples and [http://github.com/mattetti/textorize](http://github.com/mattetti/textorize) for the source code.
