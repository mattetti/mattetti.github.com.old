---
date: '2007-07-02 07:39:00'
layout: post
legacy_url: http://railsontherun.com/2007/07/02/globalite-sample-application/
slug: globalite-sample-application
source: railsontherun.com
status: publish
title: Globalite - sample application
wordpress_id: '44'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- example
- globalite
- i18n
- internationalization
- l10n
- localization
- rails
- sample
---

I recently received quite a lot of love because of Globalite. Even though I tried to write some documentation about the plugin, I know that there's not much. I also know that there's a difference between showing how to set a locale and how to actually use that code in a real application.





To fill this gap I wrote a very simple and rough Rails sample application with a fully localized User Interface:





![UI Localization sample app](http://farm2.static.flickr.com/1347/691265267_bbc167f7ab.jpg?v=0)





Looking at the code you will find out how to:







  * use a different css per locale (language/country)


  * use a different picture per locale


  * set a default css/picture if the translator doesn't specify one


  * UI localization


  * rails core localization examples


  * create a select box of available UI translations


  * get/set the locale only a specific user so other users start by seeing the default translation


  * use a translation with many dynamic arguments


  * nested translation





note: I froze Edge for this sample app (it wasn't required, Globalite works well with older version of Rails). 





To install and run the app, just do the following:




    
    <code>$ svn checkout http://globalite.googlecode.com/svn/sample/ui globalite-sample-app
    </code>





edit config/database.example and rename it config/database.yml




    
    <code>$ rake db:create
    
    $ rake db:migrate
    
    $ ruby script/server
    </code>





Don't hesitate to [leave a comment](http://www.railsontherun.com/2007/7/2/globalite-sample-application#comments), submit a Rails core localization file in your language or [submit a bug](http://code.google.com/p/globalite/issues/list)
