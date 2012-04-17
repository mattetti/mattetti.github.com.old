---
date: '2010-10-19 00:05:37'
layout: post
slug: macruby-webkit-and-js
status: publish
title: MacRuby, WebKit and JS
wordpress_id: '824'
categories:
- blog-post
- macruby
---

I was working on a piece of code using MacRuby, Webkit and JavaScript. Calling JS from MacRuby is really straight forward but calling Ruby from JS is a but tricky. There is actually a known bug in MacRuby which was giving me a hard time. The bug should be fixed in 0.8 if everything goes according to plan. In the mean time here is a quick run down:

The JS bridge only works when using WebKit so we need to create a tiny browser to test our code. What we are going to do is to make an object available via JS and also trigger some JS to test the bridge both ways. Here is the full code:



On the object we want to make available via JS (instance of Cat), we have to make the methods available by defining def self.isSelectorExcludedFromWebScript(sel); false end

To trigger JS from Ruby, we use #evaluateWebScript on windowScriptObject. In our example we are using JQuery since it's already loaded in the DOM. We also go full loop by printing out the result of JS calling a method on our Ruby object.

Here is the thing, MacRuby doesn't have the age method compiled/registered yet so JS can't call it. To fix this problem we force the registration of the method by doing: @kitty.respondsToSelector("age").  Note that if #age was taking arguments, we would have to use @kitty.respondsToSelector("age:") and then evaluate the JS like that: @js_engine.evaluateWebScript('animal.age_(12)')

Hopefully by the time you need to do something like that, MacRuby 0.8 will be released and you won't have to worry about that :)

For more information about calling Obj-C/Ruby from JavaScript, [read this doc](http://developer.apple.com/library/mac/#documentation/AppleApplications/Conceptual/SafariJSProgTopics/Tasks/ObjCFromJavaScript.html#//apple_ref/doc/uid/30001215-BBCBFJCD).
