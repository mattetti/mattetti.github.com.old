---
date: '2011-02-19 16:29:07'
layout: post
legacy_url: http://merbist.com/2011/02/19/bridgesupport-build/
slug: bridgesupport-build
source: merbist.com
status: publish
title: Automatically generating BridgeSupport files
wordpress_id: '974'
categories:
- macruby
- merbist.com
- blog-post
tags:
- BridgeSupport
- macruby
- objective-c
---

Today I was helping someone write an Objective-C framework around [cocos2d](http://cocos2d.org/).

C/Objective-C code can be called directly from MacRuby. However the Obj-C code you would like to use might be using some ANSI C symbols that are non-object-oriented items such as constants, enumerations, structures, and functions. To make these items available to our MacRuby code, you need to generate a [BridgeSupport file as explained in this section of my book](http://ofps.oreilly.com/titles/9781449380373/ch03.html#_using_objective_c_or_c_code).

In our case, we were working on the framework and I didn't feel like manually having to regenerate the BridgeSupport file every single time I would compile our code. So instead I added a new build phase in our target.





[caption id="" align="aligncenter" width="741" caption="Adding a new step to our build"][![](https://img.skitch.com/20110220-b685ag2cef8qm69uwn73e3equ4.png)](https://img.skitch.com/20110220-b685ag2cef8qm69uwn73e3equ4.png)[/caption]

And I added the following script to run at the end of the build:

`
# This step generated the bridgesupport file for the framework
PATH="$PATH:/usr/local/bin"
mkdir -p $TARGET_BUILD_DIR/$PROJECT_NAME.framework/Resources/BridgeSupport/
gen_bridge_metadata --64-bit -f $TARGET_BUILD_DIR/$PROJECT_NAME.framework/ -o $TARGET_BUILD_DIR/$PROJECT_NAME.framework/Resources/BridgeSupport/$PROJECT_NAME.bridgesupport
`

Th`e script just executes the steps required to add the BridgeSupport file to your framework. I can now rebuild my framework without having to worry about BridgeSupport.
`
