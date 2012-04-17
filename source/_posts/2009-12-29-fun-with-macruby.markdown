---
date: '2009-12-29 19:09:45'
layout: post
legacy_url: http://merbist.com/2009/12/29/fun-with-macruby/
slug: fun-with-macruby
source: merbist.com
status: publish
title: Fun with MacRuby
wordpress_id: '653'
categories:
- macruby
- merbist.com
- blog-post
tags:
- foundation
- macruby
---

To be ready for 2010, I'm taking some time off relaxing and spending time with my family in Florida.

During my free time, I've been reading, catching up on movies and TV shows and worked on the MacRuby book that I am writing for O'Reilly.

I wrote a bunch of small apps, played with various APIs and every single time I was amazed by all the goodies Apple makes available to developers. My most recent discovery is very simple but I wanted to share it with you.

I often type text in English, French and Spanish and I even mix the languages from time to time. SnowLeopard comes with a great spellchecker that auto detects the language I'm typing in and is most of the time correct. It's a very impressive feature and I was wondering if, as a MacRuby developer, I could use one of Apple's lib to detect what language is being used.  I dug through the documentation but didn't find anything. I started looking at some header files and found the API to use :)

    
    framework 'Foundation'
    class String
      def language
        CFStringTokenizerCopyBestStringLanguage(self, CFRangeMake(0, self.size))
      end
    end
    
    puts "Bonne année!".language
    # => "fr"
    puts "Happy new year!".language
    # => "en"
    puts "¡Feliz año nuevo!".language
    # => "es"
    puts "Felice anno nuovo!".language
    # => "it"
    puts "أعياد سعيدة".language
    # => "ar"
    puts "明けましておめでとうございます。".language
    # => "ja"


The documentation says that the result is not guaranteed to be accurate and that typically 200-400 characters are required to reliably guess the language of a string. ([CFStringTokenizer Doc](http://developer.apple.com/mac/library/documentation/CoreFoundation/Reference/CFStringTokenizerRef/Reference/reference.html#//apple_ref/c/func/CFStringTokenizerCopyBestStringLanguage))

Probably not the most useful piece of code, but really cool none the less :)

Happy new year!
