---
date: '2009-10-06 09:23:44'
layout: post
legacy_url: http://merbist.com/2009/10/06/macruby-tips-embed-a-custom-font/
slug: macruby-tips-embed-a-custom-font
source: merbist.com
status: publish
title: 'MacRuby tips: embed a custom font'
wordpress_id: '561'
categories:
- macruby
- merbist.com
- blog-post
tags:
- cocoa
- CTFontManager
- CTFontManagerRegisterFontsForURL
- embedded
- font
- macruby
- ruby
---

Let say you want to release your MacRuby app and use a custom embedded font?
You probably don't want to force your users to install the font.
Well, don't worry, just put the font file in your resources folder and use the following code:

    
    font_location = NSBundle.mainBundle.pathForResource('MyCustomFont', ofType: 'ttf')
    font_url = NSURL.fileURLWithPath(font_location)
    # in MacRuby, always make sure that cocoa constants start by an uppercase
    CTFontManagerRegisterFontsForURL(font_url, KCTFontManagerScopeProcess, nil)


That's it, now your custom font is available and you can change your textfield instance's font like that:

    
    text_field.font = NSFont.fontWithName('MyCustomFont', size:24)


The only tricky things here were to know the Cocoa API call and to know that even if the Cocoa API references the constant to use as kCTFontManagerScopeProcess, because in Ruby, constants start by an uppercase, you need to convert it to: KCTFontManagerScopeProcess.
